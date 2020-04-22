---
layout: docwithnav
title: ThingsBoard整体架构
description: ThingsBoard架构

---

* TOC
{:toc}

## 介绍

本文介绍了整体架构，并由高级示意图组成，
描述各种组件之间的数据流以及做出的一些体系结构选择。

请注意，ThingsBoard v2.2平台支持[**微服务**](/docs/reference/msa/)部署模式。
尽管微服务选项对于高可用性和水平可伸缩的方案是首选，
许多ThingsBoard客户发现，能够从单个ThingsBoard实例开始并在将来进行扩展很有用。

我们还建议使用此模式进行开发和原型制作。

在单片模式下，所有ThingsBoard组件都在单个Java虚拟机（JVM）中启动，并共享相同的OS资源。
由于ThingsBoard是用Java编写的，因此单片式架构的明显优势是可以最小化运行ThingsBoard所需的内存。
您可以在受限的环境中使用256或512 MB的RAM启动和运行ThingsBoard进程。
明显的缺点是，如果使用消息重载一个组件（例如MQTT传输），那么它也可能影响其他组件。
例如，如果ThingsBoard进程的操作系统限制为4096个文件描述符，您无法同时从设备和Websocket用户会话中打开超过4096个MQTT会话。

## 架构图

 <object width="80%" data="/images/reference/mono-architecture.svg"></object> 

## 传输组件

ThingsBoard提供了基于MQTT，HTTP和CoAP的API可用于您的设备应用程序/固件。
每个协议API都是由单独的服务器组件提供的，并且是ThingsBoard“传输层”的一部分。
下面列出了组件的完整列表和相应的文档页面：

* HTTP传输层微服务[API](/docs/reference/http-api/); 
* MQTT传输层微服务[API](/docs/reference/mqtt-api/)与[网关API](/docs/reference/gateway-mqtt-api/)；
* CoAP传输层微服务[API](/docs/reference/coap-api/)。

每个传输组件都将数据推送到规则引擎，并且还可以使用核心服务向数据库发出请求以验证设备凭据等。
 
由于ThingsBoard在传输和核心服务之间使用了非常简单的通信协议，
实现对自定义传输协议的支持非常容易，例如：通过纯TCP的CSV，通过UDP的二进制有效载荷等。
我们建议您查看现有的运输工具[实现](https://github.com/thingsboard/thingsboard/tree/master/common/transport/mqtt) 以开始使用或者如果需要请与[联系我们](/docs/contact-us/)您需要任何帮助。

## 规则引擎组件

ThingsBoard规则引擎负责使用用户定义的逻辑和流程来处理传入的消息。
您可以使用相应的[文档页面](/docs/user-guide/rule-engine-2-0/overview/)了解有关规则引擎的更多信息。

## 核心服务

核心服务负责处理：
 
 * 调用[REST API](/docs/reference/rest-api/);
 * WebSocket [subscriptions](/docs/user-guide/telemetry/#websocket-api) 有关实体遥测和属性更改的信息；
 * 监视设备 [连接状态](/docs/user-guide/device-connectivity-status/)（活动/不活动）。
 
ThingsBoard节点使用Akka actor系统来实现租户，设备，规则链和规则节点actor。
平台节点可以加入每个节点都相等的集群。服务发现是通过Zookeeper完成的。
ThingsBoard节点使用基于实体ID的一致哈希算法在彼此之间路由消息。
因此，用于同一实体的消息在同一ThingsBoard节点上进行处理。平台使用[gRPC](https://grpc.io/) 在ThingsBoard节点之间发送消息。

**注意**：ThingsBoard作者考虑在将来的版本中从gRPC迁移到Kafka，以便在ThingsBoard节点之间交换消息。
主要思想是牺牲性能/等待时间的小损失，以支持由卡夫卡用户群提供的持久可靠的消息传递和自动负载平衡。

## 外部系统

可以通过规则引擎将消息从ThingsBoard推送到外部系统。
您可以将数据推送到外部系统，处理数据并将处理结果报告回ThingsBoard以进行可视化。
请查看规则引擎文档和指南以获取更多详细信息。
  