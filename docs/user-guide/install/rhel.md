---
layout: docwithnav
assignees:
- ashvayka
title: 在CentOS/RHEL上安装ThingsBoard CE
description: 在CentOS/RHEL上安装ThingsBoard CE

---

* TOC
{:toc}

### 先决条件

<<<<<<< HEAD
本指南描述了如何在RHEL/CentOS 7上安装ThingsBoard。
=======
This guide describes how to install ThingsBoard on RHEL/CentOS 7/8. 
Hardware requirements depend on chosen database and amount of devices connected to the system. 
To run ThingsBoard and PostgreSQL on a single machine you will need at least 1Gb of RAM.
To run ThingsBoard and Cassandra on a single machine you will need at least 8Gb of RAM.
>>>>>>> master

硬件要求取决于选择的数据库和连接到系统的设备数量。

一台PC机运行ThingsBoard和PostgreSQL要求最低内存配置2Gb。

一台PC机运行ThingsBoard和Cassandra要求最低内存配置8Gb。

在继续安装之前请执行以下命令以安装必要的工具：

**For CentOS 7:**

```bash
# Install wget
sudo yum install -y nano wget
# Add latest EPEL release for CentOS 7
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

```
{: .copy-code}

**For CentOS 8:**

```bash
# Install wget
sudo yum install -y nano wget
# Add latest EPEL release for CentOS 8
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

```
{: .copy-code}

### 步骤1.安装Java 8(OpenJDK)

{% include templates/install/rhel-java-install.md %} 

### 步骤2.ThingsBoard服务安装

下载安装包

```bash
<<<<<<< HEAD
wget https://github.com/thingsboard/thingsboard/releases/download/v2.4.1/thingsboard-2.4.1.rpm
=======
wget https://github.com/thingsboard/thingsboard/releases/download/v3.0.1/thingsboard-3.0.1.rpm
>>>>>>> master
```
{: .copy-code}

ThingsBoard安装服务

```bash
<<<<<<< HEAD
sudo rpm -Uvh thingsboard-2.4.1.rpm
=======
sudo rpm -Uvh thingsboard-3.0.1.rpm
>>>>>>> master
```
{: .copy-code}


### 步骤3.配置ThingsBoard数据库

{% include templates/install/install-db.md %}

{% capture contenttogglespec %}
<<<<<<< HEAD
PostgreSQL <small>(建议 < 5K msg/sec)</small>%,%postgresql%,%templates/install/rhel-db-postgresql.md%br%
Hybrid <br/>PostgreSQL+Cassandra<br/><small>(建议 > 5K msg/sec)</small>%,%hybrid%,%templates/install/rhel-db-hybrid.md{% endcapture %}

{% include content-toggle.html content-toggle-id="rhelThingsboardDatabase" toggle-spec=contenttogglespec %} 

### Step 4. [可选]低性能电脑内存修改(1GB RAM) 

{% include templates/install/memory-on-slow-machines.md %} 

### Step 5. 运行安装脚本
{% include templates/run-install.md %} 


### Step 6. 启动ThingsBoard服务

默认情况下，ThingsBoard UI可在8080端口上访问。

确保您的8080端口可通过防火墙访问。
=======
PostgreSQL <small>(recommended for < 5K msg/sec)</small>%,%postgresql%,%templates/install/rhel-db-postgresql.md%br%
Hybrid <br/>PostgreSQL+Cassandra<br/><small>(recommended for > 5K msg/sec)</small>%,%hybrid%,%templates/install/rhel-db-hybrid.md%br%
Hybrid <br/>PostgreSQL+TimescaleDB<br/><small>(for TimescaleDB professionals)</small>%,%timescale%,%templates/install/rhel-db-hybrid-timescale.md{% endcapture %}

{% include content-toggle.html content-toggle-id="rhelThingsboardDatabase" toggle-spec=contenttogglespec %} 

### Step 4. Choose ThingsBoard queue service

{% include templates/install/install-queue.md %}

{% capture contenttogglespecqueue %}
In Memory <small>(built-in and default)</small>%,%inmemory%,%templates/install/queue-in-memory.md%br%
Kafka <small>(recommended for on-prem, production installations)</small>%,%kafka%,%templates/install/rhel-queue-kafka.md%br%
AWS SQS <small>(managed service from AWS)</small> %,%aws-sqs%,%templates/install/ubuntu-queue-aws-sqs.md%br%
Google Pub/Sub <small>(managed service from Google)</small> %,%pubsub%,%templates/install/ubuntu-queue-pub-sub.md%br%
Azure Service Bus <small>(managed service from Azure)</small>%,%service-bus%,%templates/install/ubuntu-queue-service-bus.md%br%
RabbitMQ <small>(for small on-prem installations)</small>%,%rabbitmq%,%templates/install/rhel-queue-rabbitmq.md{% endcapture %}

{% include content-toggle.html content-toggle-id="ubuntuThingsboardQueue" toggle-spec=contenttogglespecqueue %} 

### Step 5. [Optional] Memory update for slow machines (1GB of RAM) 

{% include templates/install/memory-on-slow-machines.md %} 

### Step 6. Run installation script
{% include templates/run-install.md %} 


### Step 7. Start ThingsBoard service
>>>>>>> master

为了打开8080端口，请执行以下命令：

```bash
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```
{: .copy-code}   

{% include templates/start-service.md %}

{% capture 90-sec-ui %}
请等待90秒以启动Web UI。这仅适用于具有1-2CPU或1-2GB RAM的计算机。{% endcapture %}
{% include templates/info-banner.md content=90-sec-ui %}

### 安装后步骤

{% include templates/install/rhel-haproxy-postinstall.md %}

### 故障排除

{% include templates/install/troubleshooting.md %}

## 下一步


{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}
