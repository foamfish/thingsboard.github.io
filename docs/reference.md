---
layout: docwithnav
<<<<<<< HEAD
title: ThingsBoard体系结构概述
description: ThingsBoard架构
=======
title: ThingsBoard architecture
description: ThingsBoard architecture
>>>>>>> master

---

* TOC
{:toc}

## ThingsBoard services

ThingsBoard设计为：

<<<<<<< HEAD
* **可扩展**：可水平扩展的平台，使用领先的开源技术构建。
* **容错**：没有单点故障，集群中的每个节点都是相同的。
* **健壮高效**：单个服务器节点可以根据使用情况处理成千上万的设备。
ThingsBoard集群可以处理数百万个设备。
* **可自定义**：使用可自定义的小部件和规则引擎节点可以轻松添加新功能。
* **持久**：永远不会丢失您的数据。
=======
* **scalable**: horizontally scalable platform, build using leading open-source technologies.
* **fault-tolerant**: no single-point-of-failure, every node in the cluster is identical.
* **robust and efficient**: single server node can handle tens or even hundreds thousands of devices depending on use-case. 
ThingsBoard cluster can handle millions of devices.
* **durable**: never lose your data. ThingsBoard supports various queue implementations to provide extremely high message durability.
* **customizable**: adding new functionality is easy with customizable widgets and rule engine nodes.
>>>>>>> master


The diagram below shows key system components and interfaces they provide. Let's walk through them.



 <object width="100%" data="/images/reference/thingsboard-architecture.svg"></object>



**ThingsBoard Transports**
 
ThingsBoard provides [MQTT](/docs/reference/mqtt-api/), [HTTP](/docs/reference/http-api/) and [CoAP](/docs/reference/coap-api/) based APIs that are available for your device applications/firmware. 
Each of the protocol APIs are provided by a separate server component and is part of ThingsBoard "Transport Layer". 
MQTT Transport also provides [Gateway APIs](/docs/reference/gateway-mqtt-api/) to be used by gateways that represent multiple connected devices and/or sensors.

Once the Transport receives the message from device, it is parsed and pushed to durable [Message Queue](/docs/reference/#message-queues-are-awesome). 
The message delivery is acknowledged to device only after corresponding message is acknowledged by the message queue.

**ThingsBoard Core**

ThingsBoard Core is responsible for handling [REST API](/docs/reference/rest-api/) calls and WebSocket [subscriptions](/docs/user-guide/telemetry/#websocket-api).
It is also responsible for storing up to date information about active device sessions and monitoring device [connectivity state](/docs/user-guide/device-connectivity-status/).
ThingsBoard Core uses Actor System under the hood to implement actors for main entities: tenants and devices. 
Platform nodes can join the cluster, where each node is responsible for certain partitions of the incoming messages.

**ThingsBoard Rule Engine**

ThingsBoard Rule Engine is the heart of the system and is responsible for processing incoming [messages](/docs/user-guide/rule-engine-2-0/overview/#rule-engine-message).
Rule Engine uses Actor System under the hood to implement actors for main entities: rule chains and rule nodes.
Rule Engine nodes can join the cluster, where each node is responsible for certain partitions of the incoming messages.

Rule Engine subscribes to incoming data feed from queue(s) and acknowledge the message only once it is processed. 
There are multiple strategies available that control the order or message processing and the criteria of message acknowledgement.
See [submit strategies](/docs/user-guide/rule-engine-2-0/overview/#queue-submit-strategy) and [processing strategies](/docs/user-guide/rule-engine-2-0/overview/#queue-processing-strategy)
for more details.

ThingsBoard Rule Engine may operate in two modes: shared and isolated. In shared mode, rule engine process messages that belong to multiple tenants.
In isolated mode Rule Engine may be configured to process messages for specific tenant only. 

**ThingsBoard Web UI**

ThingsBoard provides a lightweight component written using Express.js framework to host static web ui content. 
Those components are completely stateless and no much configuration available. 
The static web UI contains application bundle. Once it is loaded, the application starts using the REST API and WebSockets API provided by ThingsBoard Core.  
 
## Message Queues are awesome!

ThingsBoard supports multiple message queue implementations: Kafka, RabbitMQ, AWS SQS, Azure Service Bus and Google Pub/Sub. We plan to extend this list in the future.
Using durable and scalable queues allow ThingsBoard to implement back-pressure and load balancing. Back-pressure is extremely important in case of peak loads.  
We provide "abstraction layer" over specific queue implementations and maintain two main concepts: topic and topic partition. 
One topic may have configurable number of partitions. Since most of the queue implementations does not support partitions, we use *topic + "." + partition* pattern.
  
ThingsBoard message Producers determines which partition to use based on the hash of entity id. 
Thus, all messages for the same entity are always pushed to the same partition.
ThingsBoard message Consumers coordinate using Zookeeper and use consistent-hash algorithm to determine list of partitions that each Consumer should subscribe to.
While running in microservices mode, each service also has the dedicated "Notifications" topic based on the unique service id that has only one partition.      
   
ThingsBoard uses following topics:

 * **tb_transport.api.requests**: to send generic API calls to check device credentials from Transport to ThingsBoard Core.
 * **tb_transport.api.responses**: to receive device credentials verification results from ThingsBoard Core to Transport.
 * **tb_core**: to push messages from Transport or Rule Engine to ThingsBoard Core. Messages include session lifecycle events, attribute and RPC subscriptions, etc.
 * **tb_rule_engine**: to push messages from Transport or ThingsBoard Core to Rule Engine. Messages include incoming telemetry, device states, entity lifecycle events, etc.
 
**Note:** All topic properties including names and number of partitions are [configurable](/docs/user-guide/install/config/) via thingsboard.yml or environment variables. 
We plan to enable configuration via UI in ThingsBoard 2.6 and/or 3.1. 

**Note:** Starting version 2.5 we have switched from using [gRPC](https://grpc.io/) to  [Message Queues](/docs/reference/#message-queues-are-awesome)
for all communication between ThingsBoard components. 
The main idea was to sacrifice small performance/latency penalties in favor of persistent and reliable message delivery and automatic load balancing.  

## 本地部署与云部署

<<<<<<< HEAD
ThingsBoard支持本地部署和云部署。
在全球运行着超过2000台ThingsBoard服务器之后，ThingsBoard在AWS，Azure，GCE和私有数据中心的生产环境中运行。
可以在完全没有互联网访问的专用网络中启动ThingsBoard。
=======
ThingsBoard supports both on-premise and cloud deployments. 
With more then 5000 ThingsBoard servers running all over the world, ThingsBoard is running in production on AWS, Azure, GCE and private data centers.
It is possible to launch ThingsBoard in the private network with no internet access at all.
>>>>>>> master

## 独立vs集群模式

该平台设计为可水平扩展，并支持自动发现新的ThingsBoard服务器（节点）。
集群中的所有ThingsBoard节点都是相同的，并且正在分担负载。
由于所有节点都相同，因此没有“主”或“协调器”过程，也没有单点故障。
您选择的负载均衡器可能会将来自设备，应用程序和用户的请求转发到所有ThingsBoard节点。

## 整体与微服务架构

<<<<<<< HEAD
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
=======
Starting ThingsBoard v2.2, it is possible to run the platform as a monolithic application or as a set of microservices. 
Supporting both options requires some additional programming efforts, however, is critical due to back-ward compatibility with variety of existing installations.

Approximately 80% of the platform installations are still using monolithic mode due to minimum support efforts, knowledge and hardware resources to do the setup and low maintenance efforts.

However, if you do need high availability or would like to scale to millions of devices, then microservices is a way to go.
There are also some challenges that are solved with microservices architecture and applicable for more complex deployments and usage scenarios. 
For example, running a multi-tenant deployments where one need more granular isolation to protect from:

* unpredictable load spikes;
* unpredictable rule chain misconfiguration;
* single devices opening 1000s of concurrent connections due to firmware bugs;
* and many other cases.
>>>>>>> master
 
请点击下面列出的链接以了解更多信息，然后选择合适的架构和部署选项：

<<<<<<< HEAD
* [**monolithic**](/docs/reference/monolithic)：了解有关以单模式部署，配置和运行ThingsBoard平台的更多信息。
* [**microservices**](/docs/reference/msa)：了解有关在微服务模式下部署，配置和运行ThingsBoard平台的更多信息。
=======
* [**monolithic**](/docs/reference/monolithic): Learn more about deployment, configuring and running ThingsBoard platform in a monolythic mode.  
* [**microservices**](/docs/reference/msa): Learn more about deployment, configuring and running ThingsBoard platform in a microservices mode.
 
>>>>>>> master

## SQL vs NoSQL vs Hybrid数据库方法

<<<<<<< HEAD
ThingsBard使用数据库进行存储
[实体](/docs/user-guide/entities-and-relations/)设备，资产，客户，仪表板等）和[遥测](/docs/user-guide/telemetry/)数据（属性，时间序列传感器读数，统计信息，事件）。
平台目前支持三个数据库选项：

* **SQL**-将所有实体和遥测存储在SQL数据库中。 ThingsBoard作者建议使用PostgreSQL，这是ThingsBoard支持的主要SQL数据库。
可以将HSQLDB用于本地开发目的。 **除运行测试和启动具有最小可能负载的开发实例外，我们不建议将HSQLDB用于任何其他用途。
* **NoSQL**-将所有实体和遥测存储在NoSQL数据库中。 ThingsBoard作者建议使用Cassandra，这是ThingsBoard目前支持的唯一NoSQL数据库。
但是，由于对托管数据库的部署非常感兴趣，我们计划在v2.3中引入对AWS DynamoDB的支持。
* **Hybrid**-将所有实体存储在SQL数据库中，并将所有遥测存储在NoSQL数据库中。
=======
ThingsBoard uses database to store 
[entities](/docs/user-guide/entities-and-relations/) (devices, assets, customers, dashboards, etc) and 
[telemetry](/docs/user-guide/telemetry/) data (attributes, timeseries sensor readings, statistics, events). 
Platform supports three database options at the moment:

* **SQL** - Stores all entities and telemetry in SQL database. ThingsBoard authors recommend to use PostgreSQL and this is the main SQL database that ThingsBoard supports. 
It is possible to use HSQLDB for local development purposes. **We do not recommend to use HSQLDB** for anything except running tests and launching dev instance that has minimum possible load.
* **NoSQL (Deprecated)** - Stores all entities and telemetry in NoSQL database. ThingsBoard authors recommend to use Cassandra and this is the only NoSQL database that ThingsBoard supports at the moment.
Please note that this option is deprecated in favor of Hybrid approach due to many limitations of NoSQL for transactions and "joins" that are required to enable advanced search over IoT entities.
* **Hybrid (PostgreSQL + Cassandra)** - Stores all entities in PostgreSQL database and timeseries data in Cassandra database. 
* **Hybrid (PostgreSQL + TimescaleDB)** - Stores all entities in PostgreSQL database and timeseries data in Timescale database. 
>>>>>>> master

可以使用**thingsboard.yml**文件配置此选项。有关更多详细信息，请参见数据库[配置](/docs/user-guide/install/config/)页面。

```yaml
database:
  ts_max_intervals: "${DATABASE_TS_MAX_INTERVALS:700}" # Max number of DB queries generated by single API call to fetch telemetry records
  entities:
    type: "${DATABASE_ENTITIES_TYPE:sql}" # cassandra OR sql
  ts:
    type: "${DATABASE_TS_TYPE:sql}" # cassandra, sql, or timescale (for hybrid mode, DATABASE_TS_TYPE value should be cassandra, or timescale)

# note: timescale works only with postgreSQL database for DATABASE_ENTITIES_TYPE.
```

## 编程语言和第三方

<<<<<<< HEAD
ThingsBoard后端是用Java编写的，但是我们也有一些基于Node.js的微服务。 ThingsBoard前端是基于Angular JS框架的SPA。
有关使用的第三方组件的更多详细信息，请参见[monolithic](/docs/reference/monolithic)和[microservices](/docs/reference/monolithic)页面。
=======
ThingsBoard back-end is written in Java, but we also have some micro-services based on Node.js. ThingsBoard front-end is a SPA based on Angular 9 framework. 
See [monolithic](/docs/reference/monolithic) and [microservices](/docs/reference/monolithic) pages for more details about third-party components used.  
>>>>>>> master
