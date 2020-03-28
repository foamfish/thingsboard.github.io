---
layout: docwithnav
title: 向客户发送电子邮件
description: 发送电子邮件流程

---

本教程将向您展示如何使用规则引擎向客户发送电子邮件。

* TOC
{:toc}


#### **注意:**
本教程基于[发送警报时发送电子邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/#use-case)教程并且它是用例。我们将重用上述教程中的规则链，并添加更多规则节点，以将电子邮件发送给指定设备的客户。

<br>

# 用例

假设您的设备正在使用DHT22传感器来收集温度读数并将其推送到ThingsBoard。

DHT22传感器适用于-40至80°C的温度读数。如果温度超出范围，我们想生成警报，并在警报创建时发送电子邮件。

在本教程中，我们将ThingsBoard规则引擎配置为：

-如果温度超出范围，即：低于-40度且高于80度，则向指定设备的客户发送电子邮件。

- 将消息始发者属性添加到消息中。

- 使用传入消息中的“脚本转换”节点将其他数据添加到电子邮件正文中。

# 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
  * [创建&清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)。
  * [发送警报邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)。
  
  
# 创建客户并分配设备

首先我们需要创建客户并将设备分配给客户。以下屏幕截图显示了如何执行此操作：

![image](/images/user-guide/rule-engine-2-0/tutorials/email/create-customer.png)

<br>

我们需要分配设备**Thermostat Home**（[创建和清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/#adding-the-device)教程）。<br>

转到客户页面上的**Manage devices**然后选择我们的设备。

![image](/images/user-guide/rule-engine-2-0/tutorials/email/manage-devices.png)

<br>

![image](/images/user-guide/rule-engine-2-0/tutorials/email/assign-device.png)  

<br>

接下来我们的客户应具有**server scope**属性的**email**。请注意邮件将发送到该电子邮箱，因此请编写您的电子邮件进行测试。

![image](/images/user-guide/rule-engine-2-0/tutorials/email/customer-email.png)
 
<br>

此外，我们还需要向服务器**Thermostat Home**添加服务器作用域属性-**address** **：
<br>

转到**Devices** -> **Thermostat Home** -> **Attributes** -> **Server attributes**然后点击 **+** 按钮添加 **address**

![image](/images/user-guide/rule-engine-2-0/tutorials/email/add-address.png)

<br>

# 消息流  

在本节中，我们将说明在本教程中添加或修改到初始规则链的每个节点的用途：

- 节点A: [**Customer attributes**](/docs/user-guide/rule-engine-2-0/enrichment-nodes/#customer-attributes)。
 - 此节点将用于获取客户的电子邮件属性，并将其保存在消息metadata属性customerEmail中
- 节点B: [**Originator attributes**](/docs/user-guide/rule-engine-2-0/enrichment-nodes/#originator-attributes)
- 此节点将用于获取发起者（设备是传入消息的发起者）的地址服务器范围属性，并将其保存在消息Metadata中。
- 节点C: [**To Email**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#to-email-node) node.
  - 此节点从配置的模板构建实际的电子邮件。
- 节点D：**Rule Chain** 节点。
  - 将传入消息转发到指定的规则链**Create/Clear Alarm & Send Email to Customer**。

<br/>

# 配置规则链

在本教程中，我们使用了[发送警报邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)。

我们通过添加以上在[消息流](/docs/user-guide/rule-engine-2-0/tutorials/send-email-to-customer/#message-flow)部分中描述的节点，修改了规则链**Create/Clear Alarm & Send Email**并将此规则链重命名为：**Create/Clear Alarm & Send Email to Customer**。

<br/>以下屏幕截图显示了以上规则链的外观：
 
  - **Create/Clear Alarm & Send Email to Customer:**

![image](/images/user-guide/rule-engine-2-0/tutorials/email/send-email-to-customer-chain.png)

 - **Root Rule Chain:**

![image](/images/user-guide/rule-engine-2-0/tutorials/email/root-rule-chain.png)

<br/> 

下载json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/create_clear_alarm___send_email_to_customer.json)以用于**Create/Clear Alarm & Send Email to Customer:**规则链。
如上图所示，在根规则链中创建节点**D**，以将遥测转发到导入的规则链。

以下部分向您展示了如何修改此规则链特别是：添加规则节点[**A**](/docs/user-guide/rule-engine-2-0/tutorials/send-email-to-customer/#node-a-customer-attributes)和[**B**](/docs/user-guide/rule-engine-2-0/tutorials/send-email-to-customer/#node-b-originator-attributes)并修改节点[**C**](/docs/user-guide/rule-engine-2-0/tutorials/send-email-to-customer/#node-c-to-email)。
<br/> 

  
## 修改**Create & Clear Alarms详情:**

### 修改所需的节点

在此规则链中，您将添加2个节点并修改1个节点，如以下各节所述：

#### 节点A: **Customer attributes**
- 添加**Customer attributes**节点并将其连接到关联类型为**True**的**Filter Script**节点。<br>
  该节点将用于获取客户的**email**属性并将其保存在
  邮件元数据属性**customerEmail**
  
 - 填写下表中输入的数据字段：
 
 <table style="width: 30%">
   <thead>
       <tr>
        <td>Field</td>
        <td>Input Data </td>
       </tr>
   </thead>
   <tbody>
       <tr>
            <td>Name</td>
            <td>Get Customer Email</td>
       </tr>
       <tr>
            <td>Source attribute</td>
            <td>email</td>
       </tr>
       <tr>
            <td>Target attribute</td>
            <td>customerEmail</td>
       </tr>       
   </tbody>
 </table>


![image](/images/user-guide/rule-engine-2-0/tutorials/email/get-customer-email.png)

#### 节点B: **Originator attributes**
- 添加**Originator attributes**节点并将其粘贴在节点之间：**Customer attributes**和**Create alarm**关系类型为**Success**。<br>
  该节点将用于获取始发者 **(Thermostat Home)** 的地址服务器范围属性。此属性将保存在消息元数据属性ss_address中。
  
 - 填写下表中输入的数据字段：
 
 <table style="width: 30%">
   <thead>
       <tr>
        <td>Field</td>
        <td>Input Data </td>
       </tr>
   </thead>
   <tbody>
       <tr>
            <td>Name</td>
            <td>Get Device Address</td>
       </tr>
       <tr>
            <td>Server attributes</td>
            <td>address</td>
       </tr>      
   </tbody>
 </table>


![image](/images/user-guide/rule-engine-2-0/tutorials/email/get-device-address.png)

#### 节点C: **To Email**
- 修改**To Email**节点。为此我们需要更改此节点详细信息中的某些字段：

    - **To template**.

    - **Body tempalte**.
    
- 填写下表中输入的数据字段：
    
<table style="width: 30%">
   <thead>
       <tr>
        <td>Field</td>
        <td>Input Data </td>
       </tr>
   </thead>
   <tbody>
       <tr>
            <td>To template</td>
            <td>${customerEmail}</td>
       </tr>
       <tr>
            <td>Body tempalte</td>
            <td>Device ${deviceName} has unacceptable temperature: ${temperature}. Device address - ${ss_address}</td>
       </tr>      
   </tbody>
 </table>    
  
![image](/images/user-guide/rule-engine-2-0/tutorials/email/modify-to-email.png)  

   
# 进行遥测并验证
对于发布设备遥测我们将使用Rest API[遥测上传API](/docs/reference/http-api/#telemetry-upload-api)。为此我们将需要复制设备**Thermostat Home**的访问令牌。

![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/copy-token.png)


发布temperature = 99. 创建警报:

{% highlight bash %}
curl -v -X POST -d '{"temperature":99}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"

**您需要将$ACCESS_TOKEN替换为实际的设备令牌**
{% endhighlight %}

您应该了解仅在创建警报的情况下，警报更新后才将消息发送到电子邮件。

最后我们可以看到收到的电子邮件具有正确的值。 （如果您没有收到任何电子邮件，请检查您的垃圾邮件文件夹）


![image](/images/user-guide/rule-engine-2-0/tutorials/email/mail-received.png)


此外您可以查看有关如何进行以下操作的更多信息：

- 定义其他用于警报处理的逻辑，例如使用Telegram Bot将通知发送到Telegram App。

- 在**Create and Clear Alarm**节点中配置**Alarm Details**功能并通过添加警报部件以可视化警报来配置仪表板。
  
- 设备离线时创建警报。

请参阅**另请参阅**部分下的链接，以了解如何执行此操作。

<br/>
<br/>

# 另请参阅

- [使用Telegram Bot的智能手机上的通知和警报](/docs/iot-gateway/integration-with-telegram-bot/)。

- [使用详细信息创建警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/)。

- [设备离线时创建警报](/docs/user-guide/rule-engine-2-0/tutorials/create-inactivity-alarm/)。

# 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}  