---
layout: docwithnav
title: 动作节点
description: 规则引擎2.0动作节点

---

动作节点根据传入的消息执行各种动作。

* TOC
{:toc}

# 创建警报节点（Create Alarm Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-create-alarm.png)

该节点尝试加载为消息发起者配置的 **Alarm Type** 最新警报。
如果存在 **Uncleared** 警报, 则将更新此警报, 否则将创建新的警报。

节点配置:

- **Alarm Details Builder** 脚本
- **Alarm Type** - 代表警报类型的任何字符串
- **Alarm Severity** - {CRITICAL \| MAJOR \| MINOR \| WARNING \| INDETERMINATE}
- **是否传播** - 是否将警报传播到所有与父相关的实体。

注意：自TB版本2.3.0起，规则节点具有以下功能：

-  从消息中读取警报配置：

-  使用带有消息元数据中字段的模式来获取警报类型：

    ![image](/images/user-guide/rule-engine-2-0/nodes/action-create-alarm-config-from-msg.png)
  
注意：自TB版本2.4.3起，规则节点具有以下功能：

- 按关系类型过滤传播到父实体：

    ![image](/images/user-guide/rule-engine-2-0/nodes/action-create-alarm-propagate-list.png)

**Alarm Details Builder** 用于生成警报详细信息JsonNode. 这对于在Alarm内部存储其他参数很有用。例如，您可以从原始消息payload或Metadata中保存属性名称/值对。

**Alarm Details Builder** 脚本应返回 **details**对象。
 
![image](/images/user-guide/rule-engine-2-0/nodes/action-create-alarm-config.png)

- 消息payload可以通过<code>msg</code>变量访问。例如<code>msg.temperature</code><br/> 
- 可以通过<code>metadata</code>变量访问消息。例如<code>metadata.customerName</code><br/> 
- 可以通过<code>msgType</code>变量访问。例如<code>msgType</code><br/> 

**可选:** 可以通过访问先前的警报详细信息 <code>metadata.prevAlarmDetails</code>. 
如果先前的警报不存在，则该字段将不会出现在元数据中. **注意** 这个<code>metadata.prevAlarmDetails</code> 
是一个原始的String字段，需要使用以下结构将其转换为对象:
{% highlight javascript %}
var details = {};
if (metadata.prevAlarmDetails) {
    details = JSON.parse(metadata.prevAlarmDetails);
}
{% endhighlight %}

**Alarm Details Builder** 可以使用[Test JavaScript function](/docs/user-guide/rule-engine-2-0/overview/#test-javascript-functions)来验证警报详细信息生成器脚本功能。
 
**Details Builder 功能示例**

<code>count</code> 会从警报中获取属性并进行递增。最后将 <code>temperature</code>消息payload的属性放入警报详细信息。

{% highlight javascript %}
var details = {temperature: msg.temperature, count: 1};

if (metadata.prevAlarmDetails) {
    var prevDetails = JSON.parse(metadata.prevAlarmDetails);
    if(prevDetails.count) {
        details.count = prevDetails.count + 1;
    }
}

return details;
{% endhighlight %}


**:**使用以下属性创建/更新警报

- Alarm details - object returned from 从**Alarm Details Builder** 脚本中返回对象
- Alarm status - 如果是 **new alarm** -> *ACTIVE_UNACK*. 如果是 **existing Alarm** -> 不改变
- Severity - 节点中配置的值
- Propagation - 节点中配置的值
- Alarm type - 节点中配置的值
- Alarm start time - 如果是 **new alarm** -> *当前系统时间*. 如果是 **existing Alarm** -> 不改变
- Alarm end time - *当前系统时间*

**出站消息具有以下结构:**

- **Message Type** - *警报*
- **Originator** - 入站消息具有相同的消息发起者
- **Payload** - 以JSON的形创建和更新警报
- **Metadata** -原始消息中的所有Metadata字段  

创建警报后出站消息将在Metadata中包含**isNewAlarm**属性其值为**true**，消息将通过**Created**链传递。

现有警报更新后出站消息将在Metadata中包含**isExistingAlarm**属性其值为**true**，消息将通过**Updated**链传递。

这是出站消息的**payload**示例
{% highlight json %}
{
  "tenantId": {
    "entityType": "TENANT",
    "id": "22cd8888-5dac-11e8-bbab-ad47060c9bbb"
  },
  "type": "High Temperature Alarm",
  "originator": {
    "entityType": "DEVICE",
    "id": "11cd8777-5dac-11e8-bbab-ad55560c9ccc"
  },
  "severity": "CRITICAL",
  "status": "ACTIVE_UNACK",
  "startTs": 1526985698000,
  "endTs": 1526985698000,
  "ackTs": 0,
  "clearTs": 0,
  "details": {
    "temperature": 70,
    "ts": 1526985696000
  },
  "propagate": true,
  "id": "33cd8999-5dac-11e8-bbab-ad47060c9431",
  "createdTime": 1526985698000,
  "name": "High Temperature Alarm"
}
{% endhighlight %}

可以在[本教程中](/docs/user-guide/alarms/)找到有关Thingsboard中警报的更多详细信息

在下一个教程中，您可以看到使用该节点的真实示例:

- [创建和清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)

<br/>

# 清除警报节点（Clear Alarm Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-clear-alarm.png)

该节点加载具有为消息发起者配置的 **Alarm Type** 的最新警报，并清除警报（如果存在）。

节点配置:

- **Alarm Details Builder** 脚本
- **Alarm Type** - 代表警报类型的任何字符串

**Alarm Details Builder** 脚本用于更新警报详细信息JsonNode. 这对于在Alarm内部存储其他参数很有用。例如，您可以从原始消息payload或Metadata中保存属性名称/值对。

**Alarm Details Builder** 脚本应返回 **详细信息**对象。

![image](/images/user-guide/rule-engine-2-0/nodes/action-clear-alarm-config.png)
 
- 消息payload可以通过<code>msg</code>变量访问。例如<code>msg.temperature</code><br/> 
- 可以通过<code>metadata</code>变量访问消息。例如<code>metadata.customerName</code><br/> 
- 可以通过<code>msgType</code>变量访问。例如<code>msgType</code><br/> 
- 当前的报警信息可以通过metadata.prevAlarmDetails访问

**注意**  <code>metadata.prevAlarmDetails</code> 是一个原始字符串字段，需要使用此结构将其转换为对象:
{% highlight javascript %}
var details = {};
if (metadata.prevAlarmDetails) {
    details = JSON.parse(metadata.prevAlarmDetails);
}
{% endhighlight %}

可以使用 [Test JavaScript function](/docs/user-guide/rule-engine-2-0/overview/#test-javascript-functions)**验证警报详情构建器**脚本函数。

**Details Builder功能示例**

<code>count</code> 会从警报中获取属性并进行递增。最后将 <code>temperature</code>消息payload的属性放入警报详细信息。
{% highlight javascript %}
var details = {temperature: msg.temperature, count: 1};

if (metadata.prevAlarmDetails) {
    var prevDetails = JSON.parse(metadata.prevAlarmDetails);
    if(prevDetails.count) {
        details.count = prevDetails.count + 1;
    }
}

return details;
{% endhighlight %}

 
This Node updates Current Alarm:

- 如果已经确认警报**状态**，则将警报状态更改为**CLEARED_ACK**，否则更改为**CLEARED_UNACK**
- 为当前系统设置确切的时间
- 使用从 **Alarm Details Builder**返回的新对象更新警报细节


当警报不存在或已清除警报时，原始消息将通过**false**链传递到下一个节点。 否则，新的消息将通过**Cleared**链传递。

**出站消息的结构如下:**

- **Message Type** - *ALARM*
- **Originator** - 来自入站消息的同一发起者
- **Payload** - JSON表示清除的警报
- **Metadata** - 来自原始消息元数据的所有字段。元数据中的额外属性将被添加：**isClearedAlarm**值为**True**。

这是出站消息的**payload**示例
{% highlight json %}
{
  "tenantId": {
    "entityType": "TENANT",
    "id": "22cd8888-5dac-11e8-bbab-ad47060c9bbb"
  },
  "type": "High Temperature Alarm",
  "originator": {
    "entityType": "DEVICE",
    "id": "11cd8777-5dac-11e8-bbab-ad55560c9ccc"
  },
  "severity": "CRITICAL",
  "status": "CLEARED_UNACK",
  "startTs": 1526985698000,
  "endTs": 1526985698000,
  "ackTs": 0,
  "clearTs": 1526985712000,
  "details": {
    "temperature": 70,
    "ts": 1526985696000
  },
  "propagate": true,
  "id": "33cd8999-5dac-11e8-bbab-ad47060c9431",
  "createdTime": 1526985698000,
  "name": "High Temperature Alarm"
}
{% endhighlight %}

可在[本教程中](/docs/user-guide/alarms/)找到有关Thingsboard中警报的更多详细信息

在下一个教程中，您可以看到使用该节点的真实示例：

- [创建和清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)

<br/>

# 延迟节点（Delay Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.1</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-delay.png)

将传入消息延迟可配置的时间段。

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/action-delay-config.png)

- **Period in seconds** - 指定应暂停传入消息的时间段的值
- **Maximum pending messages** - 指定允许的最大未决消息数（已暂停消息的队列） 

当到达特定传入消息的延迟时间时，它将从挂起队列中删除并通过 **Success** 链路由到下一个节点。
  
如果将达到最大未决消息限制，则每个下一条消息将通过 **Failure** 链进行路由.  

<br/>

# 生成节点（Generator Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-generator.png)

生成具有可配置周期的消息。JavaScript函数用于生成消息.

节点配置:

- 消息生成频率以秒为单位
- 消息发起者 
- JavaScript函数将生成实际的消息。

JavaScript函数接收3个输入参数: 

- <code>prevMsg</code> - 是先前生成的消息的payload.
- <code>prevMetadata</code> - 是先前生成的消息的metadata.
- <code>prevMsgType</code> - 是先前生成的消息类型.

脚本应返回以下结构:
{% highlight java %}
{   
    msg: new payload,
    metadata: new metadata,
    msgType: new msgType 
}
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/nodes/action-generator-config.png)

结果对象中的所有字段都是可选的，如果未指定，则将从先前生成的Message中获取。

来自此节点的出站消息将是使用已配置的JavaScript函数构造的新消息。

使用[Test JavaScript function](/docs/user-guide/rule-engine-2-0/overview/#test-javascript-functions)来验证JavaScript生成器功能。

该节点可用于规则链调试

<br/>

# 日志节点（Log Node ）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-log.png)

使用配置好的JavaScript函数将传入消息转换为String并将最终值记录到Thingsboard日志文件中。

**INFO** 日志级别用于记录.

JavaScript函数接收3个输入参数

- <code>metadata</code> - 消息metadata.
- <code>msg</code> - 消息payload.
- <code>msgType</code> - 消息类型.

脚本应返回字符串值。

![image](/images/user-guide/rule-engine-2-0/nodes/action-log-config.png)

JavaScript转换功能可以使用[Test JavaScript function](/docs/user-guide/rule-engine-2-0/overview/#test-javascript-functions)进行验证。

在下一个教程中，您可以看到使用该节点的真实示例：

- [回复RPC调用](/docs/user-guide/rule-engine-2-0/tutorials/rpc-reply-tutorial.md#log-unknown-request)

# RPC呼叫回复节点（RPC Call Reply Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-rpc-call-reply.png)

将响应发送到RPC调用发起方。所有传入的RPC请求都通过规则链作为消息传递。同样，所有RPC请求都具有请求ID字段。它用于映射请求和响应。消息发起者必须是**Device**实体，因为已向消息发起者发起了RPC响应。

节点配置具有特殊的请求ID字段映射。如果未指定映射，则默认使用**requestId**元数据字段。

![image](/images/user-guide/rule-engine-2-0/nodes/action-rpc-call-reply-config.png)

可以通过不同的传输方式接收RPC请求:

- MQTT
- HTTP
- CoAP  

消息payload示例
{% highlight json %}
{
  "method": "setGpio",
  "params": {
    "pin": "23",
    "value": 1
  }
}
{% endhighlight %}

在以下情况下，消息将通过**Failure**链进行路由：

- 入站消息发起者不是**Device**实体
- 消息元数据中不存在请求ID
- 入站消息payload为空

有关RPC如何在Thingsboard中工作的更多详细信息，请阅读R[RPC功能](/docs/user-guide/rpc/)文章。

在下一个教程中，您可以看到使用该节点的真实示例:

- [回复RPC调用](/docs/user-guide/rule-engine-2-0/tutorials/rpc-reply-tutorial.md)

# RPC呼叫请求节点（RPC Call Request Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-rpc-call-request.png)

将RPC请求发送到设备，并将响应路由到下一个Rule节点。消息发起者必须是**Device**实体，因为只能向设备发起RPC请求。

节点配置具有**Timeout**字段，用于指定等待设备响应的超时。

![image](/images/user-guide/rule-engine-2-0/nodes/action-rpc-call-request-config.png)

消息payload必须具有用于RPC请求的正确格式。它必须包含**method** 和 **params**字段。

例：

{% highlight json %}
{
  "method": "setGpio",
  "params": {
    "pin": "23",
    "value": 1
  }
}
{% endhighlight %}

如果消息Payload包含**requestId** 字段，则其值用于标识对设备的RPC请求。否则将生成随机的requestId。

出站消息将具有与入站消息相同的发起者和元数据。来自设备的响应将添加到消息Payload中.

在以下情况下，消息将通过**Failure**链进行路由：

- 入站消息发起者不是**Device**实体
- 入站消息丢失了**method**或**params**字段
- 节点在配置超时期间不接收响应
 
否则消息将通过**Success**链路由

有关RPC如何在Thingsboard中工作的更多详细信息，请阅读[RPC功能](/docs/user-guide/rpc/)文章.

<br/>

# 保存属性节点（Save Attributes Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-save-attributes.png)

将来自传入消息payload的属性存储到数据库中，并将它们与消息发起者标识的实体相关联。配置的**范围**用于标识属性范围

支持的范围类型:

- 客户端属性
- 共享属性
- 服务端属性

![image](/images/user-guide/rule-engine-2-0/nodes/action-save-attributes-config.png)

预期消息类型为**POST_ATTRIBUTES_REQUEST**.
如果消息类型不是**POST_ATTRIBUTES_REQUEST**, 消息将通过f**Failure**链路由. 

当属性通过现有API (HTTP / MQTT / CoAP /等)上传时 如果消息类型payload和均满足预期，那么消息将被传递到**Root Rule Chain**的**Input**节点.

如果需要在规则链中触发属性存储，则需要配置规则链，来将消息Payload转换为预期格式，并将消息类型设置为 **POST_ATTRIBUTES_REQUEST**. 可以使用[**脚本转换节点**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#script-transformation-node)完成。

**预期消息Payload的例子:**
{% highlight json %}
{
  "firmware_version": "1.0.1",
  "serial_number": "SN-001"
}
{% endhighlight %}

成功保存属性后，原始消息将通过**Success**链传递到下一个节点，否则将使用**Failure**链。

<br/>

# 保存时间序列节点（Save Timeseries Node ）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-save-timeseries.png)

将来自传入消息的payload时间序列数据存储到数据库，并将它们与消息发起者标识的实体相关联。配置的**TTL**秒用于时间序列数据到期。**0**值表示数据永不过期

![image](/images/user-guide/rule-engine-2-0/nodes/action-save-timeseries-config.png)

预期消息类型为**POST_TELEMETRY_REQUEST**。如果消息类型不是**POST_TELEMETRY_REQUEST**，则将通过**Failure**链路由消息。
 
当timeseries数据通过现有的API (HTTP / MQTT / CoAP /等等)发布时，如果消息类型和payload均满足预期，那么消息将被传递到**Root Rule Chain**链的**Input**节点。

在需要触发规则链内的timeseries数据存储时，应配置规则链来转换消息有效负载 将消息类型设置为**POST_TELEMETRY_REQUEST**。

可以使用 [**脚本转换节点**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#script-transformation-node)完成该操作。 

消息元数据必须包含**ts**字段。

该字段可识别已发布的遥测技术几毫秒内的时间戳。 


此外，如果消息元数据包含**TTL**字段，则使用该字段值识别timeseries数据期限，否则，请使用节点配置中的**TTL**。

**预期消息Payload的例子:**

{% highlight json %}
{  
  "values": {
    "key1": "value1",
    "key2": "value2"
  }
}
{% endhighlight %}

成功保存timeseries数据之后，原始消息将通过**Success**链传递到下一个节点，否则使用**Failure**链。

<br/>

# 保存到自定义表（Save to Custom Table）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.3.1+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-save-to-custom-cassandra-table.png)

节点将来自传入消息payload的数据存储到**Cassandra DB**数据库中，存储到应该具有**cs_tb_**_前缀的预定义定制表中，以避免将数据插入到公共TB表中。

请注意，该规则节点只能用于**Cassandra DB**

配置:

管理员应设置不带前缀的自定义表名称: **cs_tb_**.

![image](/images/user-guide/rule-engine-2-0/nodes/action-save-to-custom-cassandra-table-name-config.png)

管理员可以配置消息字段名称和表的列名称之间的映射。如果映射键为 **$entity_id** ，由消息发起者标识，则将消息发起者id写入适当的列名（映射值）。

![image](/images/user-guide/rule-engine-2-0/nodes/action-save-to-custom-cassandra-table-config.png)

If specified message field does not exist or is not a JSON Primitive, the outbound message will be routed via **Failure** chain, otherwise, the message will be routed via **Success** chain.
如果指定的消息字段不存在或不是JSON原语，则出站消息将通过Failure链进行路由，否则，消息将通过**Success**链进行路由。

<br/>

# 分配给客户节点（Assign To Customer Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.2+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-assign-to-customer-node.png)

将消息发起者实体分配给[Customer](/docs/user-guide/ui/customers/). 

支持以下消息发起者类型: **Asset**, **Device**, **Entity View**, **Dashboard**.

通过客户名称模式查找目标客户，然后将发起者实体分配给该客户.

如果不存在（**Create new Customer if not exists**），将创建新客户，如果不存在，将创建新客户设置为**true**

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/action-assign-to-customer-node-configuration.png)

- **Customer name pattern** - 可以设置直接的客户名称，也可以使用模式，将使用消息元数据将其解析为真实的客户名称.
- **Create new customer if not exists** - 则创建新客户-如果选中，则将创建不存在的新客户.
- **Customers cache expiration time** - 指定允许存储找到的客户记录的最大时间间隔（以秒为单位）。0值表示记录永不过期。

在以下情况下，消息将通过**Failure**链进行路由：

- 不支持Originator实体类型时.
- 目标客户不存在，未选中创建客户如果不存在（**Create customer if not exists**）

在其他情况下，消息将通过**Success**链进行路由

<br/>

# 从客户节点取消分配（Unassign From Customer Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.2+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-unassign-from-customer-node.png)

从[Customer](/docs/user-guide/ui/customers/)取消分配消息发起者实体

支持以下消息发起者类型: **Asset**, **Device**, **Entity View**, **Dashboard**.

通过客户名称模式查找目标客户，然后从该客户取消分配发起者实体。

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/action-unassign-from-customer-node-configuration.png)

- **Customer name pattern** - 可以设置直接的客户名称，也可以使用模式，将使用消息元数据将其解析为真实的客户名称。
- **Customers cache expiration time** - 指定允许存储找到的客户记录的最大时间间隔（以秒为单位）。0值表示记录永不过期。

在以下情况下，消息将通过**Failure**链进行路由：

- 不支持Originator实体类型时.
- 目标客户不存在.

在其他情况下，消息将通过**Success**链进行路由。

<br/>

# 创建关系节点（Create Relation Node） 

<table  style="min-width:12%; max-width: 20%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.2.1+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-create-relation.png)

 按类型和方向创建从所选实体到消息发起者的关系。

 支持以下消息发起者类型: **Asset**, **Device**, **Entity View**, **Customer**, **Tenant**, **Dashboard**.

通过元数据键模式查找目标实体，然后在发起方实体和目标实体之间创建关系.

如果选择的实体类型为 **Asset**, **Device** 或 **Customer**  规则节点: 则如果不存在则创建新的Entity并选中复选框**Create new Entity if not exists**.

**注意:** 如果选择的实体类型为 **Asset** 或 **Device** 则需要设置两种模式: 

 - 实体名称模式; 
 
 - 实体类型模式. 

否则，仅应设置名称模式.

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/action-create-relation-node-configuration.png)

- **Direction** - 允许下列类型: **From**, **To**.
- **Relation type** - 与消息始发者实体的定向连接的类型。可以从下拉列表中选择默认类型 **Contains** 和 **Manages** 。
- **Name pattern** and **Type pattern** - 可以设置直接实体名称/类型，也可以使用模式，使用消息元数据将其解析为真实实体名称/类型。
- **Entities cache expiration time** - 指定允许存储找到的目标实体记录的最大时间间隔（以秒为单位）。0值表示记录永不过期。

在以下情况下，消息将通过 **Failure** 链进行路由:

- 不支持Originator实体类型时.
- 目标实体不存在.

在其他情况下，消息将通过**Success**链进行路由

**注意:** 自TB 2.3版以来，规则节点具有以下功能:

 - 根据方向和类型，从传入消息的发起者中删除当前关系: 

    ![image](/images/user-guide/rule-engine-2-0/nodes/action-create-relation-node-remove-relations.png)

 - 将传入消息的始发者更改为所选实体，并将出站消息作为来自另一个实体的消息进行处理: 
 
    ![image](/images/user-guide/rule-engine-2-0/nodes/action-create-relation-node-change-originator.png)

<br/>

# 删除关系节点（Delete Relation Node）

<table  style="min-width:12%; max-width: 20%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.2.1</em></strong></td>
     </tr>
   </thead>
</table> 


![image](/images/user-guide/rule-engine-2-0/nodes/action-delete-relation.png)

按类型和方向删除所选实体与消息发起者之间的关系。

支持以下消息发起者类型: **Asset**, **Device**, **Entity View**, **Customer**, **Tenant**, **Dashboard**。

通过实体名称模式查找目标实体，然后删除发起者实体与该实体之间的关系。

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/action-delete-relation-node-configuration.png)

- **Direction** - 允许下列类型: **From**, **To**.
- **Relation type** - 与消息始发者实体的定向连接的类型。可以从下拉列表中选择默认类型 **Contains** 和 **Manages** 。
- **Name pattern** and **Type pattern** - 可以设置直接实体名称/类型，也可以使用模式，使用消息元数据将其解析为真实实体名称/类型。
- **Entities cache expiration time** - 指定允许存储找到的目标实体记录的最大时间间隔（以秒为单位）。0值表示记录永不过期。

在以下情况下，消息将通过 **Failure** 链进行路由:

- 不支持Originator实体类型时。
- 目标实体不存在。

在以下情况下，消息将通过**Success**链进行路由。 


**注意:** 自TB版本2.3起，规则节点可以通过禁用规则节点配置中的以下复选框来根据方向和类型从传入消息的始发者删除与指定实体或与实体列表的关系:

![image](/images/user-guide/rule-engine-2-0/nodes/action-delete-relation-node-new-functionality.png)

<br/>

# GPS地理围栏事件节点（GPS Geofencing Events Node）

<table  style="width:15%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.3.1</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/action-gps-geofencing-event-node.png)

通过基于GPS的参数生成传入消息。从传入的消息数据或元数据中提取纬度和经度，并根据配置参数（地理围栏）返回不同的事件。

![image](/images/user-guide/rule-engine-2-0/nodes/filter-gps-geofencing-default-config.png)

默认情况下，规则节点从消息元数据中获取外围信息。如果未选中从 **Fetch perimeter information from message metadata**，则应配置其他信息。

<br>

###### Fetch perimeter information from message metadata

根据边界类型，有两种区域定义选项:

- 多边形 
           
    传入消息的元数据必须包含具有名称**perimeter**和以下数据结构的密钥
     
{% highlight java %}[[lat1,lon1],[lat2,lon2], ... ,[latN,lonN]]{% endhighlight %}
 
- 圆形
                 
{% highlight java %}"centerLatitude": "value1", "centerLongitude": "value2", "range": "value3"

All values for these keys are in double-precision floating-point data type.

The "rangeUnit" key requires specific value from a list of METER, KILOMETER, FOOT, MILE, NAUTICAL_MILE (capital letters obligatory).
{% endhighlight %}

###### Fetch perimeter information from node configuration
 
根据边界类型，有两种区域定义选项:
 
- 多边形 
             
![image](/images/user-guide/rule-engine-2-0/nodes/filter-gps-geofencing-polygon-config.png)           

- 圆形
                  
![image](/images/user-guide/rule-engine-2-0/nodes/filter-gps-geofencing-circle-config.png)       

###### 活动类型
地理围栏规则节点管理的事件有4种:

- **Entered** — -每当传入消息的纬度和经度首次属于所需的外围区域时报告;
- **Left** — 第一次报告来自传入消息的纬度和经度不属于所需的外围区域时;
- **Inside** 和 **Outside** 事件用于报告当前状态.

管理员可以配置持续时间阈值，以报告内部事件或外部事件。例如，每当最小内部时间设置为1分钟时，消息发件人就被视为进入该区域后60秒之内。最少的外部时间也定义了何时将消息发起者视为不在外围范围之内。

![image](/images/user-guide/rule-engine-2-0/nodes/action-gps-geofencing-event-node-duration-config.png)  
 
在以下情况下将使用**Failure**链:

   - 传入消息在数据或元数据中没有配置的纬度（latitude）或经度（longitude）键。
   - 缺少周界定义;     
    