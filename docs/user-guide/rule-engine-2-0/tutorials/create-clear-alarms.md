---
layout: docwithnav
title: 创建和清除警报
description: 创建和清除警报

---

* TOC
{:toc}

## 用例

假设您的设备正在使用DHT22传感器来收集温度读数并将其推送到ThingsBoard。

DHT22传感器适用于-40至80°C的温度读数。如果温度超出范围，我们希望生成警报。

在本教程中，我们将配置ThingsBoard规则引擎以

- 如果温度> 80°C或温度<-40°C创建或更新现有警报
- 如果温度> -40°C且<80°C清除警报



## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。

# 添加设备
  
在ThingsBoard中添加设备实体。它的名称为**Thermostat Home**其类型为**Thermostat**。
  
![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/add-device.png)
   
<br/>

# 消息流  

在本节中，我们将解释本教程中每个节点的用途：

- 节点A：[**Filter Script**](/docs/user-guide/rule-engine-2-0/filter-nodes/#check-relation-filter-node)节点。
  - 通过温度阈值检查脚本并验证节：“如果温度在预期的间隔内则脚本将返回False否则将返回True”。
- 节点B：[**Create alarm**](/docs/user-guide/rule-engine-2-0/action-nodes/#create-alarm-node)节点。
  - 如果发布的温度不在预期的时间范围内（过滤器脚本节点返回True）则创建或更新警报。
- 节点C：[**Clear alarm**](/docs/user-guide/rule-engine-2-0/action-nodes/#clear-alarm-node)节点。
  - 如果发布的温度在预期的时间范围内（脚本节点返回False）则清除警报（如果存在）。
- 节点D: **Rule Chain**节点。
  - 将传入消息转发到指定的规则链**Create & Clear Alarms**。

<br/>

# 配置规则链

在本教程中我们修改了**Root Rule Chain**并创建了规则链**Create & Clear Alarms**

<br/>以下屏幕截图显示了以上规则链的外观：
 
  - **创建和清除警报：**

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/create-clear-alarm-chain.png)

 - **根规则链：**

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/root-rule-chain.png)

<br/> 

下载附件json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/create___clear_alarms.json)以获取**Create & Clear Alarms**规则链。
如上图所示在根规则链中创建节点D，以将遥测转发到导入的规则链。
<br/>
<br/>

下一节将向您展示如何从头开始创建此规则链。
 
#### 创建新的规则链（**创建并清除警报**）

转到**Rule Chains** -> **Add new Rule Chain** 

配置:

- 名称 : **Create & Clear Alarms**

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/add-chain.png)

创建新规则链。点击**Edit**按钮进行配置。

###### 添加所需的节点

在此规则链中，您将创建3个节点，如以下各节所述：
 
###### 节点A: **Filter Script**
- 添加**Filter Script**节点并将其连接到关联类型为**Success**的**Input**节点。
 <br>此节点将使用以下脚本验证：“温度是否在预期间隔内”：
  
   {% highlight javascript %}return msg.temperature < -40 || msg.temperature > 80;{% endhighlight %}
  
如果温度在预期的时间间隔内，则脚本将返回False，否则将返回True。
    
- 输入名称**Under Threshold**。
  
![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/filter-alarm.png)
   
###### 节点B: **Create alarm**
- 添加**Create alarm**节点，并将其连接到关联类型为**True**的**Filter Script**节点。 <br>

  如果发布的温度不在预期范围内（过滤器脚本节点返回True）则此节点将为消息发起方加载配置了警报类型的最新警报，即**Thermostat Home** <br>。
  
 - 输入名称**Create alarm**并在警报类型中输入**Critical Temperature**。

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/create-alarm.png)

###### 节点C: **Clear Alarm**
- 添加**Clear Alarm**节点并将其连接到关联类型为**False**的**Filter Script**节点。 <br>
  此节点将为消息始发者**Thermostat Home** <br>加载配置了警报类型的最新警报，如果发布的温度在预期范围内（脚本节点返回False）则清除该警报（如果存在）。
  
- 输入名称**Clear Alarm**并在警报类型中输入**Critical Temperature**。

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/clear-alarm.png)


#### 修改根规则链

以下屏幕截图显示了初始“根规则链”。

![image](/images/user-guide/rule-engine-2-0/tutorials/check relation/initial-chain.png)

初始规则链已通过添加以下节点进行了修改：

###### 节点D: **规则链**
- 添加**Rule Chain**节点并将其连接到关联类型为**True**的**Filter Script**节点。 <br>
  节点将传入消息转发到指定的规则链**Create & Clear Alarms**。

- 输入名称**Create & Clear Alarms**.

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/add-chain-node.png)

<br/>

以下屏幕截图显示了最终的**Root Rule Chain**应该是什么样子：

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/root-rule-chain.png)

<br/>
<br/>

# 如何验证规则链和遥测

对于发布设备遥测我们将使用Rest API[遥测上传API](/docs/reference/http-api/#telemetry-upload-api)。为此，我们将需要复制设备**Thermometer**的访问令牌。

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/access-token.png)

{% highlight bash %}**您需要将$ACCESS_TOKEN替换为实际的设备令牌**{% endhighlight %}

点击Clear Alarm和Create Alarm节点中的调试模式按钮以验证结果。

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/debug-mode-clear-alarm.png)<br/>
![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/debug-mode-create-alarm.png)

发布temperature = 99后会创建警报:

{% highlight bash %}
curl -v -X POST -d '{"temperature":99}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/alarm-created.png)

发布temperature = 180后更新警报:

{% highlight bash %}
curl -v -X POST -d '{"temperature":180}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/alarm-updated.png)

发布temperature = 30后会清除警报:

{% highlight bash %}
curl -v -X POST -d '{"temperature":30}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/alarms v2/alarm-cleared.png)

<br/>

另外您可以：

  - 在Create and Clear Alarm节点中配置Alarm Details功能
    
  - 通过添加警报部件以可视化警报来配置仪表板。
  
  - 定义用于警报处理的其他逻辑例如：发送电子邮件。

请参阅**另请参阅**部分下第二到第四的链接，以了解如何执行此操作。
  
<br/>

# 另请参阅


- [验证传入遥测](/docs/user-guide/rule-engine-2-0/tutorials/validate-incoming-telemetry/) - 有关如何使用Script Filter节点验证传入遥测的更多信息。

- [创建和清除警报: 警报详情:](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/#modify-the-required-nodes) - 了解如何在Alarm节点中配置Alarm Details功能。


- [创建和清除警报: 配置面板](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/#configure-dashboard) - 了解如何将警报部件添加到仪表板。

- [发送邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)。

## 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}
