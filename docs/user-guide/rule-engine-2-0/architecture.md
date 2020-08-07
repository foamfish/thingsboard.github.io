---
layout: docwithnav
title: 规则引擎架构
description: 规则引擎架构

---

* TOC
{:toc}

ThingsBoard规则引擎基于两个主要组件：参与者模型和消息队列。

<br/>

![image](/images/user-guide/rule-engine-2-0/rule-engine-architecture.svg)
 
### Actor模型

<<<<<<< HEAD
只要服务器端API调用，Actor模型就可以实现高性能和并发处理来自设备传输层的消息。
ThingsBoard使用Akka作为actor系统实现。与规则引擎相关的主要参与者有两个：Rule Chain Actor和Rule Node Actor。
=======
Actor model enables high performance and concurrent processing of messages from device transport layer as long as server-side API calls. 
ThingsBoard uses own Actor System implementation that is sharpened for our use case. 
There are two main actors related to the rule engine: Rule Chain Actor and Rule Node Actor.
>>>>>>> master

#### 规则链Actor

规则链参与者负责规则节点配置，在规则节点之间路由消息以及处理队列放置和确认命令。每个Rule Chain Actor代表用户配置的单个规则链。
Rule Chain Actor是多个Rule Node Actor的父级。

#### 规则节点Actor

规则节点参与者负责处理传入消息。消息处理的逻辑是高度可定制的。RuleNode有许多内置的实现，您也可以开发自己的自定义规则节点实现。有关更多详细信息，请参见[规则节点开发](/docs/user-guide/contribution/rule-node-development/)指南。

 
## 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}