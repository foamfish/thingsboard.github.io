---
layout: docwithnav
title: ThingsBoard体系结构概述
description: ThingsBoard架构

---

* TOC
{:toc}


ThingsBoard设计为：

* **可扩展**：可水平扩展的平台，使用领先的开源技术构建。
* **容错**：没有单点故障，集群中的每个节点都是相同的。
* **健壮高效**：单个服务器节点可以根据使用情况处理成千上万的设备。
ThingsBoard集群可以处理数百万个设备。
* **可自定义**：使用可自定义的小部件和规则引擎节点可以轻松添加新功能。
* **持久**：永远不会丢失您的数据。

## 10 000 foot view

TODO: put a simple, very high-level diagram here.

## 本地部署与云部署

ThingsBoard支持本地部署和云部署。
在全球运行着超过2000台ThingsBoard服务器之后，ThingsBoard在AWS，Azure，GCE和私有数据中心的生产环境中运行。
可以在完全没有互联网访问的专用网络中启动ThingsBoard。

## 独立vs集群模式

该平台设计为可水平扩展，并支持自动发现新的ThingsBoard服务器（节点）。
集群中的所有ThingsBoard节点都是相同的，并且正在分担负载。
由于所有节点都相同，因此没有“主”或“协调器”过程，也没有单点故障。
您选择的负载均衡器可能会将来自设备，应用程序和用户的请求转发到所有ThingsBoard节点。

## 整体与微服务架构

从ThingsBoard v2.2开始，对平台进行了重构，以支持微服务体系结构，而且还能够以独立模式将其作为整体应用程序运行。
支持这两个选项都需要一些额外的编程工作，但是，由于与各种现有安装的向后兼容性，这一点至关重要。

ThingsBoard始终被设计为可作为分布式应用程序运行，但最初也被设计为整体应用程序。
这意味着在每个服务器节点上只有一个Java进程在运行该应用程序。
这些进程正在使用[gRPC](https://grpc.io/)进行通信，并且服务发现是通过[Zookeeper](https://zookeeper.apache.org/)完成的。
该模型适用于许多安装，并且只需最少的支持工作，知识和硬件资源即可进行设置。

但是，微服务架构还解决了一些挑战，这些挑战适用于更复杂的部署和使用场景。
例如，运行一个多租户部署，其中需要更精细的隔离以防止：

* 不可预测的规则链配置错误；
* 不可预测的负载峰值；
* 由于固件错误，单个设备打开了数千个并发连接；
* 和许多其他情况。
 
请点击下面列出的链接以了解更多信息，然后选择合适的架构和部署选项：

* [**monolithic**](/docs/reference/monolithic)：了解有关以单模式部署，配置和运行ThingsBoard平台的更多信息。
* [**microservices**](/docs/reference/msa)：了解有关在微服务模式下部署，配置和运行ThingsBoard平台的更多信息。

## SQL vs NoSQL vs Hybrid数据库方法

ThingsBard使用数据库进行存储
[实体](/docs/user-guide/entities-and-relations/)设备，资产，客户，仪表板等）和[遥测](/docs/user-guide/telemetry/)数据（属性，时间序列传感器读数，统计信息，事件）。
平台目前支持三个数据库选项：

* **SQL**-将所有实体和遥测存储在SQL数据库中。 ThingsBoard作者建议使用PostgreSQL，这是ThingsBoard支持的主要SQL数据库。
可以将HSQLDB用于本地开发目的。 **除运行测试和启动具有最小可能负载的开发实例外，我们不建议将HSQLDB用于任何其他用途。
* **NoSQL**-将所有实体和遥测存储在NoSQL数据库中。 ThingsBoard作者建议使用Cassandra，这是ThingsBoard目前支持的唯一NoSQL数据库。
但是，由于对托管数据库的部署非常感兴趣，我们计划在v2.3中引入对AWS DynamoDB的支持。
* **Hybrid**-将所有实体存储在SQL数据库中，并将所有遥测存储在NoSQL数据库中。

可以使用**thingsboard.yml**文件配置此选项。有关更多详细信息，请参见数据库[配置](/docs/user-guide/install/config/)页面。

```yaml
database:
  # ...
  entities:
    type: "${DATABASE_ENTITIES_TYPE:sql}" # cassandra OR sql
  ts:
    type: "${DATABASE_TS_TYPE:sql}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```

## 编程语言和第三方

ThingsBoard后端是用Java编写的，但是我们也有一些基于Node.js的微服务。 ThingsBoard前端是基于Angular JS框架的SPA。
有关使用的第三方组件的更多详细信息，请参见[monolithic](/docs/reference/monolithic)和[microservices](/docs/reference/monolithic)页面。