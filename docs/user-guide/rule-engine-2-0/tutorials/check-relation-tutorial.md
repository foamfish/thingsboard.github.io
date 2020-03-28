---
layout: docwithnav
title: 检查实体之间的关系
description: 检查关系

---
本教程的目的是演示如何可以将[**Check Relation**](/docs/user-guide/rule-engine-2-0/filter-nodes/#check-relation-filter-node)节点设置为用于检查实体之间的关系。

* TOC
{:toc}

## 用例

让我们假设以下用例：

 - 您有2台设备：
 
   - 带有**Smoke Detector**的**Smoke Sensor**当数据出现时将数据发送到ThingsBoard。
   
   - **Fire Alarm System**当有烟雾时提供火灾警报。
   
但是有多种实现此情况的方法，例如，可以使用将传入消息路由到一个或多个输出链的**Switch**节点来实现。<br/>

有关如何使用**Switch**节点的更多信息请在[**另请参见**](/docs/user-guide/rule-engine-2-0/tutorials/check-relation-tutorial/#see-also)部分。
 
## 先决条件

在开始本教程之前，您需要阅读以下指南：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
  
# 添加设备并创建它们之间的关系
  
  在ThingsBoard中添加两个Device实体：
  
   - 烟雾探测器表示为设备。它的名称为**Smoke Detector**类型为 **Smoke Sensor**。
   
   - 火警系统表示为设备。它的名称是**Fire Alarm System**类型是**Fire Alarm Device**。
  
  创建类型的关系用途：
  
   - 从烟雾探测器到火灾报警系统；
  
  以下屏幕截图显示了如何执行此操作：
  
   ![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/smoke-sensor.png) ![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/fire-alarm-system.png) <br/>
   ![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/add-relation.png)
   
<br/>

# 消息流 

在本节中我们将解释本教程中每个节点的用途：

- 节点A: [**Check Relation**](/docs/user-guide/rule-engine-2-0/filter-nodes/#check-relation-filter-node)。
  - 使用关系的类型和方向检查从设备**Fire Alarm System**到消息**Smoke Detector**的始发者的关系。
- 节点B: [**Change originator**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#change-originator)。
  - 将始发者从设备**Smoke Detector**更改为相关设备**Fire Alarm System**，提交的消息将作为来自设备“火灾警报系统”的消息进行处理。
- 节点C: [**Transformation Script**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#script-transformation-node).
  - 将原始消息转换为RPC请求消息。
- 节点D: [**RPC call request**](/docs/user-guide/rule-engine-2-0/action-nodes/#rpc-call-request-node)。
  - 获取消息payload，并将其作为响应发送到**Fire Alarm System**。
- 节点E: [**Filter Script**](/docs/user-guide/rule-engine-2-0/filter-nodes/#script-filter-node)。
  - 检查传入消息的数据是否为**smoke**。
- 节点F: [**Clear alarm**](/docs/user-guide/rule-engine-2-0/action-nodes/#clear-alarm-node)。
  - 加载最新警报该警报具有为消息发起者**Smoke Detector**配置的警报类型，并清除警报（如果存在）。
- 节点G: [**Create alarm**](/docs/user-guide/rule-engine-2-0/action-nodes/#create-alarm-node)。
  - 尝试为消息发起方加载配置了警报类型的最新警报即**Smoke Detector**。
- 节点H: **Rule Chain**。
  - 将传入消息转发到指定的规则链**Related Fire Alarm System**。

<br/>
  
# 配置规则链

在本教程中我们修改了**Root Rule Chain**并创建了**Related Fire Alarm System**

<br/>以下屏幕截图显示了以上规则链的外观：
 
  - **Related Fire Alarm System:**

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/fire-alarm-chain.png)

 - **Root Rule Chain:**

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/chain.png)

<br/> 

下载json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/check-relation-tutorial.json)以获取**Root Rule Chain**。不要忘了将此规则链标记为**root**。

<br/> 
  
![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/root-chain.png)

另外您需要创建**Related Fire Alarm System**规则链，也可以下载json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/related_fire_alarm_system.json)。
<br/>
<br/>

下一节将向您展示如何创建它。

#### 创建新的规则链 (**Related Fire Alarm System**)

转到**Rule Chains** -> **Add new Rule Chain** 

配置:

- 名称 : **Related Fire Alarm System**

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/add-fire-alarm-chain.png)

创建新规则链，按点击**Edit**按钮并配置链。

###### 添加所需的节点

在此规则链中您将创建4个节点如以下各节所述：

###### 节点A: **Check Relation**

 - 添加**Check Relation**节点，并将其连接到**Input**节点。<br/>

    该节点将使用关系的类型和方向检查从设备**Fire Alarm System**到消息**Smoke Detector**的始发者的关系。
    如果存在该关系，则消息将通过True链发送。

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
           <td>Check Relation</td>
       </tr>     
       <tr>
           <td>Direction</td>
           <td>To</td>
       </tr>
       <tr>
           <td>Type</td>
           <td>Device</td>
       </tr>
        <tr>
           <td>Device</td>
           <td>Fire Alarm System</td>
        </tr>
       <tr>
           <td>Relation type</td>
           <td>Uses</td>
       </tr>
    </tbody>
 </table>
  
![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/check-relation.png)

###### 节点B: **Change Orignator**

- 添加**Change Orignator**节点并将其连接到关联类型为**True**的**Check Relation**节点。 <br>
  该节点会将始发者从设备**Smoke Detector**更改为相关的设备**Fire Alarm System**并且提交的消息将作为来自另一个实体**Fire Alarm System**的消息进行处理。

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
          <td>Change Originator</td>
      </tr>
      <tr>
          <td>Originator source</td>
          <td>Related</td>
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
          <td>Relation type</td>
          <td>Uses</td>
      </tr>
      <tr>
          <td>Entity type</td>
          <td>Device</td>
      </tr>
   </tbody>
</table>

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/change-originator.png)
 
###### 节点C: **Script Transformation**
 - 添加**Script Transformation**节点并将其连接到关系类型为**Success**的更改**Change Orignator**节点。

该节点会将原始消息转换为RPC请求消息。

-RPC调用将具有2个属性：

    <table style="width: 25%">
      <thead>
          <tr>
              <td><b>Property</b></td><td><b>Value</b></td>
          </tr>
      </thead>
      <tbody>
          <tr>
              <td>method</td>
              <td>ON</td>
          </tr>
          <tr>
              <td>params</td>
              <td>{}</td>
          </tr>
       </tbody>
    </table>
	
 - 请添加以下脚本：
 
  {% highlight javascript %}
    var newMsg = {};
    if(msg.smoke == 'true'){
          newMsg.method = 'ON';  
    } 
    newMsg.params={};
    return {msg: newMsg, metadata: metadata, msgType: msgType};{% endhighlight %}


 - 输入名称**New RPC message**。
  
![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/transformation-node.png)  

###### 节点D: **RPC call request**
- 添加**RPC call request**节点并将其连接到关系类型为**Success**的**Script Transformation**节点<br>
  该节点获取消息payload并将其作为响应发送到消息始发者**Fire Alarm System**。
- 输入名称**Fire Alarm System**。
- 输入值为60秒。

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/rpc-call-request.png)

此规则链已准备就绪，我们应该保存它。


#### 修改Root Rule Chain

初始规则链已通过添加以下节点进行了修改：

###### 节E: **Filter Script**
- 添加**Filter Script**节点并将其连接到关联类型为**Success**的**Save Timeseries**节点。<br>
  该节点将使用以下脚本检查传入消息的数据是否为**smoke**：
  
  {% highlight javascript %}return msg.smoke== 'true';{% endhighlight %}
  
- 输入名称**Smoke Alarm Filter**。
  
![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/alarm-filter.png)
 
###### 节点F: **Clear Alarm**
- 添加**Clear Alarm**节点并将其连接到关联类型为**False**的**Filter Script**节点。<br>
  此节点将为消息始发者**Smoke Detector**加载配置了警报类型的最新警报，并清除警报（如果存在）。
  
- 输入名称**Clear Smoke Alarm**和警报类型**Smoke Alarm**。

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/clear-alarm.png)
  
###### 节点G: **Create alarm**
- 添加**Create alarm**节点，并将其连接到关联类型为**True**的**Filter Script**节点。 <br>
  该节点尝试为消息发起方加载配置了警报类型的最新警报即**Smoke Detector**。
  
 - 输入名称**Create Smoke Alarm**和警报类型**Smoke Alarm**。

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/create-alarm.png)

###### 节点H: **Rule Chain**
- 添加**Rule Chain**节点并将其连接到关联类型为**True**的**Filter Script**节点。 <br>
  该节点将传入消息转发到指定的规则链**Related Fire Alarm System**。

- 输入名称**Related Fire Alarm System**。

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/add-alarm-chain.png)
  


以下屏幕截图显示了最终的**Root Rule Chain**应该是什么样子：

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/view-chain.png)

<br/>
<br/>

# 如何验证Rule Chain和Post telemetry

- 使用以下JavaScript代码模拟**Fire Alarm System**设备。

  - [**FireAlarmEmulator.js**](/docs/user-guide/rule-engine-2-0/tutorials/resources/FireAlarmEmulator.js).
  
  - 要运行脚本您需要执行以下步骤：
  
  - 复制**Fire Alarm System**设备访问令牌，然后将其粘贴到脚本中。 <br>
  您可以从设备页面复制访问令牌。 <br> <br>
  

- 使用Rest APIs[遥测上传APIs](/docs/reference/http-api/#telemetry-upload-api)从设备**Smoke Detector**发布遥测。<br>

{% highlight bash %}
curl -v -X POST -d '{"smoke":"true"}' http://demo.thingsboard.io/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"

**您需要将$ACCESS_TOKEN替换为实际的设备令牌**
{% endhighlight %}
<br/>

  ![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/smoke-telemetry.png)![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/fire-alarm-telemetry.png)

<br/>
另外您可以：

  - 通过添加警报部件以可视化警报来配置仪表板。
  
  - 定义用于警报处理的其他逻辑例如发送电子邮件。

请参阅**另请参阅**部分下的第三和第四链接，以了解如何执行此操作。
  
<br/>

# 另请参阅

- [Switch节点](/docs/user-guide/rule-engine-2-0/filter-nodes/#switch-node)指南-有关如何在Thignsboard中使用Switch节点的更多信息。

- [验证传入遥测](/docs/user-guide/rule-engine-2-0/tutorials/validate-incoming-telemetry/#step-1-adding-temperature-validation-node)教程-有关如何操作的更多信息使用脚本过滤器节点来验证传入遥测。

- [创建和清除警报：配置信息中心](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/#configure-device-and-dashboard)指南-了解如何添加仪表板的警报部件。

- [发送邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)教程.

- [RPC功能](/docs/user-guide/rpc/#server-side-rpc-api)指南-有关RPC如何在Thignsboard中工作的更多信息，请参考RPC功能指南。

## 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}







