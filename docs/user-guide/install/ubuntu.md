---
layout: docwithnav
assignees:
- ashvayka
title: 在Ubuntu Server上安装ThingsBoard CE
description: 在Ubuntu Server上安装ThingsBoard CE

---

* TOC
{:toc}

### 先决条件

本指南介绍了如何在Ubuntu Server 18.04 LTS上安装ThingsBoard。

硬件要求取决于选择的数据库和连接到系统的设备数量。

一台PC机运行ThingsBoard和PostgreSQL要求最低内存配置2Gb。

一台PC机运行ThingsBoard和Cassandra要求最低内存配置8Gb。

### 步骤1.安装Java 8(OpenJDK)

{% include templates/install/ubuntu-java-install.md %}

### 步骤2.ThingsBoard服务安装

下载安装包。

```bash
<<<<<<< HEAD
wget https://github.com/thingsboard/thingsboard/releases/download/v2.4.1/thingsboard-2.4.1.deb
=======
wget https://github.com/thingsboard/thingsboard/releases/download/v3.0.1/thingsboard-3.0.1.deb
>>>>>>> master
```
{: .copy-code}

安装ThingsBoard服务

```bash
<<<<<<< HEAD
sudo dpkg -i thingsboard-2.4.1.deb
=======
sudo dpkg -i thingsboard-3.0.1.deb
>>>>>>> master
```
{: .copy-code}

### 步骤3.配置ThingsBoard数据库

{% include templates/install/install-db.md %}

{% capture contenttogglespec %}
<<<<<<< HEAD
PostgreSQL <small>(建议 < 5K msg/sec)</small>%,%postgresql%,%templates/install/ubuntu-db-postgresql.md%br%
Hybrid <br/>PostgreSQL+Cassandra<br/><small>(建议 > 5K msg/sec)</small>%,%hybrid%,%templates/install/ubuntu-db-hybrid.md{% endcapture %}

{% include content-toggle.html content-toggle-id="ubuntuThingsboardDatabase" toggle-spec=contenttogglespec %} 

### 步骤4. [可选]低性能电脑内存修改(1GB RAM)

{% include templates/install/memory-on-slow-machines.md %} 

### 步骤5.运行安装脚本

{% include templates/run-install.md %} 


### 步骤6.启动ThingsBoard服务
=======
PostgreSQL <small>(recommended for < 5K msg/sec)</small>%,%postgresql%,%templates/install/ubuntu-db-postgresql.md%br%
Hybrid <br/>PostgreSQL+Cassandra<br/><small>(recommended for > 5K msg/sec)</small>%,%hybrid%,%templates/install/ubuntu-db-hybrid.md%br%
Hybrid <br/>PostgreSQL+TimescaleDB<br/><small>(for TimescaleDB professionals)</small>%,%timescale%,%templates/install/ubuntu-db-hybrid-timescale.md{% endcapture %}

{% include content-toggle.html content-toggle-id="ubuntuThingsboardDatabase" toggle-spec=contenttogglespec %} 

### Step 4. Choose ThingsBoard queue service

{% include templates/install/install-queue.md %}

{% capture contenttogglespecqueue %}
In Memory <small>(built-in and default)</small>%,%inmemory%,%templates/install/queue-in-memory.md%br%
Kafka <small>(recommended for on-prem, production installations)</small> %,%kafka%,%templates/install/ubuntu-queue-kafka.md%br%
Kafka in docker container <small>(recommended for on-prem, production installations)</small> %,%kafka-in-docker%,%templates/install/ubuntu-queue-kafka-in-docker.md%br%
AWS SQS <small>(managed service from AWS)</small> %,%aws-sqs%,%templates/install/ubuntu-queue-aws-sqs.md%br%
Google Pub/Sub <small>(managed service from Google)</small>%,%pubsub%,%templates/install/ubuntu-queue-pub-sub.md%br%
Azure Service Bus <small>(managed service from Azure)</small>%,%service-bus%,%templates/install/ubuntu-queue-service-bus.md%br%
RabbitMQ <small>(for small on-prem installations)</small>%,%rabbitmq%,%templates/install/ubuntu-queue-rabbitmq.md{% endcapture %}

{% include content-toggle.html content-toggle-id="ubuntuThingsboardQueue" toggle-spec=contenttogglespecqueue %} 

### Step 5. [Optional] Memory update for slow machines (1GB of RAM) 

{% include templates/install/memory-on-slow-machines.md %} 

### Step 6. Run installation script
{% include templates/run-install.md %} 


### Step 7. Start ThingsBoard service
>>>>>>> master

{% include templates/start-service.md %}

{% capture 90-sec-ui %}
请等待90秒以启动Web UI。这仅适用于具有1-2CPU或1-2GB RAM的计算机。
{% endcapture %}
{% include templates/info-banner.md content=90-sec-ui %}

### 安装后步骤

{% include templates/install/ubuntu-haproxy-postinstall.md %}

### 故障排除

{% include templates/install/troubleshooting.md %}

## 下一步

{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}
