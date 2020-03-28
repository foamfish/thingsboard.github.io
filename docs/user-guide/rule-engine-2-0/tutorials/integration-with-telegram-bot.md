---
layout: docwithnav
title: 使用Telegram Bot的智能手机上的通知和警报

---

* TOC
{:toc}

## 概述

Telegram提供了创建被视为第三方应用程序的Telegram Bot的可能性。

因此在本教程中我们将演示如何创建Telegram Bot 并将您的ThingsBoard规则引擎配置为能够使用Rest API调用扩展将通知发送到Telegram App。

## 用例

本教程基于[创建和清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/#use-case)教程。

我们将重用上述教程中的规则链，并将添加更多规则节点以与Telegram集成

假设您的设备正在使用DHT22传感器来收集温度读数并将其推送到ThingsBoard。

DHT22传感器适用于-40至80°C的温度读数。如果温度超出范围，我们希望生成警报，并在警报创建时向Telegram App发送通知。

在本教程中我们将ThingsBoard规则引擎配置为：

- 如果警报已创建则向用户发送消息通知。

- 使用Script Transform节点将当前警报类型及其始发者添加到消息正文中。

## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
  * [创建&清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)。

## 消息流

在本节中我们将解释本教程中每个节点的用途：

- 节点A: [**Transform Script**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#script-transformation-node)。
- 此节点将用于创建电报消息通知的正文。
- 节点B: [**REST API Call**](/docs/user-guide/rule-engine-2-0/external-nodes/#rest-api-call-node)。
  - 该节点将电报消息payload发送到已配置的Telegram REST API的REST终节点中。

## 创建Telegram Bot

[The BotFather](https://telegram.me/botfather)是主要的机器人可帮助您[create](https://core.telegram.org/bots#6-botfather)新的bot并更改其设置。

Telegram Bot创建完成后为新Telegram Bot生成授权令牌 **'110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw'**。
    
先决条件：

 - ThingsBoard 启动并运行
 - Telegram Bot 创建

### 获取Chat ID

在下一步中，我们需要获取一个Chat ID通过HTTP API发送 Chat ID。

有以下几种获取Chat ID的方法：

 - 首先您需要向Bot发送一些消息：
 
    - 私有聊天；
    
       ![image](/images/gateway/telegram-bot/private-msg-to-bot.png)    
    
    - Bot被添加为成员的组中。
    
       ![image](/images/gateway/telegram-bot/msg-to-bot-in-chat.png)    
      
    <br>**ThingsBoard_Bot**是Telegram Bot名称。

 - 接下来，打开您的网络浏览器并输入以下网址：

```bash
https://api.telegram.org/bot"YOUR_BOT_TOKEN"/getUpdates

"YOUR_BOT_TOKEN" 必须替换为您的机器人的身份验证令牌, 例如：

https://api.telegram.org/bot110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw/getUpdates
```



从即将到来的数据中，您可以找到字段 **'id'**。这就是chat_id。

- 第一种选择：

![image](/images/gateway/telegram-bot/first-option.png)

 - 第二种选择：

![image](/images/gateway/telegram-bot/second-option.png)

之后，您可以开始配置规则引擎以使用Rest API调用扩展。

## 配置Rule Chains

在本教程中，我们使用了[创建和清除警报](/docs/iot-gateway/create-clear-alarms)教程中的规则链。

我们通过添加上述[消息流](/docs/iot-gateway/integration-with-telegram-bot/#message-flow)部分中所述的节点，修改了规则链**Create & Clear Alarms**并将此规则链重命名为：**Create/Clear Alarms & send notifications to Telgram**。

<br/>以下屏幕截图显示了以上规则链的外观：
 
  - **Create/Clear Alarms & send notifications to Telgram:**

![image](/images/gateway/telegram-bot/send-to-telegram-chain.png)

 - **Root Rule Chain:**

![image](/images/gateway/telegram-bot/root-rule-chain.png)

<br/> 

下载json[**文件**](/docs/iot-gateway/resources/create_clear_alarms___send_notifications_to_telgram.json)以**Create/Clear Alarms & send notifications to Telgram**规则链。

下一节将向您展示如何从头开始修改此规则链。
<br/> 

### 修改**Create/Clear Alarm & Send Email**

#### 添加所需的节点

在此规则链中您将创建2个节点如以下各节所述：
 
##### 节点A: **Transform Script**

- 添加**Transform Script** 节点并将其连接到关联类型为**Created**的**Create Alarm**节点。
 <br> 此节点将用于创建消息通知的正文。
<br>模板必须具有2个参数：
  
   - chat_id;
  
   -  text.
  
   这是出站消息的示例：
  
```json
{"chat_id" : "PUT YOUR CHOSEN CHAT_ID", "text" : "SOME MESSAGE YOU WANT TO RECEIVE"}
```
  
 - 请使用以下脚本：
 
 {% highlight javascript %}
 var newMsg ={};
 newMsg.text = '"' +  msg.name + '"' + " alarm was created for device: " + '"' + metadata.deviceName + '"';
 newMsg.chat_id = 337878729; //has to be replaced by the actual chat id
 return {msg: newMsg, metadata: metadata, msgType: msgType};{% endhighlight %}
      
- 输入名称**New telegram message**.  
  
![image](/images/gateway/telegram-bot/transform-script.png)
   
##### 节点B: **REST API Call**
- 添加**REST API Call**节点并将其连接到关联类型为**Success**的**Transform Script**节点。
  
- 此节点将向配置的REST端点发送完整的消息payload。在我们的例子中它是Telegram REST API。
  
- 在本教程的范围内，我们将使用**'/sendMessage'**动作路径来引用Telegram Bot API发送消息。

  
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
            <td>REST API telegram Call</td>
        </tr>
        <tr>
            <td>Endpoint URL pattern</td>
            <td>https://api.telegram.org/bot"YOUR_BOT_TOKEN"/sendMessage</td>
        </tr>
        <tr>
            <td>Request method</td>
            <td>POST</td>
        </tr>
        <tr>
            <td>Header</td>
            <td>Content-Type</td>
        </tr>
        <tr>
            <td>Value</td>
            <td>application/json</td>
        </tr>
     </tbody>
  </table>
     
![image](/images/gateway/telegram-bot/rest-api-telegram-node.png)


# 进行遥测并验证
对于发布设备遥测我们将使用Rest API[遥测上传API](/docs/reference/http-api/#telemetry-upload-api)。为此我们将需要复制设备**Thermostat Home**的访问令牌。

![image](/images/gateway/telegram-bot/copy-token.png)


发布temperature = 99.创建警报:

{% highlight bash %}
curl -v -X POST -d '{"temperature":99}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"

**您需要将$ACCESS_TOKEN替换为实际的设备令牌**
{% endhighlight %}

您应该了解仅在创建警报的情况下，更新警报时不会将消息发送到Telegram App。

最后我们可以看到收到的消息具有正确的值：

- 第一种选择：

![image](/images/gateway/telegram-bot/msg-received-first-way.png)


- 第二种选择：

![image](/images/gateway/telegram-bot/msg-received-second-way.png)


另外您可以：

  - 在Create and Clear Alarm节点中配置Alarm Details功能
    
  - 通过添加警报部件以可视化警报来配置仪表板。
  
  - 定义用于警报处理的其他逻辑例如：发送电子邮件。

请参阅**另请参阅**部分下的的链接，以了解如何执行此操作。
  
<br/>

## 另请参阅

- [创建和清除警报：警报详细信息：](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/#step-2-createupdate-alarm) 指南-了解如何在Alarm节点中配置Alarm Details功能。

- [创建和清除警报：配置信息中心](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/#configure-device-and-dashboard) 指南-了解如何将警报部件添加到仪表板。

- [发送邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)。


## 下一步

{% assign currentGuide = "HardwareSamples" %}{% include templates/guides-banner.md %}
