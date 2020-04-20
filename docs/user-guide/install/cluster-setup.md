---
layout: docwithnav
assignees:
- ashvayka
title: 集群安装
description: ThingsBoard集群设置指南

---

* TOC
{:toc}

本指南将帮助您以集群模式设置ThingsBoard。有两个选项。

## 使用微服务架构进行集群设置（推荐）

从ThingsBoard v2.2开始，可以使用新的微服务架构和Docker容器安装ThingsBoard集群。
deployment
部署
请参阅[**微服务**](/docs/reference/msa/)体系结构页面和[**部署**](https://github.com/thingsboard/thingsboard/blob/master/docker/README.md)有关更多详细信息的技巧，如何在“ dockerized”环境中启动ThingsBoard集群。仅建议高级用户使用此选项。

## 使用单体架构的集群设置（v2.2+）
  
不建议在单个VM中安装每个都包含所有必需的传输和核心组件的整体ThingsBoard应用程序集群。

但是，如果您想最大程度地减少使用的第三方数量，您可能仍要使用此选项。请参阅下面的说明。

### 假设

ThingsBoard需要Zookeeper进行群集协调，Cassandra需要NoSQL数据库，Redis需要群集缓存。

您可以将Cassandra和Redis托管在安装ThingsBoard的相同节点上，也可以托管在单独的节点上。

我们假设以下拓扑
 
![image](/images/user-guide/cluster-topology.svg)
 
在这种情况下，Zookeeper和Cassandra节点均以集群模式部署。

我们仅将两个节点群集用于演示目的。

不建议将其用于生产。

让我们假设以下主机名：

 - **tb1**, **tb2**和**tb3** - ThingsBoard主机
 - **zk1**和**zk2** - Zookeeper主机
 - **c1**和**c2** - Cassandra主机 
 - **r1**和**r2** (redis集群)
 
我们将为Cassandra（9042），Zookeeper（2181）和Redis（6379）使用默认端口。

### 安装

您可以使用单节点安装ThingsBoard服务[安装指南](/docs/user-guide/install/linux/)请注意，您不必为每个集群执行一次“规定数据库模式和初始数据”步骤。

### 配置

您需要在[thingsboard.yml](/docs/user-guide/install/config/#thingsboardyml)中更改以下Zookeeper和Cassandra参数

```bash
zk:
  enabled: "${ZOOKEEPER_ENABLED:true}"
  url: "${ZOOKEEPER_URL:zk1:2181,zk2:2181}"

cassandra:
  url: "${CASSANDRA_URL:c1:9042,c2:9042}"
  # Cassandra cluster connection query parameters  #
  query:
    read_consistency_level: "${CASSANDRA_READ_CONSISTENCY_LEVEL:QUORUM}"
    write_consistency_level: "${CASSANDRA_WRITE_CONSISTENCY_LEVEL:QUORUM}"
    
redis: 
  # standalone or cluster
  connection:
    type: standalone
    host: "${REDIS_HOST:localhost}"
    port: "${REDIS_PORT:6379}"
    db: "${REDIS_DB:0}"
    password: "${REDIS_PASSWORD:}"

```

另外，您需要指定**rpc.bind_host**，以与每个Thingsboard服务器的当前主机匹配。例如，**tb1**配置：

```bash
rpc:
  bind_host: "${RPC_HOST:tb1}"
```

### 网络

需要在群集内为相应服务器访问以下端口：
 
 - Zookeeper - **2181** 端口(可以使用**zk.url**属性进行修改)。
 - Cassandra - **9042** 端口(可以使用**cassandra.url**属性进行修改)。
 - ThingsBoard - **9001** 端口(可以使用**rpc.bind_port**属性进行修改)。
 - Redis     - **6379** 端口(可以使用**redis.port**属性进行修改)。

需要在集群外部访问以下ThingsBoard服务器端口以实现设备连接：
 - HTTP - **8080** 端口 (可以使用**server.port**属性进行修改)。
 - MQTT - **1883** 端口 (可以使用**mqtt.bind_port**属性进行修改)。
 - CoAP - **5683** 端口 (可以使用**coap.bind_port**属性进行修改)。

## 下一步

{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}
