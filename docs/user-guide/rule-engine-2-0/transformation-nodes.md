---
layout: docwithnav
title: 转换节点
description: 规则引擎2.0转换节点

---

转换节点用于更改传入的消息字段，例如发起方，消息类型，有效负载和元数据。

* TOC
{:toc}


# 变更发起人

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/transformation-change-originator.png)

Thingsboard中的所有传入消息都有“发起者”字段，该字段标识提交消息的实体。它可以是设备，资产，客户，租户等。

在应将提交的消息作为来自另一个实体的消息处理的情况下，使用此节点。例如，设备提交遥测，并且遥测应复制到更高级别的资产或客户中。在这种情况下，管理员应在[**Save Timeseries**](/docs/user-guide/rule-engine-2-0/action-nodes/#save-timeseries-node)节点之前添加此节点。

发起者可以更改为:

- 发起人的客户
- 发起人的租户
- 关系查询所标识的相关实体

在'Relations query'管理员可以选择所需的**Direction**和**relation depth level**。
还可以使用必需的关系类型和实体类型配置**Relation filters**。

![image](/images/user-guide/rule-engine-2-0/nodes/transformation-change-originator-config.png)

如果找到多个相关实体，则**_only the first Entity is used_**新的发起者，其他实体则被丢弃。

如果未找到相关实体/客户/租户，则使用**Failure**链否则**Success**链。

出站邮件将具有新的发起者ID。

<br/>

# 脚本转换节点

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/transformation-script.png)

Changes Message payload, Metadata or Message type using configured JavaScript function.
使用已配置的JavaScript函数更改消息payload、Metadata或消息类型

JavaScript函数接收3个输入参数：

- <code>msg</code> - 消息payload
- <code>metadata</code> - 消息metadata
- <code>msgType</code> - 消息类型

脚本应返回以下结构
{% highlight java %}
{   
    msg: new payload,
    metadata: new metadata,
    msgType: new msgType 
}
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/nodes/transformation-script-config.png)

结果对象中的所有字段都是可选的，如果未指定，则将从原始消息中获取

来自此节点的出站消息将是使用已配置的JavaScript函数构造的新消息。

JavaScript转换功能可以使用[Test JavaScript function](/docs/user-guide/rule-engine-2-0/overview/#test-javascript-functions)进行验证.

<br/>
**Example**

节点收到带有**payload**的消息:
{% highlight java %}
{
    "temperature": 22.4,
    "humidity": 78
}
{% endhighlight %}

Original **Metadata**:
{% highlight java %}
{ "sensorType" : "temperature" }
{% endhighlight %}


Original **Message Type** - POST_TELEMETRY_REQUEST
<br/>

应该执行以下修改:

- 将消息类型更改为'CUSTOM_UPDATE' 
- 将其他属性**_version_**添加到payload中值为**_v1.1_**
- 将元数据中的_**sensorType**_属性值更改为**_roomTemp_**

以下转换函数将执行所有必要的修改:
{% highlight java %}
var newType = "CUSTOM_UPDATE";
msg.version = "v1.1";
metadata.sensorType = "roomTemp"
return {msg: msg, metadata: metadata, msgType: newType};
{% endhighlight %}

您可以在这些教程中看到实际示例，以及如何使用此节点：

- [转换输入遥测](/docs/user-guide/rule-engine-2-0/tutorials/transform-incoming-telemetry/)
- [回复RPC调用](/docs/user-guide/rule-engine-2-0/tutorials/rpc-reply-tutorial.md#add-transform-script-node)

# 到电子邮件节点

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/transformation-to-email.png)

通过使用从消息元数据派生的值填充电子邮件字段，将消息转换为电子邮件消息.
设置 'SEND_EMAIL' 输出消息类型以后可以被 [**Send Email Node**](/docs/user-guide/rule-engine-2-0/external-nodes/#send-email-node)
接受。可以将所有电子邮件字段配置为使用元数据中的值.
  
![image](/images/user-guide/rule-engine-2-0/nodes/transformation-to-email-config.png)

例如，传入消息在元数据中具有 **deviceName** 字段，并且电子邮件正文应包含其值.

在这种情况下，可以像下面的示例一样引用 **deviceName** 值 <code>${deviceName}</code>:

 ```
 Device ${deviceName} has high temperature
 ```
 
<br/>

另外，如果传入消息元数据包含引用存储在数据库中的文件的 **attachments** 字段，则此节点可以准备电子邮件附件. 

**NOTE**: 这是[ThingsBoard Professional Edition](/products/thingsboard-pe/)支持的[File Storage](/docs/user-guide/file-storage/)功能的一部分。

<br/>

在下一个教程中，您可以看到使用该节点的真实示例:

- [发电子邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)


