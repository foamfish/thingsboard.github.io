---
layout: docwithnav
title: 富集节点
description: 规则引擎2.0富集节点

---

丰富节点用于更新传入消息的元数据。

* TOC
{:toc}

##### 客户属性

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-customer-attributes.png)

节点找到消息发起者实体的客户，并将客户属性或最新遥测值添加到消息元数据中。

管理员可以配置原始属性名称和元数据属性名称之间的映射。

节点配置中有**Latest Telemetry**复选框。如果选中此复选框，则节点将获取已配置密钥的最新遥测。否则，Node将获取服务器作用域属性。

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-customer-attributes-config.png)

出站邮件元数据将包含已配置的属性（如果存在）。要访问其他节点中获取的属性，可以使用此模板'<code>metadata.temperature</code>'

允许以下消息发起者类型： **Customer**, **User**, **Asset**, **Device**。

如果找到了不受支持的原始发件人类型，则会引发错误。
如果未为Originator分配客户实体**Failure**链，则使用**Success**链。
在下一个教程中，您可以看到使用该节点的真实示例：
- [发送邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)

##### 设备属性

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-device-attributes.png)

节点使用配置的Query查找消息发起者实体的相关设备，并将属性(client\shared\server scope) 和最新的遥测值添加到消息元数据中。 

属性添加到带有范围前缀的元数据中

- shared属性 -> <code>shared_</code>
- client属性 -> <code>cs_</code>
- server属性 -> <code>ss_</code>
- telemetry -> 无前缀

例如，共享属性“版本”将以名称“ shared_version”添加到元数据中。客户端属性将使用“ cs_”前缀。服务器属性使用“ ss_”前缀。按原样添加到消息元数据中的最新遥测值，不带前缀。

在“设备关系查询”配置中，管理员可以选择所需的**Direction**和**relation depth level**。还可以使用所需的**Device types**集配置**Relation type**。


![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-device-attributes-config.png)

如果找到多个相关实体，则仅第一个实体用于属性丰富，其他实体将被丢弃。

如果未找到相关实体，则使用**Failure**链，否则- **Success**链。

如果找不到属性或遥测，则不会将其添加到消息元数据中，并且仍会通过**Success**链进行路由。

出站邮件元数据将仅包含配置的属性（如果存在）。

要访问其他节点中获取的属性，可以使用此模板'<code>metadata.temperature</code>'


**注意:**  从TB版本2.3.1开始，如果出站消息中没有至少一个选定的密钥，则规则节点可以启用/禁用报告**Failure**。

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-orignator-and-device-attributes-tell-failure.png)

##### 发起者属性

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-originator-attributes.png)

在消息元数据中添加消息发起者属性(client\shared\server scope)和最新的遥测值。 将属性添加到有范围前缀的元数据中:

- shared属性   -> <code>shared_</code>
- client属性  -> <code>cs_</code>
- server属性  -> <code>ss_</code>
- telemetry -> 无前缀

例如，共享属性' version '将被添加到名为' sharedversion '的元数据中。客户端属性将使用' cs '前缀。服务器属性使用“ss_”前缀。将最新的遥测值添加到无前缀的消息元数据中。

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-originator-attributes-config.png)

出站消息元数据包含已配置的属性(如果存在)。

要访问其他节点中获取的属性，可以使用此模板'<code>metadata.cs_temperature</code>'

**Note:** 从TB版本2.3.1开始，如果出站消息中没有至少一个选定的密钥，则规则节点可以启用/禁用报告**Failure**

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-orignator-and-device-attributes-tell-failure.png)

在以下教程中，您可以看到使用该节点的真实示例:

- [转换设备历史消息](/docs/user-guide/rule-engine-2-0/tutorials/transform-telemetry-using-previous-record/)
- [发送邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)

##### 发起者字段

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0.1</em></strong></td>
     </tr>
   </thead>
</table> 


![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-originator-fields.png)

节点获取Message Originator实体的字段值，并将其添加到Message Metadata中。管理员可以配置字段名称和元数据属性名称之间的映射。如果指定的字段不是Message Originator实体字段的一部分，它将被忽略。

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-originator-fields-config.png)

允许以下消息发起者类型: **Tenant**, **Customer**, **User**, **Asset**, **Device**, **Alarm**, **Rule Chain**。

如果发现不支持的发起者类型，则使用**Failure**链否则使用**Success**链.

如果没有找到字段值，则不会将其添加到消息元数据中，而是通过成功链进行路由。 

出站消息元数据将只包含已配置的属性。 

要访问其他节点中获取的属性，可以使用这个模板'<code>metadata.devType</code>'

##### 相关属性

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-related-attributes.png)

节点使用配置的查询查找“消息发起者”实体的“相关实体”，并将“属性”或“最新遥测”值添加到“消息元数据”中。
 
管理员可以配置原始属性名称和元数据属性名称之间的映射。

在“关系查询”配置中，管理员可以选择所需的**Direction**和**relation depth level**. 
还可以使用必需的关系类型和实体类型配置**Relation filters**。

选中**Latest Telemetry**复选框则节点将获取已配置密钥的最新遥测否则使用Node将获取服务器作用域属性. 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-related-attributes-config.png)

如果找到多个相关实体**_only first Entity is used_**属性丰富，其他实体则被丢弃.

如果未找到相关实体，则使用**Failure**链否则使用**Success**链.

出站邮件元数据将包含已配置的属性（如果存在）.

要访问其他节点中获取的属性，可以使用此模板'<code>metadata.tempo</code>'

在下一个教程中，您可以看到使用该节点的真实示例:

- [回复RPC调用](/docs/user-guide/rule-engine-2-0/tutorials/rpc-reply-tutorial/#add-related-attributes-node)

##### 租户属性

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-tenant-attributes.png)

节点找到消息始发者实体的租户，并将租户属性或最新遥测值添加到消息元数据中。

管理员可以配置原始属性名称和元数据属性名称之间的映射。

选中**Latest Telemetry**复选框则节点将获取已配置密钥的最新遥测否则使用Node将获取服务器作用域属性. 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-tenant-attributes-config.png)

出站邮件元数据将包含已配置的属性（如果存在）。要访问其他节点中获取的属性，可以使用此模板'<code>metadata.tempo</code>'

允许以下消息发起者类型: **Tenant**, **Customer**, **User**, **Asset**, **Device**, **Alarm**, **Rule Chain**.

如果找到了不受支持的原始发件人类型，则会引发错误。

如果发起者尚未分配租户实体，则使用**Failure**链否则**Success**链.

##### 发起者遥测

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.1.1</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-originator-telemetry.png)

将在节点配置中选择的特定时间范围内的消息发起者遥测值添加到消息元数据中。 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-originator-telemetry-config.png)

遥测值添加到不带前缀的消息元数据中.

规则节点具有三种获取模式:

 - FIRST: 从数据库中检索最接近时间范围开始的遥测

 - LAST: 从最接近时间范围末尾的数据库中检索遥测

 - ALL: 在指定时间范围内从数据库检索所有遥测.
 
![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-originator-telemetry-fetch-mode.png)

如果选择获取模式**FIRST**或**LAST**, 则出站消息元数据将包含JSON元素(key/value)

否则，如果选定的获取模式**ALL**,则遥测将作为数组获取.

<table  style="width: 60%">
   <thead>
     <tr>
	 <td><strong><em>注意:</em></strong></td>
     </tr>
   </thead>
   <tbody>
     <tr>
	<td>
	<p>规则节点可以将记录的限制大小提取到数组中：1000个记录</p>
	</td>
     </tr>
   </tbody>
</table>

该数组将包含带有时间戳和值的JSON对象。

<table  style="width: 60%">
   <thead>
     <tr>
	 <td><strong><em>注意:</em></strong></td>
     </tr>
   </thead>
   <tbody>
     <tr>
	<td>
	<p>间隔的结尾必须始终小于间隔的开头</p>
	</td>
     </tr>
   </tbody>
</table>

如果选中**Use metadata interval patterns**复选框则规则节点将使用元数据中的Start Interval和End Interval模式。

自UNIX时代(January 1, 1970 00:00:00 UTC)以来，模式单位以毫秒为单位设置。

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-originator-telemetry-patterns.png)

 - 如果消息元数据中不存在任何模式，则出站消息将通过**failure**链进行路由.
 
 - 另外，如果任何模式的数据类型无效，则出站消息还将通过**failure**链进行路由.

出站邮件元数据将包含已配置的遥测字段（如果存在且属于所选范围）.

如果找不到属性或遥测，则不会将其添加到消息元数据中，并且仍会通过**Success**链进行路由. 
 
要访问其他节点中的获取的遥测，可以使用以下模板: <code>JSON.parse(metadata.temperature)</code>

**注意:** 从TB 2.3版开始，当选择“访存”模式: **ALL**时，规则节点可以选择遥测采样顺序。

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-originator-telemetry-order-by.png)

在以下教程中，您可以看到使用该节点的真实示例：

- [遥测增量计算](/docs/user-guide/rule-engine-2-0/tutorials/telemetry-delta-validation/)

##### 租户详细信息

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.3.1</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-tenant-details.png)

规则节点将字段从“租户”详细信息添加到消息正文或元数据

选中**Add selected details to the message metadata**复选框则现有字段将添加到消息元数据中，而不是消息数据中。

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-tenant-details-config.png)

选定的详细信息将添加到带有前缀: **tenant_**. 元数据中。出站邮件将包含已配置的详细信息（如果存在）.

要访问其他节点中的获取的详细信息，可以使用以下模板之一: 

- <code>metadata.tenant_address</code>

- <code>msg.tenant_address</code>

如果发起者尚未分配租户实体，则使用**Failure**链，否则**Success**链.

##### 顾客信息

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.3.1</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-customer-details.png)

规则节点将“客户详细信息”中的字段添加到消息正文或元数据。

选中**Add selected details to the message metadata**复选框则现有字段将添加到消息元数据中，而不是消息数据中。

![image](/images/user-guide/rule-engine-2-0/nodes/enrichment-customer-details-config.png)

选定的详细信息将添加到带有前缀: **customer_**. 元数据中。出站邮件将包含已配置的详细信息（如果存在）.

要访问其他节点中的获取的详细信息，可以使用以下模板之一: 

- <code>metadata.customer_email</code>

- <code>msg.customer_email</code>

允许以下消息发起者类型: **Asset**, **Device**, **Entity View**.
  
如果找到了不受支持的原始发件人类型，则会引发错误.
 
如果未为Originator分配客户实体**Failure**链则使用**Success**链.