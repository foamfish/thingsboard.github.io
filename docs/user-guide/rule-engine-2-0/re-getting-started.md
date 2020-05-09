---
layout: docwithnav
title: 规则引擎入门
description: 规则引擎入门

---

* TOC
{:toc}

## 什么是规则引擎?
规则引擎是基于事件构建的工作流是易于使用的框架。有3个主要组成部分:

- **Message** - 接收任何事件。它可以是来自Device，设备生命周期事件，REST API事件，RPC请求等的传入数据。
- **Rule Node** - 处理消息执行的功能。对接收的节点进行过滤、转换或者执行的能力。 
- **Rule Chain** - 关联节点之的连接，接收来自节点的出站消息将其发送至下一个节点。


## 典型实例 
ThingsBoard规则引擎是一个高度可定制的框架，用于复杂事件的处理。以下是一些可以通过ThingsBoard规则链配置的常见用例：

- 在保存到数据库之前，对接收的遥测数据或属性进行验证和修改。
- 将遥测或属性从设备复制到相关资产，以便可以汇总遥测。例如，可以将多个设备中的数据汇总到相关资产中。
- 根据定义的条件对alarms进行创建、更新、清除。
- 根据设备生命周期事件触发操作。例如，如果设备处于在线/离线状态，则创建警告。
- 加载所需的其他处理数据。例如，在客户设备或租户属性中定义的设备的负载温度阈值。
- 触发对外部系统的REST API调用。
- 发生复杂事件时发送电子邮件，并使用“电子邮件模板”中其他实体的属性。
- 在事件处理期间要考虑用户的偏好。
- 根据定义的条件进行RPC调用。
- 与外部管道（如Kafka，Spark，AWS服务等）集成。

## Hello World 实例

你可以使用ThingsBoard平台将DHT22温度传感器采集的-40°C至+ 80°C温度值进行收集。

在此教程中我们将配置ThingsBoard规则引擎来存储-40至80°C范围内的所有温度，并将所有数据记录到系统日志中。

#### 添加温度并验节点
进入Thingsboard UI中**Rule Chains**转到**Root Rule Chain**.

![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/initial-root-chain.png)

拖动**Script Filter** 规则节点放入链中并配置如下脚本:

{% highlight javascript %}
return typeof msg.temperature === 'undefined' 
        || (msg.temperature >= -40 && msg.temperature <= 80);
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/script-config.png)

如果未定义温度属性或温度有效则脚本将返回**True**，否则将返回**False**。如果脚本返回**True**则传入消息将被关联到与**True**关系连接的下一个节点。
 
我们希望所有的**telemetry requests**都通过此脚本进行验证. 删除**Message Type Switch**节点和**Save Telemetry**节点之间的**Post Telemetry**关系节点。

![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/remove-relation.png)
  
将**Message Type Switch**节点和将**Script Filter**使用**Post Telemetry**进行连接:
   
![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/realtion-window.png)

![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/connect-script.png)

将**Script Filter**节点与**Save Telemetry**节点使用关系**True**进行连接：

![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/script-to-save.png)

将**Script Filter**节点与**Log Other**节点使用关系**False**进行连接这样无效数据将被记录在系统日志中：

![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/false-log.png)

点击保存按钮应用更新。

#### 验证结果
创建设备并将遥测提交到Thingsboard，点击**Devices**并创建新的设备：

![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/create-device.png)

可以使用设备令牌进行[Rest API](/docs/reference/http-api/#telemetry-upload-api)提交遥测数据提交，
![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/copy-access-token.png)

提交temperature = 99的值，可以进行**Latest Telemetry**中查看，发现并未加成功：

{% highlight bash %}
curl -v -X POST -d '{"temperature":99}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

***替换掉$ACCESS_TOKEN为实际设备的Token**

![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/empty-telemetry.png)


提交temperature = 24可以看见遥测数据保存成功

{% highlight bash %}
curl -v -X POST -d '{"temperature":24}' http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
{% endhighlight %}

![image](/images/user-guide/rule-engine-2-0/tutorials/getting-started/saved-ok.png)


## 相关文档:

你可以通过以下链接了解Thingsboard规则引擎的更多信息：

- [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)
- [规则引擎架构](/docs/user-guide/rule-engine-2-0/architecture/)
- [调试节点执行](/docs/user-guide/rule-engine-2-0/overview/#debugging)
- [验证传入遥测](/docs/user-guide/rule-engine-2-0/tutorials/validate-incoming-telemetry/)
- [转换输入遥测](/docs/user-guide/rule-engine-2-0/tutorials/transform-incoming-telemetry/)
- [历史记录转换遥测](/docs/user-guide/rule-engine-2-0/tutorials/transform-telemetry-using-previous-record/)
- [创建&清除警报](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)
- [发送警报邮件](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)
- [设备离线创建警报](/docs/user-guide/rule-engine-2-0/tutorials/create-inactivity-alarm/)
- [检查实体之间关系](/docs/user-guide/rule-engine-2-0/tutorials/check-relation-tutorial/)
- [RPC请求相关设备](/docs/user-guide/rule-engine-2-0/tutorials/rpc-request-tutorial/)
- [动态分组中添加删除设备](/docs/user-guide/rule-engine-2-0/tutorials/add-devices-to-group/)
- [汇总传入数据流](/docs/user-guide/rule-engine-2-0/tutorials/aggregate-incoming-data-stream/)

<br/>
<br/>

## 下一步

{% assign currentGuide = "GettingStartedGuides" %}{% include templates/guides-banner.md %}
