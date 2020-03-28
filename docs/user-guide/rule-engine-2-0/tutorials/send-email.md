---
layout: docwithnav
title: 发送警报邮件
description: 发送邮件流程

---


本教程将向您展示如何使用规则引擎向用户发送电子邮件。

* TOC
{:toc}

## 用例


在本教程中，我们将从教程中实现用例：[创建并清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/#use-case)：

假设您的设备正在使用DHT22传感器来收集温度读数并将其推送到ThingsBoard。

DHT22传感器适用于-40至80°C的温度读数。如果温度超出范围，我们想生成警报，并在警报创建时发送电子邮件。

在本教程中，我们将ThingsBoard规则引擎配置为：

- 如果温度超出范围，即：低于-40和高​​于80度，请向用户发送电子邮件。

- 使用脚本转换节点将当前温度添加到电子邮件正文中，以将当前温度保存在消息Metadata中。


## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
  * [创建&清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)。

# 消息流

在本节中我们将解释本教程中每个节点的用途：

- 节点A: [**Transform Script**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#script-transformation-node)。
  - 该节点将用于在消息Metadata中保存当前温度。
- 节点B: [**To Email**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#to-email-node)。
  - 此节点从配置的模板构建实际的电子邮件。
- 节点C: [**Send Email**](/docs/user-guide/rule-engine-2-0/external-nodes/#send-email-node)。
  - 该节点实际上将使用系统SMTP设置从入站邮件发送电子邮件。

<br/>

# 配置规则链

在本教程中我们使用了[创建和清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms) 教程中的规则链。
我们通过添加[消息流](/docs/user-guide/rule-engine-2-0/tutorials/send-email/#message-flow)部分中上文所述的节点，修改了规则链**Create & Clear Alarms**并将此规则链重命名为**Create/Clear Alarm & Send Email**。<br>

<br/>以下屏幕截图显示了以上规则链的外观：
 
  - **Create/Clear Alarm & Send Email:**

![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/send-email-chain.png)

 - **Root Rule Chain:**

![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/root-rule-chain.png)

<br/> 

下载附件json[**文件**](/docs/user-guide/rule-engine-2-0/tutorials/resources/create_clear_alarm___send_email.json)以用于**Create/Clear Alarm & Send Email**规则链。

下一节将向您展示如何从头开始修改此规则链。
<br/> 
 
## 修改**Create/Clear Alarm & Send Email**

### 添加所需的节点

在此规则链中您将创建3个节点如以下各节所述：
 
#### 节点A: **Transform Script**

- 添加**Transform Script**节点并将其放置在关联类型为**True**的**Filter Script**节点之后然后通过关联**Success**将其连接到**Create Alarm**节点。
 <br>此节点将使用以下脚本将当前温度从消息数据保存到消息元数据：
 
 {% highlight javascript %}
 metadata.temperature = msg.temperature;
 return {msg: msg, metadata: metadata, msgType: msgType};{% endhighlight %}
      
- 输入名称**Add temperature to metadata**.  
  
![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/transform-script.png)
   
#### 节点B: **To Email**
- 添加**To Email**节点并将其连接到关联类型为**Created**的**Create Alarm**节点。
  <br>此节点不发送实际的电子邮件，它仅从配置的模板构造电子邮件。
  <br>因此，您可以使用对消息元数据中存在的任何字段的引用。
  
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
            <td>Temperature Email</td>
        </tr>
        <tr>
            <td>From Template</td>
            <td>info@testmail.org</td>
        </tr>
        <tr>
            <td>To Template</td>
            <td>**Your Email**</td>
        </tr>
        <tr>
            <td>Subject Template</td>
            <td>Device ${deviceType} temperature unacceptable</td>
        </tr>
        <tr>
            <td>Body Template</td>
            <td>Device ${deviceName} has unacceptable temperature: ${temperature}</td>
        </tr>
     </tbody>
  </table>
     
![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/to-email.png)

#### 节点C: **Send Email**
- 添加**Send Email**节点并将其连接到关联类型为**Success**的**To Email**节点。 <br>
  该节点实际上将使用系统SMTP设置从入站邮件发送电子邮件。<br>

- 输入名称**SendGrid SMTP**。

- 如果您无权访问系统管理员帐户，则需要为此节点进行自己的SMTP配置。

- 否则，标记一个字段**Use system SMTP settings**。

请注意，在Demo Server上已将SendGrid提供程序配置为系统SMTP。
 <br/>

以下部分将说明如何配置这些设置的说明。

<br/>

![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/send-email.png)

<br/>

链配置已完成，我们需要保存它。

#### 配置系统SMTP设置

在本节中，我们向您介绍如何配置系统SMTP设置并尝试发送测试电子邮件：

-在本教程的范围内我们将使用**SendGrid**作为SMTP提供程序，而Thingsboard将使用该提供程序发送电子邮件。您可以使用此[连接]](https://app.sendgrid.com/signup)注册进行试用。
  
  登录到SendGrid后，打开SMTP中继[配置页](https://app.sendgrid.com/guide/integrate/langs/smtp)。
   
  ![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/sendgrid-config.png)
  
如果您具有使用系统管理员帐户登录到ThingsBoard的权限，则可以自定义SMTP设置并发送测试电子邮件。
 - 对于默认的系统管理员帐户：

   - login - **sysadmin@thingsboard.org**.
   - password - **sysadmin**.
    
- 转到**System Settings** -> **Outgoing Mail**并配置**Outgoing Mail Settings**如以下屏幕截图所述：

 
![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/test-email.png)

 - 确认您配置了SMTP方法是按**Send Test Email**按钮 <br>


如果系统SMTP配置正确：您将看到一条弹出消息，如上面的屏幕快照所示。<br>
系统SMTP设置配置完成。不要忘记按**Save**按钮。

如果您无法访问系统管理员的帐户，则可以直接在节点中配置SMTP设置，但无法检查电子邮件是否已成功发送。

<br/>

# 进行遥测并验证
对于发布设备遥测我们将使用Rest API[遥测上传API](/docs/reference/http-api/#telemetry-upload-api)。为此我们将需要复制设备**Thermostat Home**的访问令牌。

![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/copy-token.png)


发布temperature = 180 创建警报

{% highlight bash %}
curl -v -X POST -d '{"temperature":180}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"

**您需要将$ACCESS_TOKEN替换为实际的设备令牌**
{% endhighlight %}

您应该了解，仅在创建警报的情况下，警报更新后才将消息发送到电子邮件。

最后我们可以看到收到的电子邮件具有正确的值。 （如果您没有收到任何电子邮件，请检查您的垃圾邮件文件夹）


![image](/images/user-guide/rule-engine-2-0/tutorials/email v2/mail-received.png)


此外您可以查看有关如何进行以下操作的更多信息：

 - 向设备的客户发送电子邮件。
 
 - 从收到的邮件中向电子邮件正文中添加其他数据。

请参阅**另请参阅**部分下的第一个链接，以了解如何执行此操作。

<br/>
<br/>

# 另请参阅*

- [发送电子邮件给客户](/docs/user-guide/rule-engine-2-0/tutorials/send-email-to-customer/)。

- [设备离线时创建警报](/docs/user-guide/rule-engine-2-0/tutorials/create-inactivity-alarm/)。

- [使用详细信息创建警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms-with-details/)。

# 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}

