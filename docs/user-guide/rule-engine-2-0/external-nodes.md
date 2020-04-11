---
layout: docwithnav
title: 外部节点
description: 规则引擎2.0外部节点

---

所使用的外部节点用于与外部系统进行交互。

* TOC
{:toc}


# AWS SNS Node

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/external-aws-sns.png)

外部节点指用来与外部系统交互的节点 AWS SNS (Amazon Simple Notification Service).

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/external-aws-sns-config.png)

- **Topic ARN pattern** - 可以为消息发布设置直接主题名，也可以使用模式，使用消息元数据将其解析为真正的主题名. 
- **AWS Access Key ID** and **AWS Secret Access Key** 是具有编程访问权限的AWS IAM用户凭据。更多关于AWS访问密钥的信息，请访问[这里](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)找到。 
- **AWS Region** 必须对应于创建SNS主题的区域。AWS区域当前列表可以在[这里](http://docs.aws.amazon.com/general/latest/gr/rande.html)找到。

在下面的例子中，主题名取决于设备类型，在元数据中有一条消息包含**deviceType**字段
{% highlight javascript %}
{
    deviceType: controller
}
{% endhighlight %}

我们将在**Topic ARN pattern**中设置**controller**的topic中发布消息:

{% highlight bash %}
arn:aws:sns:us-east-1:123456789012:${deviceType}
{% endhighlight %}

在运行时，模式将解析为 <code>arn:aws:sns:us-east-1:123456789012:controller</code>

**Published payload** - —节点将向SNS发布完整的有效消息负载。如果有需要的话，可以配置规则链，使用转换节点链向SNS发送正确的负载。

**Outbound message** 将包含消息元数据中的响应**messageId**和**requestId**。原始消息有效负载、类型和发送方不会被更改。

<br/>

# AWS SQS Node

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/external-aws-sqs.png)

节点将消息发布到AWS SQS (亚马逊简单队列服务)。

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/external-aws-sqs-config.png)

- **Queue Type** -SQS队列类型可以是*Standard*，也可以是先进先出类型*FIFO*.
- **Queue URL Pattern** - 用于构建队列URL的模式。例如,<code>${deviceType}</code>。可以为消息发布设置直接队列URL，或使用模式，使用消息元数据将该模式解析为真正的队列URL。
- **Delay** - 以秒为单位的延迟，用于延迟特定的消息.
- **Message attributes** - 可选的要发布的消息属性列表.
- **AWS Access Key ID** and **AWS Secret Access Key** 是具有编程访问权限的AWS IAM用户的凭证。可以在[此处](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)找到有关AWS访问密钥的更多信息
- **AWS Region** 必须与创建SQS队列的区域相对应。可在[此处](http://docs.aws.amazon.com/general/latest/gr/rande.html)找到AWS区域的最新列表。

在以下示例中，队列URL取决于设备类型，并且在元数据中有一个包含 **deviceType** 字段的消息:

{% highlight json %}
{
    deviceType: controller
}
{% endhighlight %}

为了在 **controller**'s 的Queue中发布消息，我们将在**Queue URL pattern**模式中设置此模式:

{% highlight bash %}
https://sqs.us-east-1.amazonaws.com/123456789012/${deviceType}
{% endhighlight %}

在运行时，模式将解析为<code>https://sqs.us-east-1.amazonaws.com/123456789012/controller</code>

**Published body** - 节点将向SQS发布完整的消息有效负载。如果需要，可以将规则链配置为使用转换节点链，以将正确的有效负载发送到SQS。

**Published attributes** - 可以添加可选的属性列表以在SQS中发布消息。这是一个集合 -- 对。NAME和VALUE都可以是静态值或模式，可以使用消息元数据进行解析。

如果选择了**FIFO**队列，则消息ID将用作重复数据**deduplication ID**，消息发起者将用作**group ID**。

**Outbound message** 这个节点的出站消息将包含消息元数据中的响应 **messageId**, **requestId**, **messageBodyMd5**, **messageAttributesMd5** 
和 **sequenceNumber** 原始消息payload、类型和发送方不会被更改。

<br/>

# Kafka Node

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/external-kafka.png)

Kafka节点向Kafka代理发送消息。消息可具有任何消息类型。将通过Kafka生产者发送记录到Kafka服务器.

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/external-kafka-config.png)

- **Topic pattern** - 可以是静态字符串，也可以是使用消息元数据属性解析的模式。例如<code>${deviceType}</code>
- **bootstrap servers** - 用逗号分隔的kafka代理列表.
- **Automatically retry times** - 如果连接失败，尝试重发消息的次数.
- **Produces batch size** - 以字节为单位的批处理大小，用于对具有相同分区的消息进行分组.
- **Time to buffer locally** - ms中最大的本地缓冲窗口持续时间.
- **Client buffer max size** - 发送消息的最大缓冲区大小(以字节为单位).
- **Number of acknowledgments** - 确认节点在考虑请求完成之前需要接收的数量.
- **Key serializer** - 默认是org.apache.kafka.common.serialization.StringSerializer
- **Value serializer** - 默认是org.apache.kafka.common.serialization.StringSerializer
- **Other properties** - 可以为kafka代理连接提供任何其他属性.

**Published body** - Node将向Kafka主题发送完整的消息负载。如果需要可以配置规则链，使用转换节点链向Kafka发送正确的payload.

**Outbound message** 此节点的出站消息将在消息元数据中包含响应**offset**, **partition** and **topic**属性。原始消息payload、类型和发送方不会被更改。

<br/>

# MQTT Node

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/external-mqtt.png)

使用QoS **AT_LEAST_ONCE**将传入消息有效负载发布到已配置的MQTT代理的主题

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/external-mqtt-config.png)

- **Topic pattern** - 可以是静态字符串，也可以是使用消息元数据属性解析的模式。例如<code>${deviceType}</code>.
- **Host** - MQTT代理主机.
- **Port** - MQTT代理端口.
- **Connection timeout** - 连接到MQTT代理的超时(秒).
- **Client ID** - 可选的客户端标识符，用于连接到MQTT代理。如果没有指定，将使用默认生成的clientId.
- **SSL Enable/Disable** - 启用/禁用安全通信.  
- **Credentials** - MQTT连接凭据。可以是匿名的，基本的或者PEM。

外部MQTT代理支持不同的身份验证凭证:

- Anonymous - 没有身份验证
- Basic - 用户名\密码对用于认证
- PEM - PEM证书用于身份验证

**PEM** PEM证书用于身份验证 如果选择PEM凭证类型:

- CA证书文件
- 证书文件
- 私钥文件
- 私钥密码

<br/>

**Published body** - 节点将向MQTT主题发送完整的消息有效负载。如果需要，可以将规则链配置为使用转换节点链，以将正确的有效负载发送到MQTT代理.

在成功发布消息的情况下，原始消息将通过**Success**链传递到下一个节点，否则将使用**Failure**链。

<br/>

# RabbitMQ Node

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/external-rabbitmq.png)

将传入的消息有效负载发布到RabbitMQ.

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/external-rabbitmq-config.png)

- **Exchange name pattern** - 发布消息时所做的交换。可以是静态字符串，也可以是使用消息元数据属性解析的模式。例如<code>${deviceType}</code> .
- **Routing key pattern** - 路由密钥。可以是静态字符串，也可以是使用消息元数据属性解析的模式。例如 <code>${deviceType}</code> .
- **Message properties** - 可选的路由headers。支持*TEXT_PLAIN*, *MINIMAL_BASIC*, *MINIMAL_PERSISTENT_BASIC*, *PERSISTENT_BASIC*, *PERSISTENT_TEXT_PLAIN*
- **Host** - 用于连接的默认主机
- **Port** - 用于连接的默认端口
- **Virtual host** - 连接代理时要使用的虚拟主机
- **Username** - AMQP用户名，在连接代理时使用
- **Password** - AMQP连接代理时使用的密码
- **Automatic recovery** - 启用或禁用自动连接恢复
- **Connection timeout** - 连接建立TCP超时，以毫秒为单位;零表示无限
- **Handshake timeout** - AMQP0-9-1协议握手超时，以毫秒为单位
- **Client properties** - 启动连接时发送到服务器的附加属性 

**Published body** - 已发布的主体-节点将向RabbitMQ发送完整的消息有效负载。

如果需要，可以配置规则链，使用转换节点链发送正确的负载。 如果消息发布成功，原始消息将通过**Success**链传递到下一个节点，否则将使用**Failure**链。

<br/>

# REST API Call Node

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/external-rest-api-call.png)

REST API调用外部REST服务器。

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/external-rest-api-call-config.png)

- **Endpoint URL pattern** - 可以是静态字符串，也可以是使用消息元数据属性解决的模式。例如 <code>${deviceType}</code>
- **Request method** - *GET*, *POST*, *PUT*, *DELETE*
- **Headers** - 请求Headers、Headers或值可以是静态字符串，也可以是使用消息元数据属性解析的模式。

**Endpoint URL**

URL可以是静态字符串或patterns。只使用消息元数据解析模式。因此，模式中使用的属性名必须存在于消息元数据中，否则原始模式将被添加到URL中。 

例如，如果消息payload包含带有值**container**的属性**deviceType**，则此模式: 

<code>http://localhost/api/${deviceType}/update</code> 

被解析为

<code>http://localhost/api/container/update</code>   

**Headers**

可以配置标题名称/值的集合。 这些标题将被添加到Rest请求中。 模式应用于配置标头名称和标头值。
例如 <code>${deviceType}</code>. 仅消息元数据用于解决模式。因此，模式中使用的属性名称必须存在于消息元数据中，否则原始模式将添加到标头中。 

**Request body** -节点将向配置的REST端点发送完整的消息Payload。如果需要，可以将规则链配置为使用转换节点链来发送正确的Payload。

**Outbound message** 将在消息元数据中包含响应状态，**status**, **statusCode**, **statusReason**和**headers**。
出站消息有效负载将与响应正文相同。原始邮件类型和原始发件人将不会更改。

如果请求成功，则出站消息将通过**Success**链传递到下一个节点，否则将使用**Failure**链

<br/>

# Send Email Node

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/external-send-email.png)

节点通过已配置的邮件服务器发送传入消息。此节点只适用于在创建时使用[**Email**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#to-email-node)的消息，请使用**Success**链将此节点与**To Email**节点连接。

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/external-send-email-config.png)

- **Use system SMTP settings** - 如果启用，将使用在系统级别配置的默认邮件服务器
- **Protocol** - 邮件服务器传输协议：*SMTP* 或 *SMTPS*
- **SMTP host** - 邮件服务器主机
- **SMTP port** - 邮件服务器端口
- **Timeout ms** - 读取超时，以毫秒为单位
- **Enable TLS** - 如果为true，则启用STARTTLS命令的使用（如果服务器支持）
- **Username** - 邮件主机上帐户的用户名（如果有）
- **Password** - 邮件主机上帐户的密码（如果有）

该节点可以与在系统级别配置的默认邮件服务器一起使用。
请找到有关 [如何配置默认系统SMTP设置的详细信息](/docs/user-guide/ui/mail-settings/)

如果此节点需要特定的邮件服务器，请禁用**Use system SMTP settings**复选框，并手动配置邮件服务器。
该节点可以与在系统级别配置的默认邮件服务器一起使用。[有关如何配置默认系统SMTP设置](/docs/user-guide/ui/mail-settings/)请找到的更多详细信息。
如果此节点需要特定的邮件服务器，请禁用使用系统SMTP设置复选框并手动配置邮件服务器

<br/>

另外，如果传入消息已参考数据库中存储的文件准备了**附件**字段，则此节点可以创建电子邮件附件。

**注意**: 这是[ThingsBoard Professional Edition](/products/thingsboard-pe/)支持的[文件存储](/docs/user-guide/file-storage/)功能的一部分。

<br/>

如果成功发送邮件，原始消息将通过**Success**链传递到下一个节点，否则将使用**Failure**链。

在下一个教程中，您可以看到使用该节点的真实示例:

- [邮件发送](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)

<br/>

# Twilio SMS 节点

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0.2</em></strong></td>
     </tr>
   </thead>
</table> 

{% assign rulenode = "Twilio SMS" %}{% include templates/pe-rule-node-banner.md %}

![image](/images/user-guide/rule-engine-2-0/nodes/external-twilio-sms.png)

通过Twilio服务将传入消息有效负载作为SMS消息发送。

配置:

![image](/images/user-guide/rule-engine-2-0/nodes/external-twilio-sms-config.png)

- **Phone Number From** - 可以设置直接电话号码，因为可以使用SMS的号码发件人或可以使用模式，将其解析为使用消息元数据的真实号码发件人。
- **Phone Numbers To** - 逗号分隔的收件人电话号码列表。可以设置直接电话号码，也可以使用模式，这将使用消息元数据解析为真实电话号码。
- **Twilio Account SID** - 位于twilio.com/console的Sid帐号
- **Twilio Account Token** -  位于twilio.com/console的令牌

SMS消息将发送至**电话号码至**列表中的所有收件人。

如果将SMS消息成功发送给所有收件人，则原始消息将通过**Success**链传递到下一个节点，否则将使用**Failure**链。
