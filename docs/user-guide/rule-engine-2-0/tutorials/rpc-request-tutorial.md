---
layout: docwithnav
title: 将RPC请求发送到相关设备
description: 将RPC请求发送到相关设备

---

ThingsBoard允许您从服务器端应用程序向设备发送远程过程调用[**RPC**](/docs/user-guide/rpc/#server-side-rpc-api)反之亦然。 <br>
本教程将向您展示如何使用规则引擎将远程请求调用发送到相关设备。


* TOC
{:toc}

# 用例
让我们假设以下用例：

  - 您将以下设备连接到ThingsBoard：	
	- 风向(Direction)传感器
	- 旋转(Rotating)系统	
  - 另外您拥有一项asset:
	- 风力发电机
 - 您要向旋转系统发起RPC请求，并根据风向更改风力涡轮机的方向
 - RPC调用将具有两个属性:
	- 方法: **spinLeft** 或 **spinRight**
	- 参数: **value**

<table  style="width: 60%">
   <thead>
     <tr>
	 <td><strong><em>注意:</em></strong></td>
     </tr>
   </thead>
   <tbody>
     <tr>
	<td>
	<p>向左或向右旋转旋转系统是基于哪种方法更好，更快，因此风向与风力涡轮机之间的角度不得超过5度。</p>
	</td>
     </tr>
   </tbody>
</table>


## 先决条件

我们假设您已完成以下指南并查看了以下文章：

 * [入门指南](/docs/getting-started-guides/helloworld/)。
 * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
 
 
# 型号定义
风力涡轮机已安装两个设备：风向(Direction)传感器和旋转(Rotating)系统。

- 风力涡轮机表示为资产。它的名称为**Wind Turbine**和类型为**Wind turbine**。
- 风向传感器表示为设备。它的名称为**Wind Direction Sensor**和类型为**Direction Sensor**。
- 旋转系统表示为设备。它的名称为**Rotating System**和类型为**Rotating System**.
- 创建类型为**Contains**的关系*:
	- 从**Wind Turbine**到**Wind Direction Sensor**
	- 从**Wind Turbine**到**Rotating System**.
- 创建类型的关系**Uses**:
  - 从**Rotating System**到 **Wind Direction Sensor**。


# 消息流
在本节中我们将解释本教程中每个节点的用途：

- 节点 A: [**Message Type Switch**](/docs/user-guide/rule-engine-2-0/filter-nodes/#message-type-switch-node)
  - 根据消息类型路由传入的消息。
- 节点 B: [**Save Timeseries**](/docs/user-guide/rule-engine-2-0/action-nodes/#save-timeseries-node)
  - 将来自**Wind Direction Sensor**和**Rotating System**的消息遥测存储到数据库中。
- 节点 C: [**Related attributes**](/docs/user-guide/rule-engine-2-0/enrichment-nodes/#related-attributes)
  - 加载相关**Wind Direction Sensor**的遥测源**windDirection** 并将其保存到消息元数据中， 名称为**windDirection**。
- 节点 D: [**Change originator**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#change-originator) node.
  - 将originator从设备**Wind Direction Sensor**和**Rotating System**更改为相关资产**Wind Turbine** 提交的消息将作为来自资产的消息进行处理。
- 节点 E: [**Save Timeseries**](/docs/user-guide/rule-engine-2-0/action-nodes/#save-timeseries-node)
  - 将来自资产**Wind Turbine**的消息遥测存储到数据库中。
- 节点 F: [**Transformation Script**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#script-transformation-node)
  - 将原始消息转换为RPC请求消息。
- 节点 G: [**Filter Script**](/docs/user-guide/rule-engine-2-0/filter-nodes/#script-filter-node)
  - 检查传入消息的msgType是否为**RPC消息**.
- 节点 H: [**RPC call request**](/docs/user-guide/rule-engine-2-0/action-nodes/#rpc-call-request-node) node.
  - 获取消息payload并将其作为响应发送到**Rotating System**。
  

<br/>
<br/>

# 配置规则链

以下屏幕截图显示了**RPC呼叫请求教程**规则链图片：

![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/chain.png)

-下载上述规则链的json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/tutorial_of_rpc_call_request.json)并将其导入。
- 不要忘记将新规则链标记为"root"。

另外您可以从头开始创建新的规则链。下一节将向您展示如何创建它。


#### 创建新的规则链（**RPC调用请求教程**）

- 转到时**Rule Chains** -> **Add new Rule Chain** 
- 输入名称**Tutorial of RPC Call Request**并点击**ADD**按钮。

![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/create-chain.png) ![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/root-chain.png) 

- 现在创建了新的规则链。不要忘记将其标记为“root”。

##### 添加所需的节点

在本教程中您将创建8个节点，如以下各节所述：

###### 节点A: **Message Type Switch**
- 添加**Message Type Switch**并将其连接到**Input**节点 <br>
  该节点将根据消息类型**POST_TELEMETRY_REQUEST**路由传入的消息。

- 输入名称**Message Type Switch**。


![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/message-type-switch.png)

######  节 B: **Save TimeSeries**
- 添加**Save TimeSeries**节点并将其连接到**Message Type Switch**节点其关联类型为**Post telemetry**. <br>
  该节点会将来自传入消息payload的TimeSeries数据存储到数据库中，并将它们与消息发起者标识的设备关联，即**Wind Direction Sensor**和**Rotating System**。
  
- 输入名称**Save Time Series**


![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/save-ts.png)

###### 节 C: **Related attributes**
- 添加**Related attributes**节点并将其连接到关系类型为**Success**的**Save TimeSeries**的节点。<br>
  该节点从相关的**Wind Direction Sensor**的遥测源**windDirection**加载到**Rotating System**将其保存到消息metadata中。
- 填写下表中输入的数据字段：

<table style="width: 25%">
  <thead>
      <tr>
          <td><b>Field</b></td><td><b>Input Data</b></td>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td>Name</td>
          <td>Fetch Wind Sensor Telemetry</td>
      </tr>
      <tr>
          <td>Direction</td>
          <td>From</td>
      </tr>
      <tr>
          <td>Max relationship level</td>
          <td>1</td>
      </tr>
      <tr>
          <td>Relationship type</td>
          <td>Uses</td>
      </tr>
      <tr>
          <td>Entity type</td>
          <td>Device</td>
      </tr>
      <tr>
          <td>Latest telemetry</td>
          <td>true</td>
      </tr>
      <tr>
          <td>Source telemetry</td>
          <td>windDirection</td>
      </tr>
      <tr>
          <td>Target telemetry</td>
          <td>windDirection</td>
      </tr>
   </tbody>
</table>



![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/get-related.png)

###### 节点 D: **Change Orignator**
- 添加**Change Orignator**节点，并将其连接到关联类型为**Success**的**Save TimeSeries**节点。<br>
  该节点会将originator设备**Wind Direction Sensor**和**Rotating System**更改为相关资产**Wind Turbine**，它们之间的关联类型均为**Contains**。
  <br/>>结果，提交的消息将作为来自该实体的消息进行处理
- 填写下表中输入的数据字段：

<table style="width: 25%">
  <thead>
      <tr>
          <td><b>Field</b></td><td><b>Input Data</b></td>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td>Name</td>
          <td>Create New Telemetry</td>
      </tr>
      <tr>
          <td>Originator source</td>
          <td>Related</td>
      </tr>
      <tr>
          <td>Direction</td>
          <td>To</td>
      </tr>
      <tr>
          <td>Max relationship level</td>
          <td>1</td>
      </tr>
      <tr>
          <td>Relationship type</td>
          <td>Contains</td>
      </tr>
      <tr>
          <td>Entity type</td>
          <td>Asset</td>
      </tr>
   </tbody>
</table>

![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/change-originator.png)

###### 节 E: **Save TimeSeries**
- 添加**Save TimeSeries**”节点，并将其连接到关系类型为**Success**的**Change Orignator**节点。 <br>
  该节点将来自传入消息payload的TimeSeries数据从作为消息Originator的Asset**Wind Turbine**存储到数据库中。
- 输入名称**Save Time Series**。


![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/save-ts.png)

###### 节点F: **Transform Script**
- 添加**Transform Script**节点并将其连接到关联类型为**Success**的**Related attributes**节点。 <br>
该节点会将原始消息转换为RPC请求消息。
- RPC调用将具有2个属性：
	- 方法: **spinLeft** 或 **spinRight**。
	- 参数: **value**。


![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/rpc-message.png)

- 输入名称**New RPC Message**.
- 添加以下脚本：
{% highlight javascript %}
 var newMsg = {};
 var value = Math.abs(msg.turbineDirection - metadata.windDirection);
 if ((value < 180 && msg.turbineDirection < metadata.windDirection)||
     (value > 180 && msg.turbineDirection > metadata.windDirection)) {
     newMsg.method = 'spinLeft';
 }
 if ((value <= 180 && msg.turbineDirection > metadata.windDirection)||
     (value >= 180 && msg.turbineDirection < metadata.windDirection)) {
     newMsg.method = 'spinRight';
 }
 if(newMsg.method == 'spinLeft' || 'spinRight'){
     msgType = 'RPC message';
 }
 newMsg.params = Math.round(value * 100) / 100;
 return {msg: newMsg, metadata: metadata, msgType: msgType}; {% endhighlight %}


###### 节点 G: **Filter Script**

- 添加**Filter Script**节点并将其连接到关联类型为**Success**的**Transform Script**。 <br> 
  此节点将检查传入消息的msgType是否为**RPC消息**。

- 输入名称**Check RPC Message**。
- 添加以下脚本：
{% highlight javascript %}: return msgType == 'RPC message'; {% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/check-validity.png)


###### 节点 H: **RPC call request**
- 添加**RPC call request**节点并将其连接到关联类型为**True**的**Filter Script**节点。 <br>
  该节点获取消息payload，并将其作为响应发送到消息始发者。
- 输入名称**Rotating System**。
- 输入超时值60秒。

![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/request.png)

<br/>

保存已经编辑完成的规则链。

<br/>
<br/>

# 如何验证规则链

- 使用以下javascript代码模拟**Wind Direction Sensor**设备。
    - [**WindDirectionEmulator.js**](/docs/user-guide/rule-engine-2-0/tutorials/resources/WindDirectionEmulator.js).
- 使用以下JavaScript代码来模拟**Rotating System** 设备。 <br>
   该代码包含一种方法，可根据传入的RPC消息模拟更改涡轮机方向。
    - [**RotatingSystemEmulator.js**](/docs/user-guide/rule-engine-2-0/tutorials/resources/RotatingSystemEmulator.js).


要运行脚本，您需要执行以下步骤：

- 复制**Wind Direction Sensor**设备访问令牌和**Rotating System**设备访问令牌，然后将其粘贴到脚本中<br>
  您可以从设备页面复制访问令牌。 <br> <br>
  在本教程中，
    - **Wind Direction Sensor**设备访问令牌为**Z61K03FAGSziW9b0nKsm**
    - **Rotating System**设备访问令牌**jSuvzrURCbw7q4LGtygc**

  但是这些访问令牌是唯一的，您将需要复制设备的访问令牌。

 ![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/wind-token.png) ![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/rs-token.png)

- 打开终端并转到包含这些仿真器脚本的文件夹，然后运行以下命令：
    - node WindDirectionEmulator.js
    - node RotatingSystemEmulator.js


<br/>
<br/>

# 配置仪表盘
以下屏幕截图显示了**Wind Turbine Dashboard**的外观:

![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/dashboard.png)

下载json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/wind_turbine_dashboard.json)并将其导入。

- 转到**Dashboards** -> **Add new Dashboard** -> **Import Dashboard**并删除下载的json文件

下一步是配置导入的仪表板使用的别名。
 
![image](/images/user-guide/rule-engine-2-0/tutorials/rpc-request/aliases.png)

单击**Edit alias**按钮，然后输入下表中显示的输入数据：

<table style="width: 30%">
  <thead>
      <tr>
       <td>Alias </td>
       <td>Field</td>
       <td>Input Data </td>
      </tr>
  </thead>
  <tbody>
      <tr>
           <td rowspan="3" >Wind Turbine
           </td>
           <td>Filter type
           </td>
           <td>Single entity
           </td>
      </tr>
      <tr>
           <td>Type
           </td>
           <td>Asset
           </td>
      </tr>
      <tr>
           <td>Asset
           </td>
           <td>Wind Turbine
           </td>
      </tr>
      <tr>
           <td rowspan="3" >Wind Direction Sensor
           </td>
           <td>Filter type
           </td>
           <td>Single entity
           </td>
      </tr>
      <tr>
           <td>Type
           </td>
           <td>Device
           </td>
      </tr>
      <tr>
           <td>Device
           </td>
           <td>Wind Direction Sensor
           </td>
          </tr>
      <tr>
           <td rowspan="3" >Rotating System
           </td>
           <td>Filter type
           </td>
           <td>Single entity
           </td>
      </tr>
      <tr>
           <td>Type
           </td>
           <td>Device
           </td>
      </tr>
      <tr>
           <td>Device
           </td>
           <td>Rotating System
           </td>
      </tr>
  </tbody>
</table>


仪表盘的配置现已完成，您可以验证它是否按预期工作。

此外您可以看到：

  - 如何使用**RPC call reply**规则节点

请参阅**另请参阅**部分下的第二个链接，以了解如何执行此操作。

<br>
<br>

# 另请参阅

 - 有关RPC在Thignsboard中的工作方式的更多详细信息，请参阅[RPC capabilities](/docs/user-guide/rpc/#server-side-rpc-api)指南。

 - [带有来自相关设备的数据的RPC回复](/docs/user-guide/rule-engine-2-0/tutorials/rpc-reply-tutorial/)指南。

## 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}

