---
layout: docwithnav
title: 设备离线时创建警报
description: 使用规则引擎在设备离线一段时间后创建警报。

---


本教程将向您展示如何使用RuleEngine在设备离线一段时间后创建警报。

* TOC
{:toc}


# 用例

让我们假设以下用例：

 - 您已将一个设备连接至ThingsBoard，并且该设备具有一个温度传感器以收集并推送遥测数据。

 - 由于任何类型的故障，温度传感器可能会停止推送遥测数据。


因此在这种情况下您需要将ThingsBoard Rule Engine配置为：

 - 如果设备在一段时间内保持非活动状态则创建警报。可以通过以下两种方式之一来定义此时间段：
 
    - 第一种方法：通过更改非活动超时的全局配置参数。 <br>
      此参数在**thingsboard.yml**(state.defaultInactivityTimeoutInSec)中定义默认情况下设置为10秒。
    
    - 第二种方式：通过设置服务器端属性**inactivityTimeout**（毫秒为单位）来覆盖特定设备的此参数。 <br>
      以下各节将介绍这种方式。
 
 - 如果设备处于活动状态清除警报。


# 背景
ThingsBoard设备状态服务负责监视设备连接状态并触发推送到规则引擎的设备连接事件。

ThingsBoard支持四种类型的事件：
<table style="width: 70%">
  <thead>
      <tr>
          <td><b>事件类型</b></td><td><b>描述</b></td>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td>连接(Connect)</td>
          <td>在设备连接到ThingsBoard时触发。</td>
      </tr>
      <tr>
          <td>断开(Disconnect)</td>
          <td>当设备与ThingsBoard断开连接时触发。</td>
      </tr>
      <tr>
          <td>活动(Activity)</td>
          <td>在设备推动遥测，属性更新或RPC命令时触发。</td>
      </tr>
      <tr>
          <td>不活(Inactivity)</td>
          <td>在设备在一段时间内处于非活动状态时触发。</td>
      </tr>
   </tbody>
</table>


本教程将详细说明设备不活动事件，并将向您展示如何：

 - 使用规则引擎创建不活动警报。

 - 为非活动超时配置参数。


<br/>
<br/>



# 添加设备

 - 在ThingsBoard中添加设备实体。
 - 输入设备名称为**Temperature device**，并输入设备类型为**Temperature sensor**：

![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/add-device.png)    

<br/>
<br/>

# Configuring the Device

 - 转到**Devices** -> **Temperature device** -> **Attributes** -> **Server attributes** 点击 **Add** 按钮;

 - 例如将**inactivityTimeout**属性设置为等于60000毫秒的值。

![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/add-attribute.png)    

<br/>
<br/>

# 配置规则链

以下屏幕截图显示了初始Root Rule Chain。请注意不相关的规则节点已从Root Rule Chain链中删除。

![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/initial-chain.png)


通过添加以下两个操作节点来修改默认规则链：

 - **Create alarm**节点：以关系类型**Inactivity Event**连接到**Message Type Switch**节点；
 
 - **Clear alarm**节点：连接到**Message Type Switch**节点，其关系类型为**Activity Event**。

以下屏幕截图显示了最终规则链的外观：

![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/chain.png)

- 下载上述规则链json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/tutorial_of_inactivity_event.json)并将其导入。

- 不要忘记将新规则链标记为"root"。

另外您可以从头开始创建新的规则链。下一节将向您展示如何创建它。

#### 创建新的规则链(**Tutorial of Inactivity Event**)
  
  - 转到**Rule Chains** -> **Add new Rule Chain** 
  	
  - 输入名称**Tutorial of Inactivity Event** 点击 **ADD** 按钮。
 
  - 创建新的规则链。不要忘记将其标记为“root”。
  
  ![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/add-chain.png)  ![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/root-chain.png) 

##### 添加所需的节点

在本教程中您将创建5个节点如以下各节所述：

###### **Message Type Switch**节点
添加**Message Type Switch**节点并将其连接到**Input**节点。

该节点将根据消息类型路由传入的消息，即：
  
  - **POST_TELEMETRY_REQUEST**;

  - **POST_ATTRIBUTES_REQUEST**;
  
  - **ACTIVITY_EVENT**;
  
  - **INACTIVITY_EVENT**.
  
 输入名称**Message Type Switch**点击**ADD**按钮。

 
![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/message-type-switch.png)
  

###### **Save Timeseries**节点 
添加**Save TimeSeries**节点并将其连接到**Message Type Switch**节点其关系类型为**Post telemetry**。

该节点会将来自传入消息payload的TimeSeries数据存储到数据库中，并将其链接到消息发起方标识的设备。

输入名称**Save Time Series**。
  
![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/save-ts.png)
  
###### **Save Server Attributes**节点 
添加**Save Attributes**节点并将其连接到关系类型为**Post attributes**的**Message Type Switch**节点。

该节点会将来自传入消息payload的属性存储到数据库中，并将它们链接到消息发起者标识的实体。

输入名称**Save Server Attributes**。
  
![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/save-attributes.png)  
  
###### **Create Inactivity alarm**节点
添加**Create alarm**节点，并将其连接到关系类型为**Inactivity Event**的**Message Type Switch**节点。

该节点尝试使用为消息发起者配置的警报类型加载最新的警报。如果存在未清除的警报则将更新此警报否则将创建一个新的警报。


- 输入名称**Create Inactivity Alarm**并在警报类型中输入**Inactivity TimeOut**。

![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/create-alarm.png)


###### **Clear Inactivity alarm**节点
添加**Clear alarm**节点并将其连接到关系类型为**Activity Event**的**Message Type Switch**节点。

该节点使用消息始发者配置的警报类型加载最新的警报并清除警报（如果存在）。

- 输入名称**Clear Inactivity Alarm**并在警报类型中输入**Inactivity TimeOut**。


![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/clear-alarm.png)

<br/>

现在此规则链已准备就绪您需要保存它。

<br/>
<br/>

# 如何验证Rule Chain和Post telemetry

- 使用Rest API [遥测上传API](/docs/reference/http-api/#telemetry-upload-api)发布设备遥测。 <br>
  请注意，您需要从设备**Temperature device**复制设备访问令牌，如以下屏幕截图所示。

![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/access-token.png)


发布temperature = 20应该在遥测发布一分钟后创建警报:

{% highlight bash %}
curl -v -X POST -d '{"temperature":20}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"

***您需要将$ACCESS_TOKEN替换为实际的设备令牌**
{% endhighlight %}


![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/telemetry.png)

![image](/images/user-guide/rule-engine-2-0/tutorials/inactivity alarms/created-alarm.png)

<br/>

另外您可以：

  - 通过添加警报部件以可视化警报来配置仪表板。
  
  - 定义用于警报处理的其他逻辑，例如发送电子邮件。

请参阅**另请参阅**部分下的前两个链接，以了解如何执行此操作。


    
<br/>
<br/>

# 另请参阅

- [创建和清除警报：配置信息中心](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/#configure-device-and-dashboard) 指南-了解如何添加仪表板的警报小部件。

- [发送邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)。

- [设备连接状态](/docs/user-guide/device-connectivity-status/)。

## 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}

