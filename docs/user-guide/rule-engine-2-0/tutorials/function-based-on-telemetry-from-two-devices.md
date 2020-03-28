---
layout: docwithnav
title: 基于2个设备收集遥测数据
description: 基于2个设备收集遥测数据的功能

---

本教程将展示如何根据室内和室外仓库温度计的读数计算温度变化量。

* TOC
{:toc}

## 用例


假设您有一个带有室内温度计和室外温度计的仓库：我们将配置ThingsBoard规则引擎以根据温度传感器的最新读数自动计算仓库内部和外部的温度变化量。

请注意这只是一个简单的理论用例用于演示平台的功能。

您可以将本教程用作更复杂场景的基础。

 
## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
  
## 模型定义
  
我们将创建一个名称为“Warehouse A”的资产后输入“warehouse”。

![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/add-asset.png)

我们将创建两个名称分别为“Inside Thermometer”和“Outside Thermometer”的设备并分别使用“Inside Thermometer”和“Outside Thermometer”类型。

![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/add-indoor-thermometer.png)
![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/add-outdoor-thermometer.png)

我们还必须在资产“Warehouse A”和设备“Inside Thermometer”之间建立关系。

此关系将在规则链中使用以将消息的始发者从温度计更改为仓库本身以及从设备“Inside Thermometer”到设备“Outside Thermometer”的关系，以从“Outside Thermometer”获取最新温度。

 
![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/add-relation-from-asset.png)

<br>

**注意**：请查看以下[**文档页面**](/docs/user-guide/entities-and-relations/)以了解如何创建资产和关系。

## 消息流

在本节中我们将解释本教程中每个节点的用途。将涉及三个规则链：

  - "Thermometer Emulators"-可选规则链用于模拟来自两个温度传感器的数据。

  - "Root rule chain" - 规则链实际上将遥测从设备保存到数据库中并在将设备重定向到"Delta Temperature"链之前按设备类型过滤消息。
   
  - "Delta Temperature" - 规则链用于实际计算仓库内和室外温度计之间的温度增量。

### 温度计模拟器规则链

![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/thermostats-emulators-chain.png)

  - **节点A和B**：生成器节点
  
  - 两个类似的节点会定期生成一个非常简单的消息，并带有随机温度读数。
    
  - **节点A：室内温度计模拟器**
           
             {% highlight javascript %}
             var msg = {
             	temperature: (20 + 5 * Math.random()).toFixed(1)
             };
             
             return {
             	msg: msg,
             	metadata: {
             		deviceType: "indoor thermometer"
             	},
             	msgType: "POST_TELEMETRY_REQUEST"
             };
             {% endhighlight %}
    
  - **节点B：室外温度计模拟器**
            
             {% highlight javascript %}
             var msg = {
             	temperature: (18 + 5 * Math.random()).toFixed(1)
             };
             
             return {
             	msg: msg,
             	metadata: {
             		deviceType: "outdoor thermometer"
             	},
             	msgType: "POST_TELEMETRY_REQUEST"
             };
             {% endhighlight %}
    
  ![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/indoor-thermometer-emulator.png)
    
  ![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/outdoor-thermometer-emulator.png)
  
<br>  
  
**注意**: 在实际情况下，设备类型默认设置为消息metadata。
    
    
  - **节点C**：规则链节点

    - 将所有邮件转发到默认的根规则链。
    
<br>
   
### 根规则链

![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/root-rule-chain.png)

   - **节点D**：规则链节点
 
     - 将传入消息转发到指定的规则链"Delta Temperature"。

<br>
      
### Delta Temperature 规则链

![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/delta-temperature-chain.png)

  - **节点E**: Switch node.
  
    - 按从消息metadata中获取的deviceType路由传入消息。如果来自传入消息的deviceType是"indoor thermometer"，则通过"indoor"关系类型切换到链，否则如果来自传入消息的deviceType是"outdoor thermometer"，则通过"outdoor"关系类型切换到链。
    
        {% highlight javascript %}
        function nextRelation(metadata, msg) {
        	if (metadata.deviceType === 'indoor thermometer') {
        		return ['indoor'];
        	} else if (metadata.deviceType === 'outdoor thermometer')
        		return ['outdoor'];
        }
        
        return nextRelation(metadata, msg);
        {% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/switch-by-type.png)   
    
  - **节点F和G**: Transform script nodes
  
    - 两个类似的节点根据来自先前节点的关系类型，将密钥名称从消息有效负载从"temperature"更改为"indoorTemperature"或"outdoorTemperature"。
    
    - 创建新的出站消息，并在其中放入新的遥测。 <br>
    
  - **节点F: 更改为Outdoor** 
       
         {% highlight javascript %}
         var newMsg = {};
               
         newMsg.outdoorTemperature = msg.temperature;
               
         return {
            msg: newMsg,
           	metadata: metadata,
           	msgType: msgType
         };
         {% endhighlight %}

  - **节点G: 更改为Indoor**
        
         {% highlight javascript %}
         var newMsg = {};
          
         newMsg.indoorTemperature = msg.temperature;
          
         return {
          	msg: newMsg,
          	metadata: metadata,
          	msgType: msgType
         };
         {% endhighlight %}

   
 - **节点H**: 更改发起方节点。
  
    - 将发起者从"Inside Thermometer"更改为相关资产"Warehouse A"并且所提交的消息将作为来自资产的消息进行处理。
        
![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/to-asset.png) 
    
 - **节点I**: Save Timeseries node.
  
    - 将来自传入消息payload的TimeSeries数据保存到数据库中。
         
 - **节点J**: Originator attributes node.
   
    -  将消息始发者的最新遥测值添加到消息metadata中。
    
![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/fetch-latest-timeseries.png) 

 - **节点K**: Transform script node.
  
    - 创建新的出站消息在该消息中放入新的遥测"deltaTemperature"，其计算方式类似于消息元数据遥测值之差即"indoorTemperature"和"outdoorTemperature"之间的绝对值。 <br>
    
        {% highlight javascript %}
        var newMsg = {};
        
        newMsg.deltaTemperature = parseFloat(Math.abs(metadata.indoorTemperature - metadata.outdoorTemperature).toFixed(2));
        
        return {
        	msg: newMsg,
        	metadata: metadata,
        	msgType: msgType
        };
        {% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/temperature-delta.png)         

 - **节点L**: Save Timeseries节点.
  
    - 将来自传入消息payload的TimeSeries数据保存到数据库中。

<br>

## 配置Rule Chains

下载并[**导入**](/docs/user-guide/ui/rule-chains/#rule-chains-importexport)附加的模拟器规则链[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/thermometer_emulators.json)作为新的"Thermometer Emulators"规则链，
根规则链[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/root_rule_chain_function_from_two_devices.json)作为新的"Root rule chain"和"Delta Temperature"[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/delta_temperature.json)。
请注意，某些节点已启用调试。

## 验证流程

下载并[**导入**](/docs/user-guide/ui/dashboards/#iot-dashboard-importexport)附加的信息中心[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/warehouse_dashboard.json)作为新的"Warehouse dashboard"。

![image](/images/user-guide/rule-engine-2-0/tutorials/data-function/dashboard.png) 

## 下一步

{% assign currentGuide = "DataAnalytics" %}{% include templates/guides-banner.md %}

 






