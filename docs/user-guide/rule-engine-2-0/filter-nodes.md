---
layout: docwithnav
title: 过滤器节点
description: 规则引擎2.0过滤器节点

---

筛选器节点用于邮件筛选和路由。

* TOC
{:toc}

##### 检查过滤器节点关系（Check Relation Filter Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard版本2.0.1+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/filter-check-relation.png)

根据类型和方向检查从所选实体到消息发起者的关系。

![image](/images/user-guide/rule-engine-2-0/nodes/filter-check-relation-config.png)

如果存在关系-消息通过**True**链发送，否则使用**False**链

**注意:** 从Thingsboard 2.3版开始，规则节点可以通过禁用规则节点配置进行根据方向和关系类型检查与特定实体或任何实体的关系是否存在：

![image](/images/user-guide/rule-engine-2-0/nodes/check-relation-checkbox.png)

如果禁用复选框并且存在任何关系-消息通过**True**链发送，否则使用**False**链。

##### 检查存在字段节点（Check Existence Fields Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard版2.3+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/check-existance-fields.png)

规则节点从传入的消息数据和元数据中检查所选键的存在。

![image](/images/user-guide/rule-engine-2-0/nodes/check-existance-fields-config.png)

如果选中复选框**Check that all selected keys are present**表示消息数据和元数据中的所有键是否存在，如果为**True**则通过此链发送消息，否则使用**False**链。<br>

如果未选中此复选框，并且消息的数据或元数据中至少有一个键存在，请通过**True**链发送消息，否则 使用**False**链。

##### 消息类型过滤器节点（Message Type Filter Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard版本2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/filter-message-type.png)

在节点配置中，管理员为传入消息定义了一组允许的消息类型。系统中有[预定义的消息类型](/docs/user-guide/rule-engine-2-0/overview/#predefined-message-types)，例如**Post Attributes**、**Post Telemetry**、**RPC Request**等。

![image](/images/user-guide/rule-engine-2-0/nodes/filter-message-type-config.png)

如果需要传入消息类型-通过**True**链发送消息，否则使用**False**链。

##### 消息类型切换节点（Message Type Switch Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/filter-message-type-switch.png)

按消息类型路由传入的消息。如果传入的消息具有已知的[消息类型](/docs/user-guide/rule-engine-2-0/overview/#predefined-message-types)，则将其发送到相应的链，否则，将消息发送到**Other**链。
如果使用自定义消息类型则可以通过消**Message Type Switch Node** 的**Other**链将这些消息路由 到配置了所需路由逻辑的**Switch Node**或**Message Type Filter Node**。

##### 发起者类型过滤器节点（Originator Type Filter Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>Since TB Version 2.1+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/filter-originator-type.png)

在节点配置中，管理员为传入的消息定义了一组允许的发起者[实休](/docs/user-guide/entities-and-relations/)类型。

![image](/images/user-guide/rule-engine-2-0/nodes/filter-originator-type-config.png)

如果期望传入的发起方类型-通过**True**链发送消息，否则使用**False**链。

##### 发起方类型交换节点（Originator Type Switch Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard版本2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/filter-originator-type-switch.png)

按发起者[实休](/docs/user-guide/entities-and-relations/)类型路由传入消息。

##### 脚本过滤器节点（Script Filter Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard版本2.0</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/filter-script.png)

使用配置的JavaScript条件传入的消息。

JavaScript函数接收3个输入参数：

- <code>msg</code> - 消息payload
- <code>metadata</code> - 消息metadata
- <code>msgType</code> - 消息类型

脚本应返回布尔值。如果为**True**-通过**True**链发送消息，否则使用**False**链。

![image](/images/user-guide/rule-engine-2-0/nodes/filter-script-config.png)
 
消息payload可以通过<code>msg</code>变量访问。例如<code>msg.temperature < 10;</code><br/> 
可以通过<code>metadata</code>变量访问消息。例如<code>metadata.customerName === 'John';</code><br/> 
可以通过<code>msgType</code>变量访问。例如<code>msgType === 'POST_TELEMETRY_REQUEST'</code><br/> 

完整脚本示例:

{% highlight javascript %}
if(msgType === 'POST_TELEMETRY_REQUEST') {
    if(metadata.deviceType === 'vehicle') {
        return msg.humidity > 50;
    } else if(metadata.deviceType === 'controller') {
        return msg.temperature > 20 && msg.humidity > 60;
    }
}

return false;
{% endhighlight %}

可以使用[Test JavaScript function](/docs/user-guide/rule-engine-2-0/overview/#test-javascript-functions)来验证JavaScript条件.

在接下来的教程中，您可以看到使用该节点的真实示例:

- [创建和清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)
- [回复RPC调用](/docs/user-guide/rule-engine-2-0/tutorials/rpc-reply-tutorial/#add-filter-script-node)

##### 交换节点（Switch Node）

<table  style="width:12%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持Thingsboard版本2.0+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/filter-switch.png)

将传入消息路由到一个或多个输出链。节点执行已配置的JavaScript函数。

JavaScript函数接收3个输入参数：

- <code>msg</code> - 消息payload
- <code>metadata</code> - 消息metadata
- <code>msgType</code> - 消息类型
 
该脚本应返回一个将消息路由到的**下一关系名称的数组**。
如果返回的数组为空-消息将不会路由到任何节点并被丢弃。

![image](/images/user-guide/rule-engine-2-0/nodes/filter-switch-config.png)

消息payload可以通过<code>msg</code>变量访问。例如<code>msg.temperature < 10;</code><br/> 
可以通过<code>metadata</code>变量访问消息。例如<code>metadata.customerName === 'John';</code><br/> 
可以通过<code>msgType</code>变量访问。例如<code>msgType === 'POST_TELEMETRY_REQUEST'</code><br/> 

完整脚本示例:

{% highlight javascript %}
if (msgType === 'POST_TELEMETRY_REQUEST') {
    if (msg.temperature < 18) {
        return ['Low Temperature Telemetry'];
    } else {
        return ['Normal Temperature Telemetry'];
    }
} else if (msgType === 'POST_ATTRIBUTES_REQUEST') {
    if (msg.currentState === 'IDLE') {
        return ['Idle State', 'Update State Attribute'];
    } else if (msg.currentState === 'RUNNING') {
        return ['Running State', 'Update State Attribute'];
    } else {
        return ['Unknown State'];
    }
}
return [];
{% endhighlight %}

可以使用[Test JavaScript function](/docs/user-guide/rule-engine-2-0/overview/#test-javascript-functions)测试JavaScript”功能来验证JavaScript切换功能.

为了指定自定义关系名称，应选择自定义类型。这将允许输入自定义关系名称。定制关系名称不区分大小写。

![image](/images/user-guide/rule-engine-2-0/nodes/filter-switch-custom-relation.png)

##### GPS地理围栏过滤器节点(GPS Geofencing Filter Node)

<table  style="width:15%">
   <thead>
     <tr>
	 <td style="text-align: center"><strong><em>支持ThingsBoard版本2.3.1+</em></strong></td>
     </tr>
   </thead>
</table> 

![image](/images/user-guide/rule-engine-2-0/nodes/filter-gps-geofencing.png)

通过参数过滤消息的传入基于GPS的从数据或元数据中提取纬度和经度，并检查它们是否在配置的边界（地理围栏）内。

![image](/images/user-guide/rule-engine-2-0/nodes/filter-gps-geofencing-default-config.png)

默认情况下，规则节点从消息元数据中获取外围信息。如果未**Fetch perimeter information from message metadata**，则应配置其他信息

<br>

###### （从消息元数据中获取边界信息）Fetch perimeter information from message metadata

根据边界类型，有两种区域定义选项:

- 多边形（Polygon)
           
    传入消息的元数据必须包含具有名称**范围**和以下数据结构的密钥：
     
{% highlight java %}[[lat1,lon1],[lat2,lon2], ... ,[latN,lonN]]{% endhighlight %}
 
- 圈（Circle）
                 

{% highlight java %}"centerLatitude": "value1", "centerLongitude": "value2", "range": "value3"

All values for these keys are in double-precision floating-point data type.

The "rangeUnit" key requires specific value from a list of METER, KILOMETER, FOOT, MILE, NAUTICAL_MILE (capital letters obligatory).
{% endhighlight %}

###### 从节点配置中获取周边信息（Fetch perimeter information from node configuration）
 
根据边界类型，有两种区域定义选项：
 
- 多边形（Polygon) 
             
![image](/images/user-guide/rule-engine-2-0/nodes/filter-gps-geofencing-polygon-config.png)           

- 圈（Circle）
                  
![image](/images/user-guide/rule-engine-2-0/nodes/filter-gps-geofencing-circle-config.png)          
    
如果配置的纬度和经度在通过**True**链发送的配置的周界消息内部，则使用**False**链。

在以下情况下将使用**故障**链：

传入消息在数据或元数据中没有配置的纬度或经度键。
缺少周界定义；
        
    