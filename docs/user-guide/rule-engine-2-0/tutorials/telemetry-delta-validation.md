---
layout: docwithnav
title: 遥测增量计算
description: 增量验证

---

* TOC
{:toc}

## 用例

假设我们有一台使用温度传感器来收集和读取ThingsBoard中的温度读数的设备。

另外，我们假设当最后五分钟的温度读数和最新的温度读数之间的差值超过5度时需要生成警报。

请注意这只是一个简单的理论用例，用于演示平台的功能。您可以将本教程用作更复杂场景的基础。

## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
  * [创建&清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)指南。
  
# 添加设备
  
在ThingsBoard中添加设备实体。它的名称是**Thermometer**类型是**temperature sensor**。
  
![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/add-thermometer.png)
   
<br/>

# 消息流  

在本节中我们将解释本教程中每个节点的用途。将涉及两个规则链：

  - **Root rule chain** - 实际上将遥测从设备保存到数据库中，并将消息重定向到**Temperature delta validation**链
   
  - **Temperature delta validation** - 规则链，实际计算最后五分钟的温度与最新的温度读数之间的增量。
    <br>结果如果增量值超过5度，将创建/更新警报，否则将清除警报。

以下屏幕截图显示了以上规则链的外观：
 
  - **Temperature delta validation:**

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/temperature-delta-validation-chain.png)

 - **Root Rule Chain:**

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/root-rule-chain.png)

<br/> 

下载json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/temperature_delta_validation.json)以获取**Temperature delta validation**。

如上图所示在根规则链中创建节点G以将遥测转发到导入的规则链。
<br/>
<br/>

下一节将向您展示如何从头开始创建此规则链。
 
#### 创建新的规则链(**Temperature delta validation**)

转到**Rule Chains** -> **Add new Rule Chain** 

配置:

- 名称 : **Temperature delta validation**

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/add-temperature-delta-validation-chain.png)

创建新规则链。按**Edit**按钮并配置链。

###### 添加所需的节点

在此规则链中您将创建6个节点如以下各节所述：
 
###### 节点A: **Originator telemetry**
- 添加**Originator telemetry**节点并将其连接到关联类型为**Success**的**Input**节点。

  此规则节点将所选消息发起者的遥测信息添加到所选时间范围内的消息元数据中。

规则节点具有三种获取模式：

 - **FIRST**: 从最接近时间范围开始处的数据库中检索遥测

 - **LAST**: 从最接近时间范围末尾的数据库中检索遥测

 - **ALL**: 从数据库中检索指定时间范围内的所有遥测。

使用模式：**LAST**其时间范围为24小时前到5分钟。
  
 - 在名称字段中输入**Latest five-minute old record**。
 
![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/latest-five-minute-old-record.png)
 
###### 提取模式全部
    
  发起方遥测节点还支持从特定时间范围获取所有遥测的功能。

  我们不会在本教程中使用此功能但是如果您需要根据特定时间范围内的遥测变化来计算特定键的方差或预测遥测的进一步变化该功能可能会很有用。
  
  在这种情况下您需要选择提取模式**ALL**。它将强制规则节点从指定的时间范围获取所有遥测并将其作为数组添加到消息元数据中。

  该数组将包含带有时间戳和值的JSON对象。
  
  - 出站邮件的元数据将是具有以下结构的JSON文档：
   
  {% highlight javascript %}
  {
    "temperature": "[{\"ts\":1540892498884,\"value\":22.4},{\"ts\":1540892528847,\"value\":20.45},{\"ts\":1540892558845,\"value\":22.3}]"
  }{% endhighlight %}
    
  - 为了将数组转换为有效的JSON文档，您可以使用以下函数：
    
  {% highlight javascript %}
  var temperatureArray = JSON.parse(metadata.temperature);{% endhighlight %}
  
  - The temperature array will look like introduced below:
  
  {% highlight javascript %}
  {
      "temperatureArray": [{
          "ts": 1540892498884,
          "value": 22.4
      }, {
          "ts": 1540892528847,
          "value": 20.45
      }, {
          "ts": 1540892558845,
          "value": 22.3
      }]
  }{% endhighlight %}  
 
 
###### 节点B: **Script Transformation**
 - 添加**Script Transformation**节点并将其连接到关系类型为**Success**的**Change Orignator**节点。
 
 该节点将使用以下脚本计算从消息payload读取的温度与从消息metadata读取的五分钟旧温度之间的增量：
  
   {% highlight javascript %}
   var newMsg = {};
   
   newMsg.deltaTemperature = parseFloat(Math.abs(msg.temperature - JSON.parse(metadata.temperature)).toFixed(2));
     
   return {msg: newMsg, metadata: metadata, msgType: msgType};{% endhighlight %}
  
 - 在名称字段中输入**Calculate delta**。
   
 ![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/calculate-delta.png)  
  
###### 节点C: **Save Timeseries**  
 - 添加**Save TimeSeries**节点，并将其连接到关系类型为**Success**的**Script Transformation**节点。
 该节点会将TimeSeries数据从传入的消息payload中保存到数据库中，并将其链接到被标识为Message Originator的设备。
     
 - 在名称字段中输入**Save Time Series**。

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/save-timeseries.png)

###### 节点D: **Filter Script**
 - 添加**Filter Script**节点并将其连接到关系类型为**Success**的**Save TimeSeries**节点。
 <br>此节点将使用以下脚本验证最新温度读数与五分钟前温度读数之间的计算出的增量值是否不超过5度：
  
   {% highlight javascript %}
   return msg.deltaTemperature > 5;{% endhighlight %}
      
 - 在名称字段中输入**Validate delta**。
  
![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/validate-delta.png)


###### 节点E: **Create alarm**
 - 添加**Create alarm**节点并将其连接到关联类型为**True**的**Filter Script**节点。 <br>
   如果发布的增量温度不在预期范围内（过滤器脚本节点返回True）则此节点将加载最新消息，该消息具有配置为Message Originator的警报类型的最新警报，即**Thermometer** <br>。
  
 - 在名称字段中输入**Create alarm**并警报类型中输入**General Alarm**。

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/create-alarm.png)

###### 节点F: **Clear Alarm**
 - 添加**Clear Alarm**节点并将其连接到关联类型为**False**的**Filter Script**节点。 <br>
 此节点将为消息始发者**Thermometer**<br>加载配置了警报类型的最新警报，并在发布的温度增量在预期范围内的情况下清除警报（如果存在）（脚本节点返回False）。
  
 - 在名称字段中输入**Clear Alarm**并警报类型中输入**General Alarm**.

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/clear-alarm.png)

#### 修改Root Rule Chain

初始根规则链已通过添加以下节点进行了修改：

###### 节点G: **Rule Chain**
- 添加**Rule Chain**节点并将其连接到关系类型为**Success**“成功”****的**Save Timeseries**节点。 <br>
  该节点将传入消息转发到指定的规则链**Temperature delta validation**。

- 选择Rule Chain字段：**Temperature delta validation**。

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/add-rule-chain-node.png)

<br/>

以下屏幕截图显示了最终的**Root Rule Chain**应该是什么样子：

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/root-rule-chain.png)

- 为上述规则链下载json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/root_rule_chain_delta_calculation.json)并将其导入。
- 不要忘记将新规则链标记为**root**。

<br/>
<br/>

# 如何验证规则链和后遥测

对于发布设备遥测我们将使用Rest API[遥测上传API](/docs/reference/http-api/#telemetry-upload-api)。为此我们将需要从设备**Thermometer**中复制设备访问令牌。

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/access-token.png)

{% highlight bash %}**您需要将$ACCESS_TOKEN替换为实际的设备令牌**{% endhighlight %}

为了验证规则链是否按预期工作，我们需要对同一设备两次遥测两次，间隔不小于5分钟且不超过24小时。
<br>另外，让我们在**Create Alarm**节点中按下调试模式按钮，以验证是否在第二次遥测请求之后创建警报。

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/debug-mode-create-alarm.png)<br/>

发布temperature = 20

{% highlight bash %}
curl -v -X POST -d '{"temperature":20}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/first-post-telemetry.png)

延迟5分钟后，让我们发送例如温度 = 26

{% highlight bash %}
curl -v -X POST -d '{"temperature":26}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/second-post-telemetry.png)

应创建警报：

![image](/images/user-guide/rule-engine-2-0/tutorials/delta-validation/alarm-created.png)

<br/>

另外您可以：

  - 在创建和清除警报节点中配置警报详细信息功能。
    
  - 通过添加警报部件以可视化警报来配置仪表板。
  
  - 定义用于警报处理的其他逻辑，例如，发送电子邮件。

请参阅**另请参阅**分下第二到第四的链接，以了解如何执行此操作。
  
<br/>

# 另请参阅


- [验证传入遥测](/docs/user-guide/rule-engine-2-0/tutorials/validate-incoming-telemetry/) 教程 - 有关如何使用脚本过滤器节点验证传入遥测的更多信息。

- [创建和清除警报：警报详细信息：](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/#modify-the-required-nodes) 指南-了解如何在警报节点中配置“警报详细信息”功能。

- [创建和清除警报：配置信息中心](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/#configure-dashboard) 指南-了解如何添加仪表板的警报小部件。

- [发送电子邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)教程。

# 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}
