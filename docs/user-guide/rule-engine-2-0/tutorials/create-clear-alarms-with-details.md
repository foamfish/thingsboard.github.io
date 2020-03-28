---
layout: docwithnav
title: 处理警报详细信息
description: 创建和清除带有详细信息的警报

---

* TOC
{:toc}


## 用例

本教程基于[创建和清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/#use-case)教程并且它是用例。

我们将重用上述教程中的规则链并在创建和清除警报点中配置警报详细信息功能。

假设您的设备正在使用DHT22传感器来收集温度读数并将其推送到ThingsBoard。

DHT22传感器适用于-40至80°C的温度读数。如果温度超出范围，我们希望生成警报。

在本教程中我们将ThingsBoard规则引擎配置为：

- 计算每个设备的临界温度更新数，并将此信息保存在警报详细信息中。

- 在警报详细信息中保存最新的临界温度值。

## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
  * [创建和清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)。
  
# 消息流  

在本节中我们将解释本教程中每个节点的用途：

- 节点A: [**Create alarm**](/docs/user-guide/rule-engine-2-0/action-nodes/#create-alarm-node)。
  - 如果发布的温度不在预期的时间范围内（过滤器脚本节点返回True）则创建或更新警报。
- 节点B: [**Clear alarm**](/docs/user-guide/rule-engine-2-0/action-nodes/#clear-alarm-node)。
  - 如果发布的温度在预期的时间范围内（脚本节点返回False）则清除警报（如果存在）。
- 节点C: **Rule Chain**。
  - 将传入消息转发到指定的规则链**Create & Clear Alarms with details**。

<br/>  
  
# 配置规则链

在本教程中我们仅修改了**Create & Clear Alarms**规则链，即在节点中配置了“警报详细信息”功能，如上文[消息流](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/#message-flow)<br>
另外，我们将此规则链重命名为**Create & Clear Alarms with details**。

<br/>以下屏幕截图显示了以上规则链的外观：
 
  - **Create & Clear Alarms详细信息:**

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/create-clear-alarm-chain.png)

  - **Root Rule Chain:**

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/root-rule-chain.png)

<br/> 

下载json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/create___clear_alarms_with_details.json)以使用**Create & Clear Alarms with details:**规则链。
如上图所示，在根规则链中创建节点**C**，以将遥测转发到导入的规则链。
<br/>
<br/>

下一节将向您展示如何修改此规则链，特别是：规则节点[**A**](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/#node-a-create-alarm)和[**B**](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/#node-b-clear-alarm)。


## 修改**Create & Clear Alarms详细信息：**

### 修改所需的节点

在此规则链中，您将修改2个节点，如以下各节所述：

#### 节点A: **Create alarm**

如果发布的温度不在预期的时间范围内（**脚本**节点返回**True**）我们想创建一个警报。
我们要在“警报详细信息”字段中添加当前的**temperature**。
另外，如果警报已经存在，我们想增“警报详细信息中的**count**字段，否则将计数设置为1。
 
我们将覆盖**Details**函数：

**Details** function:
{% highlight javascript %}
var details = {};
details.temperature = msg.temperature;

if (metadata.prevAlarmDetails) {
    var prevDetails = JSON.parse(metadata.prevAlarmDetails);
    details.count = prevDetails.count + 1;
} else {
    details.count = 1;
}

return details;
{% endhighlight %}

**Details**函数使用初始参数创建必需的**Details**对象然后，在**if**语句中我们确认是否存在新的警报或警报已经存在。
如果存在-取前一个**count**字段并递增。

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/create-alarm.png)

如果在**Create Alarm**节点中创建了新警报，则该警报会通过关系**Created**传递到其他节点（如果存在）。

如果Alarm已更新-如果存在，则通过关系**Updated**将其传递到其他节点。

#### 节点B: **Clear Alarm**

如果发布的温度在预期的时间范围内（**脚本**节点返回**False**），我们希望清除现有警报。

同样在清除过程中，我们想在现有的警报详细信息中添加最新的**temperature**。

覆盖**Details**函数：

**Alarm Details**函数:
{% highlight javascript %}
var details = {};
if (metadata.prevAlarmDetails) {
    details = JSON.parse(metadata.prevAlarmDetails);
}
details.clearedTemperature = msg.temperature;

return;
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/clear-alarm.png)

如果**Clear Alarm**节点找不到现有警报，则不进行任何更改，并将原始消息通过关系**False**传递给其他节点（如果存在）。
如果确实存在警报，则将其清除并通过**Cleared**关系传递给其他节点。


链配置已完成，我们需要 **save it**。

<br/>

# 配置仪表板

下载本教程中指示的仪表板的附件json[**文件**](/docs/user-guide/resources/thermostat_dashboard.json)并将其导入。

- 转到**Dashboards** -> **Add new Dashboard** -> **Import Dashboard** 并删除下载的json文件。

您也可以从头开始创建仪表板，以下部分向您展示了如何执行此操作：

## 创建仪表板

我们将为所有**Thermostat**设备创建仪表板并在其上添加Alarm部件。创建新的仪表板：

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/add-dashboard.png)

按**Edit**仪表板，然后**add alias**，这将被解析为所有类型为**Thermostat**的设备：

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/add-alias.png)

将**Alarm widget**添加到仪表板(Add new widget -> Alarm widget bundle -> Alarms)。选择已配置的别名**entity alarm source**。
另外，添加其他**alarm fields**。

- details.temperature.
- details.count.
- details.clearedTemperature. 

并按字段上的**edit**按钮重命名每个字段的标签：

 - 从: -> 至:
 
   - details.temperature        -> Event Temperature.
   - details.count              -> Events count.
   - details.clearedTemperature -> Clear Temperature.

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/alarm-widget-config.png)

# 进行遥测并验证

为了发布设备遥测，我们将使用Rest API([连接](/docs/reference/http-api/#telemetry-upload-api))。为此，我们将需要复制设备**Thermostat Home**的访问令牌


![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/copy-access-token.png)

**您需要将$ACCESS_TOKEN替换为实际的设备令牌**

发布temperature = 99。创建警报:

{% highlight bash %}
curl -v -X POST -d '{"temperature":99}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/alarm-created.png)

发布temperature = 180。警报应更新，计数字段应增加

{% highlight bash %}
curl -v -X POST -d '{"temperature":180}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/alarm-updated.png)

发布temperature = 30。应清除警报并显示清除的温度

{% highlight bash %}
curl -v -X POST -d '{"temperature":30}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms/alarm-cleared.png)

您还可以查看如何：

 - 定义用于警报处理的其他逻辑例如向Telegram App发送电子邮件或发送通知。
 
请参阅“另请参阅”部分下的链接，以了解如何执行此操作。

<br>

# 另请参阅

 - [发送邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)。
 
 - [使用Telegram Bot的智能手机上的通知和警报](/docs/iot-gateway/integration-with-telegram-bot/)。

# 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}
