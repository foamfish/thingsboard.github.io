---
layout: docwithnav
title: ThingsBoard部署方案
description: 部署方案和技巧概述

---

* TOC
{:toc}

本文介绍了ThingsBoard支持的最流行的部署体系结构。
所有部署方案都包含某些优点和缺点。
为您的部署选择正确的体系结构取决于TCO，性能和高可用性要求。
我们将从最简单的场景开始，看看如何将简约部署升级到最复杂的场景。

您可以在此找到使用AWS部署的ThingsBoard的总拥有成本（TCO）计算。
重要说明：以下所有计算和价格均为近似值，并作为示例列出。
请咨询您的云提供商，以获取准确的价格。

## 性能要求

我们准备了项目清单，以快速估算典型的物联网解决方案性能要求：

1. 生产或每年生产的已连接设备，资产，客户，客户用户和租户的总数；
2. 每台设备每天的最大和平均消息量；
3. 设备有效负载的最大和平均大小；
4. 每条消息的平均数据点数；
5. 用于设备连接的通信协议或集成类型；
6. 实体数据生命周期（以年为单位）。

一旦我们对上述参数有了大致了解，我们（以及您）将能够估算所需的基础设施。
ThingsBoard的性能在很大程度上取决于设备生成的消息量以及这些消息的结构。

**范例1：20,000个追踪器**
 
每分钟20,000个设备将消息发送到云一次。每个消息包含以下参数：

```json
{"latitude": 42.222222, "longitude": 73.333333, "speed": 55.5, "fuel": 92, "batteryLevel": 81}
```
在这种情况下，ThingsBoard会不断维护20,000个连接，并每秒处理333条消息。
每条消息传递5个数据点，可能需要分别对其进行图形化/分析/提取。
每秒导致对数据库的1,667个写请求，每天产生1.43亿个请求。
根据所选的数据库类型，每天大约需要1-2GB（Cassandra）或7-10GB（PostgreSQL）。

**示例2：100,000个智能电表**

每小时100,000个LoRaWAN设备将消息发送到云一次。每个消息结构如下：

```json
{"pulseCounter": 1234567, "leakage": false, "batteryLevel": 81}
```
ThingsBoard通过HTTP或MQTT从可用的网络服务器之一接收上行链路消息。
典型的消息速率是100,000 / 3600 =每秒28条消息，这非常低。
每个消息包含3个数据点，可能需要分别对其进行图形化/分析/获取。
但是，我们决定不存储“泄漏”属性，因为它是多余的（大多数情况下为“false”）。
我们将仅使用它来生成警报。
这导致每秒向数据库发送55.5个写入请求，每天产生478万个请求。
根据所选的数据库类型，每天大约需要100MB（Cassandra）或238MB（PostgreSQL）。

## 基础架构关键特征

根据[性能要求](/docs/reference/iot-platform-deployment-scenarios/#performance-requirements), 
您可以确定关键的ThingsBoard服务器/集群特征：

- 每秒 **传入消息数(incoming messages per second)** (主要影响RAM和CPU消耗);
- 并发的**活动设备会话(active device sessions)**数量（主要影响RAM消耗）；
- **规则引擎处理的消息(messages processed by Rule Engine)**数量（主要影响CPU消耗）；
- **持久数据点的(persisted data points)**数量（直接影响IOPS和相应的数据库）。

ThingsBoard集群可以水平扩展，因此您可以轻松应对RAM / CPU影响者。
但是，您需要仔细计划持久数据点的数量（上面列表中的最后一项）。
如果您打算使用PostgreSQL，我们建议每秒记录少于20,000个数据点。
如果您打算使用混合数据库方法（PostgreSQL和Cassandra），则可以将遥测（Cassandra）写操作扩展到每秒1M数据点，尽管属性更新已推送到PostgreSQL，所以20,000个限制仍然有效。

## 部署方案

### 独立服务器部署（方案A）

根据实际生产用例，最简单的部署方案适用于多达30万个设备，每秒具有10,000条消息和10,000个数据点。
这种情况需要在同一台服务器（本地或云中）中部署ThingsBoard平台和PostgreSQL数据库。
HAProxy负载平衡器也安装在同一服务器上，并充当反向代理和可选的TLS终止代理。
参见下图。

![image](/images/reference/deployment/single.png)

**优点**：

* 非常简单的设置，实际上是：使用[我们的安装指南](/docs/installation/)部署10分钟。
* 易于维护和更新软件实例。

**缺点**：

* 升级会导致停机，每次升级大约需要5-10分钟。
* 最小的高可用性。万一硬件或应用程序出现故障，所有设备和用户都会受到影响。
* 无数据持久性。一切都存储在一台服务器上。
* 系统性能受单个服务器性能的限制。

**性能**：

解决方案的整体性能取决于实例硬件，而在很大程度上取决于数据库的性能。
我们建议在独立服务器部署方案中将PostgreSQL用于实体和遥测数据。
一个普通的虚拟环境每秒可以处理约5,000个遥测数据点。
请参阅[关键基础结构特征](/docs/reference/iot-platform-deployment-scenarios/#key-infrastructure-characteristics)
和[性能测试](/docs/reference/performance-aws-instances/)在不同的AWS实例上。此信息对于正确决定您的解决方案的基础结构很有用。

**拥有总成本（TCO）示例**：

假设每小时有10,000个LoRaWAN智能电表设备将消息发送到云一次。

单个AWS EC2"m5.large"实例的费用为每月约41.66 USD（如果预付1年，则每年约为500 USD）。
500GB存储价格为每月50美元。
大约每月的基础设施成本约为100美元。

ThingsBoard PE永久许可证（低于v3.0）的价格为2,999美元（包括使用首年内的可选更新和基本支持）。后续年份的软件更新+基本支持的价格为1,199美元。

TCO：每月约350美元。这个价格与每个设备每月0.035 USD相关，而设备数量为1万。
添加[Premium support](/docs/services/support/)软件包将导致每月〜850 USD或每台设备每月0.085 USD。

**评论和建议**：

这种部署方案非常简单，非常适合开发环境，原型设计和早期创业公司。
在投入生产之前，建议您设置数据备份脚本并将数据库快照定期上载到持久性存储（AWS S3等）中。实施服务器实例的常规快照也很有用，这样可以最大程度地减少发生故障时的恢复时间。
  
如果您想最小化用于数据库维护的资源，我们建议使用云托管数据库。有关更多详细信息，请参见方案B。

### 具有外部数据库的单服务器部署（方案B）

此部署方案与方案A相似，但需要将完全托管的数据库部署在单独的服务器上。
ThingsBoard customers successfully utilize [AWS RDS](https://aws.amazon.com/rds/postgresql/), [Azure Database for PostgreSQL](https://azure.microsoft.com/en-us/services/postgresql/) and
ThingsBoard客户成功利用[AWS RDS](https://aws.amazon.com/rds/postgresql/)，[Azure PostgreSQL数据库](https://azure.microsoft.com/en-us/services/postgresql/)和
[Google Cloud SQL](https://cloud.google.com/sql/docs/postgres/) 可以最大程度地减少数据库设置，备份和支持方面的工作。
参见下图。

![image](/images/reference/deployment/standalone.png)

**优点**：

* 安装非常简单（使用我们的安装指南大约需要1个小时来部署）。
* 易于维护和更新软件实例。
* 数据与托管备份和故障转移分开存储。

**缺点**：

* 升级会导致停机。每次升级的停机时间约为5分钟。
* 最小的高可用性。如果发生硬件或应用程序故障，则管理员必须执行手动操作以启动和运行系统。
* 系统性能受单个服务器性能的限制。

**性能**：

解决方案的整体性能取决于实例硬件，而在很大程度上取决于数据库的性能。
我们建议在这种情况下将PostgreSQL用于实体和遥测数据。
一个普通的虚拟环境每秒可以处理约5,000个遥测数据点。
请参阅[关键基础结构特征](/docs/reference/iot-platform-deployment-scenarios/#key-infrastructure-characteristics)
和[性能测试](/docs/reference/performance-aws-instances/) 在不同的AWS实例上。

**方案B的总拥有成本示例**：

假设每小时有10,000个LoRaWAN智能电表设备将消息发送到云一次。

每月单个AWS EC2"m5.large"实例的成本为〜41.66 USD（在每年预付款的情况下，每年约为500 USD）。
在db.t2.medium和多可用区部署的情况下，Amazon RDS PostgreSQL实例的每月费用约为200美元。
大约基础设施成本：约250美元/月。

单个ThingsBoard PE永久许可的价格为2,999美元（包括使用首年内的可选更新和基本支持）。后续年份的软件更新+基本支持的价格为1,199美元。

TCO：每月约500美元或每台设备每月0.05美元（最多可容纳1万个设备用例）。
添加[Premium support](/docs/services/support/) 软件包将导致每月〜1000 USD或每个设备每月0.1 USD。 

### 使用微服务架构进行集群部署（场景C）

ThingsBoard支持微服务架构（MSA），可以为数百万个设备执行可扩展的部署。请参阅[平台架构](/docs/reference/msa/)了解更多详细信息。通过MSA部署，系统管理员可以灵活地调整传输，规则引擎，Web UI和JavaScript执行程序微服务的数量，以根据当前负载优化集群。
ThingsBoard使用[Kafka](https://kafka.apache.org/)作为主要消息队列和流解决方案，使用[Redis](https://redis.io/)作为分布式缓存，并使用[Cassandra](http://cassandra.apache.org/)作为高度可用，可扩展且快速的NoSQL数据库。
请注意，Cassandra的使用是可选的，在高遥测数据速率（每秒超过20,000个数据点）的情况下建议使用。在其他情况下，基于PostgreSQL的部署就足够了。

**优点**：

* 简单的Kubernetes部署。
* 无SPOF。
* 高可用性和系统。
* 在次要版本升级期间无停机时间。

**缺点**：

* 少量设备上的总拥有成本高（每个ThingsBoard集群少于100000个设备）。

**性能**：

该解决方案的整体性能取决于群集硬件，并且在很大程度上取决于所用数据库的性能。
具有5个ThingsBoard服务器和5个Cassandra节点的虚拟机集群可以处理一百万个设备；
有关更多详细信息，请参见[关键基础结构特征](/docs/reference/iot-platform-deployment-scenarios/#key-infrastructure-characteristics)。
  
**集群部署方案的总拥有成本示例**：

#### 100万智能电表的总拥有成本

**示例1：**假设**1,000,000** LoRaWAN / NB-IoT **智能仪表**设备每小时向云发送消息的设备**一次**。
每个消息包含3个数据点，可能需要分别对其进行图形化/分析/提取。
我们认为消息是通过HTTP或UDP集成发送到ThingsBoard的，这在这种情况下是很典型的。

1,000,000个设备表示每秒负载280条消息（1,000,000个设备/ 3600秒），这导致每秒280 x 3 = 840个写入请求到数据库（数据点）的请求，或每天7260万个请求。
根据所选的数据库类型，以上情况每天导致大约消耗1.2GB（Cassandra）或4GB（PostgreSQL）的磁盘空间。

以下Kubernetes集群足以支持此用例：

- 2个“r5.xlarge”实例（4vCPU和32 GB RAM）来托管2个ThingsBoard Node容器。大约价格约为380美元/月。
- 3个“c5.large”实例（2vCPU和4 GB的RAM）来托管3个Zookeeper和大约9个JS执行器。大约价格是〜120美元/月。
- 基于2 x“cache.m5.large”的Amazon ElastiCache for Redis。大约价格约为200美元/月。
- 基于3个“kafka.m5.large”和1TB数据存储的Amazon Managed Streaming for Kafka。估计：620 USD /月。
- 基于“db.m5.large”多可用区部署的Amazon RDS for PostgreSQL。估计：220 USD /月。
- 1TB多可用区部署存储。价格为每月230美元。

![image](/images/reference/deployment/smart-meter-cluster.png)

因此，大约每台设备的基础设施成本约为1770美元/月或0.00177美元/月。

两个ThingsBoard PE永久许可证的价格为5,998美元（包括使用首年内的可选更新和基本支持）。随后几年的软件更新+基本支持的价格为2,398美元。
在超过1万个设备的用例中，我们提供了**托管服务**来支持生产环境（不是基本的Support订阅）。费率为每台设备每月0.01美元。
 
TCO：每月约12,270美元或每台设备每月0.01227美元。

**如果您想在集群设置中重现这种情况，请遵循以下指南：**
[智能电表用例性能测试](https://github.com/ashvayka/tb-pe-k8s-perf-tests/tree/scenario/1-million-smart-meters)

#### 100万个智能跟踪器TCO

**范例2：**假设有1,000,000台**智能追踪器**设备每分钟发送一次**到云中**。
每个消息包含5个数据点，可能需要分别对其进行图形化/分析/提取。

典型的消息速率是1,000,000 / 60秒。=每秒16,667条消息。
这导致每秒对数据库（数据点）的16667 x 5 = 83,335写请求，或每天7.2B请求。
使用Cassandra可以可靠地处理此负载，每天可达到144GB。由于需要在Cassandra中将数据复制3次，因此每天需要432 GB的磁盘空间。

以下Kubernetes集群足以支持此用例：

- 8个“c5.large”实例（2vCPU和4 GB RAM）来托管8个ThingsBoard MQTT Transport容器。大约价格是〜320美元/月。
- 15个“c5.xlarge”实例（4vCPU和8 GB RAM）来托管15个ThingsBoard Node容器。大约价格是〜1095美元/月。
- 3个“c5.xlarge”实例（2vCPU和4 GB RAM）来托管3个Zookeeper和约30个JS执行器。大约价格是〜240美元/月。
- 基于2 x“ cache.m5.large”的Amazon ElastiCache for Redis。大约价格约为200美元/月。
- 基于3个“ kafka.m5.large”和1TB数据存储的Amazon Managed Streaming for Kafka。估计：620 USD /月。
- 基于“ db.m5.large”多可用区部署的Amazon RDS for PostgreSQL。估计：220 USD /月。
- 100TB的部署存储。价格：10,000美元/月。

![image](/images/reference/deployment/smart-tracker-cluster.png)

因此，每台设备的基础设施成本约为13790美元/月或0.0138美元/月。
15个ThingsBoard PE永久许可证（低于v3.0）的价格为44,985美元（包括使用首年内的可选更新和基本支持）。后续几年的软件更新+基本支持的价格为17,985美元。
ThingsBoard **支持生产环境的托管服务**：每台设备每月0.01美元。

TCO：每月约27,508美元或每台设备每月0.0275美元。

**如果您想在集群设置中重现这种情况，请遵循以下指南：**
[智能跟踪器用例性能测试](https://github.com/ashvayka/tb-pe-k8s-perf-tests/tree/scenario/1-million-smart-trackers)