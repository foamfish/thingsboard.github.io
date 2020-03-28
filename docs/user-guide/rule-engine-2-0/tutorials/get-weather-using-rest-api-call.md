---
layout: docwithnav
title: 使用REST API调用进行天气阅读
description: REST API天气指南

---



本教程将展示如何使用REST API获取天气数据。

* TOC
{:toc}

## 用例

假设您需要了解资产位置的当前天气。您可以将天气信息用于某些数据
处理逻辑或仅用于跟踪历史记录并在仪表板上启用此信息的可视化。

在本教程中，我们将配置ThingsBoard规则引擎以使用REST API自动获取天气信息。
您可以将本教程用作更复杂任务的基础。
 

## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
  * [外部规则节点](/docs/user-guide/rule-engine-2-0/external-nodes/)。

## 添加资产
  
在ThingsBoard中添加资产实体。它的名称为**Building A**，类型为**building**。

![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/rest-api-weather-building.png)

**注意**：如果您拥有专业版，则需要使用客户层次结构向客户添加资产
以下方式：

- 转到**Customers Hierarchy** -> **All** -> **(Current tenant)** -> **Customer groups** -> **(Your customer group)**
 -> **(Your customer)** -> **Asset groups** -> **(Your asset group)** -> **Add asset**
 
 ![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/add-asset-pe-weather-rest-api.png)
 
## 在社区版中为客户分配资产

- 转到**Assets** -> **Assign to customer** -> **(Your Customer)** -> **Assign**

 ![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/assign-asset-weather-rest-api.png)

## 在提供数据的网站上注册

为了获取天气数据，您应该在提供该数据的网站上注册。在这种情况下
 [OpenWeatherMap](https://openweathermap.org/) 将被使用。

在此处签名后，请转到[这里](https://home.openweathermap.org/api_keys)页面以获取您的api密钥。

![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/openweathermap-apikey.png)

## 创建属性

要执行REST API调用，我们需要以下URL参数：
API密钥，经度，纬度和度量单位。

我们建议将API键参数添加到客户服务器端属性，并将其他参数添加到资产
服务器端属性。
 
客户属性应如下所示：
 
 - 转到**(Assigned customer)** -> **Attributes** -> **Add**
 
 ![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/add-attribute-customer.png)
 
如下添加属性：
 
 <table style="width: 50%">
   <thead>
       <tr>
           <td><b>Field</b></td><td><b>Data Type</b></td><td><b>Input Data</b></td>
       </tr>
   </thead>
   <tbody>
       <tr>
           <td>APPID</td>
           <td>String</td>
           <td>(an API key you got from OpenWeatherMap)</td>
       </tr>
    </tbody>
 </table> 
 
资产属性应如下所示：

- 转到时**Building A** -> **Attributes** -> **Add**

![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/add-new-attribute.png)

- 使用下表中显示的输入数据填写属性：

<table style="width: 50%">
  <thead>
      <tr>
          <td><b>Field</b></td><td><b>Data Type</b></td><td><b>Input Data</b></td>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td>latitude</td>
          <td>Double</td>
          <td>latitude of an asset</td>
      </tr>
      <tr>
          <td>longitude</td>
          <td>Double</td>
          <td>longitude of an asset</td>
      </tr>
      <tr>
          <td>units</td>
          <td>String</td>
          <td>"metric" for meters per second wind speed and Celsius temperature, "imperial" for miles per hour wind speed and
           Fahrenheit temperature, empty for meters per second wind speed and Kelvin temperature</td>
      </tr>
   </tbody>
</table> 


在此示例中，将使用纽约市的坐标和公制单位。


## 消息流

在本节中，我们将解释本教程中每个节点的用途。将涉及一个规则链：

  - **Outside Temperature/Humidity** - 规则链每15秒将API调用发送到OpenWeatherMap，并将有关湿度和温度的数据发送到选定的资产。

下面的屏幕截图显示了上面的规则链应该是什么样子：

![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/weather-rule-chain-customer.png)   
   
下载并[**导入**](docs/user-guide/ui/rule-chains/#rule-import)
json [**文件**](/docs/user-guide/resources/outside-temperature-humidity-customer.json)其中包含本教程的规则链。
请注意，您需要将最开始创建的资产设置为最左侧生成器节点中的发起者。

下一节将向您展示如何从头开始创建此规则链。

#### 创建新的规则链(**Outside Temperature/Humidity**)

转到**Rule Chains** -> **Add new Rule Chain** 

配置:

- Name : **Outside Temperature/Humidity**

![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/add-weather-rest-api-chain.png) 

创建新规则链。按**Edit**按钮并配置链。

###### 添加所需的节点

在此规则链中，您将创建5个节点，如以下各节所述：

###### 节点A A: **Generator node**
   - 添加**Generator**节点。此规则节点生成空消息以触发REST API调用。
   - 通过以下方式填写其字段：
   <table style="width: 50%">
     <thead>
         <tr>
             <td><b>Field</b></td><td><b>Value</b></td>
         </tr>
     </thead>
     <tbody>
         <tr>
             <td>Name</td>
             <td>Generate requests</td>
         </tr>
         <tr>
             <td>Message count</td>
             <td>0</td>
         </tr>
         <tr>
             <td>Period in seconds</td>
             <td>15</td>
         </tr>
         <tr>
             <td>Originator type</td>
             <td>Asset</td>
         </tr>
         <tr>
             <td>Asset</td>
             <td>Building A</td>
         </tr>
         <tr>
             <td>Generate function</td>
             <td>return { msg: {}, metadata: {}, msgType: "POST_TELEMETRY_REQUEST" };</td>
         </tr>
      </tbody>
   </table>
    
   ![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/weather-rule-chain-node-A.png) 
   
   
###### 节点 B: **Customer attributes enrichment node**
       
   - 添加**Customer attributes node**并将其连接到关联类型为**Success**的**Generator node**节点。
   该节点会将客户属性APPID放入消息的元数据中。
   - 通过以下方式填写其字段：
   <table style="width: 50%">
            <thead>
                <tr>
                    <td><b>Field</b></td><td><b>Value</b></td>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>Name</td>
                    <td>Get customer API key</td>
                </tr>
                <tr>
                    <td>Latest telemetry</td>
                    <td>False</td>
                </tr>
                <tr>
                    <td>Source attribute</td>
                    <td>APPID</td>
                </tr>
                <tr>
                    <td>Target attribute</td>
                    <td>APPID</td>
                </tr>
             </tbody>
   </table>     
   ![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/weather-rule-chain-node-B.png) 
      
###### 节点 C: **Originator attributes enrichment node**
   - 添加**Originator attributes enrichment node**节点并将其连接到关联类型为**Success**的**Customer attributes node**节点该节点将获取服务器属性的纬度，经度和在**Generator**节点中设置的发起者的单位到元数据中。

   - 通过以下方式填写其字段：
       <table style="width: 50%">
         <thead>
             <tr>
                 <td><b>Field</b></td><td><b>Value</b></td>
             </tr>
         </thead>
         <tbody>
             <tr>
                 <td>Name</td>
                 <td>Latitude/Longitude</td>
             </tr>
             <tr>
                 <td>Server attributes</td>
                 <td>latitude, longitude, units</td>
             </tr>
          </tbody>
       </table>  
   ![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/weather-rule-chain-node-C.png)
  
###### 节点 D: **External REST API call node**
   - 添加**External REST API call node**并将其连接到关联类型为**Success**的**Originator attributes enrichment node**节点该节点将执行对OpenWeatherMap的REST API调用。
   - 通过以下方式填写其字段：
      <table style="width: 50%">
             <thead>
                <tr>
                    <td><b>Field</b></td><td><b>Value</b></td>
                </tr>
             </thead>
             <tbody>
                <tr>
                    <td>Name</td>
                    <td>Get Weather Data</td>
                </tr>
                <tr>
                    <td>Endpoint URL pattern</td>
                    <td>http://api.openweathermap.org/data/2.5/weather?lat=${ss_latitude}&lon=${ss_longitude}&units=${ss_units}&APPID=${APPID}</td>
                </tr>
                <tr>
                    <td>Request method</td>
                    <td>GET</td>
                </tr>
                <tr>
                    <td>Use simple client HTTP factory</td>
                    <td>False</td>
                </tr>
             </tbody>
      </table>  
   - ss_latitude, ss_longitude, ss_units, ss_APPID是从元数据中获取的服务器属性，这些元数据由**Originator attributes enrichment node**放置在那里。
   
   ![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/weather-rule-chain-node-D.png)
    
###### 节点 E: **Script transformation node**
   - 添加**Script transformation node**并将其连接到关系类型为**Success**的**External REST API call node**点.该中断外部温度，最高温度，最低温度和湿度插入消息。
   - 通过以下方式填充变换功能：
     
     
  ```js
      var newMsg = {
          "outsideTemp": msg.main.temp,
          "outsideMaxTemp": msg.main.temp_max,
          "outsideMinTemp": msg.main.temp_min,
          "outsideHumidity": msg.main.humidity,
      };
      
      
      return {msg: newMsg, metadata: metadata, msgType: msgType};
```
   ![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/weather-rule-chain-node-E.png)     
    
######  节点 F: **Save timeseries node**
        
   - 添加**Script transformation node**并将其连接到关系类型为***Success*的**External REST API call node** 调用节点”。该节点会将消息放入遥测中。
      
   ![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/weather-rule-chain-node-F.png)     

 

## 设置仪表板

下载并[**导入**](/docs/user-guide/ui/dashboards/#dashboard-import)上传的仪表板教程[**文件**](/docs/user-guide/resources/weather_dashboard.json)。

仪表板应如下所示：
![image](/images/user-guide/rule-engine-2-0/tutorials/rest-api-weather/weather-dashboard.png)   


## 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}