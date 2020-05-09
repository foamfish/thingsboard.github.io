---
layout: docwithnav
title: 规则引擎概述
description: 规则引擎概述

---

* TOC
{:toc}

ThingsBoard规则引擎是用于复杂事件处理的高度可定制和可配置的系统。使用规则引擎，您可以过滤，丰富和转换由IoT设备和相关资产发出的传入消息。您还可以触发各种操作，例如，通知或与外部系统的通信。
  
## 关键概念

#### 规则引擎消息

规则引擎消息是可以被序列化的有着规定的数据结构，表示系统中的各种消息。例如：

  * 对来自设备传入的[遥测](/docs/user-guide/telemetry/),[属性更新](/docs/user-guide/attributes/)或[RPC调用](/docs/user-guide/rpc/) from device;
  * 实体生命周期事件: created, updated, deleted, assigned, unassigned, attributes updated;
  * 设备状态事件: connected, disconnected, active, inactive, etc;
  * 其他系统事件。
  
规则引擎消息包含以下信息:

  * 消息ID（Message ID）：基于时间的通用唯一标识符;
  * 消息发起者（Originator of the message）：Device，Asset或其他[Entity](/docs/user-guide/entities-and-relations/)标识符;
  * 消息类型（Type of the message）：遥测或不活动的事件等;
  * 消息负载（Payload of the message）：带有实际消息有效payload的JSON正文;
  * 元数据（Metadata）键值对的列表以及与消息有关的其他数据. 

##### 预定义的消息类型

下表列出了预定义的消息类型：

<table>
  <thead>
      <tr>
          <td><b>消息类型</b></td><td><b>显示名称</b></td><td><b>描述</b></td><td><b>消息元数据</b></td><td><b>Message payload</b></td>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td>POST_ATTRIBUTES_REQUEST</td>
          <td><b>发布属性</b></td>
          <td>请求从设备发布到 <a href="/docs/user-guide/attributes/#attribute-types">客户端属性</a> (请参见 <a href="/docs/reference/mqtt-api/#publish-attribute-update-to-the-server">属性api</a> 获取参考)</td>
          <td><b>deviceName</b> - 设备原始名称,<br><b>deviceType</b> - 设备原始类型</td>
          <td>key/value json: <br> <code style="font-size: 12px;">{ <br> &nbsp;&nbsp;"currentState": "IDLE" <br> }</code></td>
      </tr>
      <tr>
          <td>POST_TELEMETRY_REQUEST</td>
          <td><b>Post telemetry</b></td>
          <td>Request from device to publish telemetry (see <a href="/docs/reference/mqtt-api/#telemetry-upload-api">telemetry upload api</a> for reference)</td>
          <td><b>deviceName</b> - 设备原始名称,<br><b>deviceType</b> - 设备原始类型,<br><b>ts</b> - timestamp (milliseconds)</td>
          <td>key/value json: <br> <code style="font-size: 12px;">{ <br> &nbsp;&nbsp;"temperature": 22.7 <br> }</code></td>
      </tr>
      <tr>
          <td>TO_SERVER_RPC_REQUEST</td>
          <td><b>RPC Request from Device</b></td>
          <td>RPC request from device (see <a href="/docs/reference/mqtt-api/#client-side-rpc">client side rpc</a> for reference)</td>
          <td><b>deviceName</b> - 设备原始名称,<br><b>deviceType</b> - 设备原始类型,<br><b>requestId</b> - RPC request Id provided by client</td>
          <td>json containing <b>method</b> and <b>params</b>: <br> <code style="font-size: 12px;">{ <br> &nbsp;&nbsp;"method": "getTime", <br>&nbsp;&nbsp;"params": { "param1": "val1" } <br> }</code></td>
      </tr>
      <tr>
          <td>RPC_CALL_FROM_SERVER_TO_DEVICE</td>
          <td><b>RPC Request to Device</b></td>
          <td>RPC request from server to device (see <a href="/docs/user-guide/rpc/#server-side-rpc-api">server side rpc api</a> for reference)</td>
          <td><b>requestUUID</b> - internal request id used by sustem to identify reply target,<br><b>expirationTime</b> - time when request will be expired,<br><b>oneway</b> - specifies request type: <i>true</i> - without response, <i>false</i> - with response</td>
          <td>json containing <b>method</b> and <b>params</b>: <br> <code style="font-size: 12px;">{ <br> &nbsp;&nbsp;"method": "getGpioStatus", <br>&nbsp;&nbsp;"params": { "param1": "val1" } <br> }</code></td>
      </tr>
      <tr>
          <td>ACTIVITY_EVENT</td>
          <td><b>Activity Event</b></td>
          <td>Event indicating that device becomes active</td>
          <td><b>deviceName</b> - originator device name,<br><b>deviceType</b> - originator device type</td>
          <td>json containing device activity information: <br> <code style="font-size: 12px;">{<br> &nbsp;&nbsp;"active": true,<br> &nbsp;&nbsp;"lastConnectTime": 1526979083267,<br> &nbsp;&nbsp;"lastActivityTime": 1526979083270,<br> &nbsp;&nbsp;"lastDisconnectTime": 1526978493963,<br> &nbsp;&nbsp;"lastInactivityAlarmTime": 1526978512339,<br> &nbsp;&nbsp;"inactivityTimeout": 10000<br>}</code></td>
      </tr>
      <tr>
          <td>INACTIVITY_EVENT</td>
          <td><b>Inactivity Event</b></td>
          <td>Event indicating that device becomes inactive</td>
          <td><b>deviceName</b> - originator device name,<br><b>deviceType</b> - originator device type</td>
          <td>json containing device activity information, see <b>Activity Event</b> payload</td>
      </tr>
      <tr>
          <td>CONNECT_EVENT</td>
          <td><b>Connect Event</b></td>
          <td>Event produced when device is connected</td>
          <td><b>deviceName</b> - originator device name,<br><b>deviceType</b> - originator device type</td>
          <td>json containing device activity information, see <b>Activity Event</b> payload</td>
      </tr>
      <tr>
          <td>DISCONNECT_EVENT</td>
          <td><b>Disconnect Event</b></td>
          <td>Event produced when device is disconnected</td>
          <td><b>deviceName</b> - originator device name,<br><b>deviceType</b> - originator device type</td>
          <td>json containing device activity information, see <b>Activity Event</b> payload</td>
      </tr>
      <tr>
          <td>ENTITY_CREATED</td>
          <td><b>Entity Created</b></td>
          <td>Event produced when new entity was created in system</td>
          <td><b>userName</b> - name of the user who created the entity,<br><b>userId</b> - the user Id</td>
          <td>json containing created entity details: <br> <code style="font-size: 12px;">{<br> &nbsp;&nbsp;"id": {<br> &nbsp;&nbsp;&nbsp;&nbsp;"entityType": "DEVICE",<br> &nbsp;&nbsp;&nbsp;&nbsp;"id": "efc4b9e0-5d0f-11e8-8559-37a7f8cdca74"<br> &nbsp;&nbsp;},<br> &nbsp;&nbsp;"createdTime": 1526918366334,<br> &nbsp;&nbsp;...<br> &nbsp;&nbsp;"name": "my-device",<br> &nbsp;&nbsp;"type": "temp-sensor"<br>}</code></td>
      </tr>
      <tr>
          <td>ENTITY_UPDATED</td>
          <td><b>Entity Updated</b></td>
          <td>Event produced when existing entity was updated</td>
          <td><b>userName</b> - name of the user who updated the entity,<br><b>userId</b> - the user Id</td>
          <td>json containing updated entity details, see <b>Entity Created</b> payload</td>
      </tr>
      <tr>
          <td>ENTITY_DELETED</td>
          <td><b>Entity Deleted</b></td>
          <td>Event produced when existing entity was deleted</td>
          <td><b>userName</b> - name of the user who deleted the entity,<br><b>userId</b> - the user Id</td>
          <td>json containing deleted entity details, see <b>Entity Created</b> payload</td>
      </tr>
      <tr>
          <td>ENTITY_ASSIGNED</td>
          <td><b>Entity Assigned</b></td>
          <td>Event produced when existing entity was assigned to customer</td>
          <td><b>userName</b> - name of the user who performed assignment operation,<br><b>userId</b> - the user Id,<br><b>assignedCustomerName</b> - assigned customer name,<br><b>assignedCustomerId</b> - Id of assigned customer</td>
          <td>json containing assigned entity details, see <b>Entity Created</b> payload</td>
      </tr>
      <tr>
          <td>ENTITY_UNASSIGNED</td>
          <td><b>Entity Unassigned</b></td>
          <td>Event produced when existing entity was unassigned from customer</td>
          <td><b>userName</b> - name of the user who performed unassignment operation,<br><b>userId</b> - the user Id,<br><b>unassignedCustomerName</b> - unassigned customer name,<br><b>unassignedCustomerId</b> - Id of unassigned customer</td>
          <td>json containing unassigned entity details, see <b>Entity Created</b> payload</td>
      </tr>
      <tr>
          <td>ADDED_TO_ENTITY_GROUP</td>
          <td><b>Added to Group</b></td>
          <td>Event produced when entity was added to <a href="/docs/user-guide/groups/">Entity Group</a>. This Message Type is specific to <a href="/products/thingsboard-pe/">ThingsBoard PE</a>.</td>
          <td><b>userName</b> - name of the user who performed assignment operation,<br><b>userId</b> - the user Id,<br><b>addedToEntityGroupName</b> - entity group name,<br><b>addedToEntityGroupId</b> - Id of entity group</td>
          <td>empty json payload</td>
      </tr>
      <tr>
          <td>REMOVED_FROM_ENTITY_GROUP</td>
          <td><b>Removed from Group</b></td>
          <td>Event produced when entity was removed from <a href="/docs/user-guide/groups/">Entity Group</a>. This Message Type is specific to <a href="/products/thingsboard-pe/">ThingsBoard PE</a>.</td>
          <td><b>userName</b> - name of the user who performed unassignment operation,<br><b>userId</b> - the user Id,<br><b>removedFromEntityGroupName</b> - entity group name,<br><b>removedFromEntityGroupId</b> - Id of entity group</td>
          <td>empty json payload</td>
      </tr>
      <tr>
          <td>ATTRIBUTES_UPDATED</td>
          <td><b>Attributes Updated</b></td>
          <td>Event produced when entity attributes update was performed</td>
          <td><b>userName</b> - name of the user who performed attributes update,<br><b>userId</b> - the user Id,<br><b>scope</b> - updated attributes scope (can be either <b>SERVER_SCOPE</b> or <b>SHARED_SCOPE</b>)</td>
          <td>key/value json with updated attributes: <br> <code style="font-size: 12px;">{ <br> &nbsp;&nbsp;"softwareVersion": "1.2.3" <br> }</code></td>
      </tr>
      <tr>
          <td>ATTRIBUTES_DELETED</td>
          <td><b>Attributes Deleted</b></td>
          <td>Event produced when some of entity attributes were deleted</td>
          <td><b>userName</b> - name of the user who deleted attributes,<br><b>userId</b> - the user Id,<br><b>scope</b> - deleted attributes scope (can be either <b>SERVER_SCOPE</b> or <b>SHARED_SCOPE</b>)</td>
          <td>json with <b>attributes</b> field containing list of deleted attributes keys: <br> <code style="font-size: 12px;">{ <br> &nbsp;&nbsp;"attributes": ["modelNumber", "serial"] <br> }</code></td>
      </tr>
      <tr>
        <td>ALARM</td>
        <td><b>Alarm event</b></td>
        <td>Event produced when an alarm was created, updated or deleted</td>
        <td> 
            All fields from original Message Metadata
            <br><b>isNewAlarm</b> - true if a new alram was just created
            <br><b>isExistingAlarm</b> - true if an alarm is existing already
            <br><b>isClearedAlarm</b> - true if an alarm was cleared
        </td>
        <td>json containing created alarm details: <br><code style="font-size: 12px;">
        {
          <br> &nbsp;&nbsp;"tenantId": {
            <br> &nbsp;&nbsp;&nbsp;&nbsp; ...
          <br> &nbsp;&nbsp;},
          <br> &nbsp;&nbsp;"type": "High Temperature Alarm",
          <br> &nbsp;&nbsp;"originator": {
            <br> &nbsp;&nbsp;&nbsp;&nbsp; ...
          <br> &nbsp;&nbsp;},
          <br> &nbsp;&nbsp;"severity": "CRITICAL",
          <br> &nbsp;&nbsp;"status": "CLEARED_UNACK",
          <br> &nbsp;&nbsp;"startTs": 1526985698000,
          <br> &nbsp;&nbsp;"endTs": 1526985698000,
          <br> &nbsp;&nbsp;"ackTs": 0,
          <br> &nbsp;&nbsp;"clearTs": 1526985712000,
          <br> &nbsp;&nbsp;"details": {
            <br> &nbsp;&nbsp;&nbsp;&nbsp;"temperature": 70,
            <br> &nbsp;&nbsp;&nbsp;&nbsp;"ts": 1526985696000
          <br> &nbsp;&nbsp;},
          <br> &nbsp;&nbsp;"propagate": true,
          <br> &nbsp;&nbsp;"id": "33cd8999-5dac-11e8-bbab-ad47060c9431",
          <br> &nbsp;&nbsp;"createdTime": 1526985698000,
          <br> &nbsp;&nbsp;"name": "High Temperature Alarm"
        <br>}
        </code>
        </td>
      </tr>
      <tr>
          <td>REST_API_REQUEST</td>
          <td><b>REST API Request to Rule Engine</b></td>
          <td>Event produced when user executes REST API call</td>
          <td><b>requestUUID</b> - the unique request id,<br><b>expirationTime</b> - the expiration time of the request</td>
          <td>json with request payload</td>
      </tr>
   </tbody>
</table>

#### 规则节点

规则节点是规则引擎的基本组件，每次处理单个传入消息并生成一个或多个传出消息。

规则节点是规则引擎的主要逻辑单元。

规则节点可以是filter, enrich, transform传入消息，执行操作或与外部系统通信。

#### 规则节点关系

规则节点可能与其他规则节点相关。每个关系都有关系类型，这是用于标识关系的逻辑含义的标签。

当规则节点生成传出消息时，它总是指定用于将消息路由到下一个节点的关系类型。

典型的规则节点关系是**Success**和**Failure**。表示逻辑运算的规则节点可以使用**True**或**False**。

一些特定的规则节点可能使用完全不同的关系类型，例如：“Post Telemetry”，“Attributes Updated”，“Entity Created”等。

#### 规则链

规则链是规则节点及其关系的逻辑组。例如，下面的规则链将：

  * 将所有遥测消息保存到数据库中;
  * 如果消息中的温度字段高于50度，则发出“高温警报”;
  * 如果消息中的温度字段低于-40度，则发出“低温警报”;
  * 如果在脚本中发生逻辑或语法错误时，则无法执行温度脚本检查控制台记录。 

![image](/images/user-guide/rule-engine-2-0/rule-node-relations.png)

租户管理员可以定义一个**Root Rule Chain**，还可以定义多个其他规则链。根规则链处理所有传入的消息，并将其转发到其他规则链以进行其他处理。

例如，下面的规则链将：

  * 如果消息中的温度字段高于50度，则发出“高温警报”；
  * 如果消息中的温度字段小于50度，则清除“高温警报”
  * 将有关“已创建”和“已清除”警报的事件转发到外部规则链，该规则链处理向相应用户的通知。
 
![image](/images/user-guide/rule-engine-2-0/rule-chain-references.png)
 
## 规则节点类型

根据其性质将所有可用规则节点分组：

  * [**Filter Nodes**](/docs/user-guide/rule-engine-2-0/filter-nodes/)用于消息过滤和路由;
  * [**Enrichment Nodes**](/docs/user-guide/rule-engine-2-0/enrichment-nodes/)用于更新传入消息的元数据;
  * [**Transformation Nodes**](/docs/user-guide/rule-engine-2-0/transformation-nodes/)用于更改传入的消息字段，例如Originator, Type, Payload, Metadata;
  * [**Action Nodes**](/docs/user-guide/rule-engine-2-0/action-nodes/)根据传入的消息执行各种动作;
  * [**External Nodes**](/docs/user-guide/rule-engine-2-0/external-nodes/)用于与外部系统进行交互.

## 配置

每一个规则节点具有特定的参数配置，例如：Filter节点可以通过自定义JS函数。External节点可以通过参数配置实现外部邮件服务器连接设置
  
可以通过在“规则链”编辑器中双击节点来打开“规则节点”配置窗口：
  
![image](/images/user-guide/rule-engine-2-0/rule-node-configuration.png)

### Javascript函数

一些规则节点具有特定的UI功能，允许用户测试JS函数。单击**Test Filter Function**后，您将看到JS编辑器，可使用该编辑器替换输入参数并验证函数的输出。

![image](/images/user-guide/rule-engine-2-0/rule-node-test-function.png)

你可以定义:

- **Message Type** 左上角.
- **Message payload** 左侧中间.
- **Metadata** 右上角.
- **JS script** 实际脚本.

点击**Test**按钮将在右侧**Output**返回值

## 调试

ThingsBoard提供了查看每个规则节点的传入和传出消息的功能。要启用调试，用户需要确保在主配置窗口中选中了“调试模式”复选框。启用调试后，只要相应的关系类型，用户就可以查看传入和传出消息的信息。（请参见上面[配置](/docs/user-guide/rule-engine-2-0/overview/#configuration)）。


启用调试后，只要相应的关系类型，用户就可以查看传入和传出消息的信息。请参阅下图，获取示例调试消息视图：:
  
![image](/images/user-guide/rule-engine-2-0/rule-node-debug.png)  

## 导入导出

您可以将规则链导出为JSON格式，并将其导入到相同或其他ThingsBoard实例。
为了导出规则链，您应该导航到**Rule Chains**页面，然后单击位于特定规则链卡上的导出按钮。
	
![image](/images/user-guide/rule-engine-2-0/rule-chain-export.png)

类似地，要导入规则链，您应该导航到**Rules Chains**页面，然后单击屏幕右下角的大“ +”按钮，然后单击导入按钮。

## 架构

要了解有关规则引擎内部的更多信息，请参阅[架构](/docs/user-guide/rule-engine-2-0/architecture/)页面.

## 自定义REST API调用规则引擎

{% assign feature = "Custom Rule Engine REST API calls" %}{% include templates/pe-feature-banner.md %}

ThingsBoard提供了将自定义REST API调用发送到规则引擎，处理请求的有效负载并在响应正文中返回处理结果的API。
这对于许多用例很有用。例如：

 
 - 通过自定义API调用扩展平台的现有REST API;
 - 利用设备/资产/客户的属性丰富REST API调用，并转发给外部系统以进行复杂处理;
 - 为您的自定义小部件提供自定义API.
 
要执行REST API调用，您可以使用规则引擎控制器[REST APIs](/docs/reference/rest-api/): 
 
![image](/images/user-guide/rule-engine-2-0/rest-api.png) 

注意：您在呼叫中指定的实体ID将是“规则引擎”消息的始发者。如果您未指定实体ID参数，则您的用户实体将成为消息的发起者。

## 教程

ThingsBoard的作者准备了一些教程，以帮助您通过示例开始设计规则链:

  * [**转换设备消息**](/docs/user-guide/rule-engine-2-0/tutorials/transform-incoming-telemetry/) 
  * [**转换设备历史消息**](/docs/user-guide/rule-engine-2-0/tutorials/transform-telemetry-using-previous-record/) 
  * [**创建并清除设备警报消息**](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)
  * [**发送设备邮件警报**](/docs/user-guide/rule-engine-2-0/tutorials/send-email/) 
  * [**设备互发消息**](/docs/user-guide/rule-engine-2-0/tutorials/rpc-reply-tutorial/)
  
点击此处查看更多教程[here](https://thingsboard.io/docs/guides/).