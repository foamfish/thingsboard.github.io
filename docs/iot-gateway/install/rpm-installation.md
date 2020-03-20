---
layout: docwithnav
title: Centos系统下安装ThingsBoard网关

---


### 先决条件

本指南描述了如何在CentOS或RHEL上安装ThingsBoard网关。

### Step 1. Download the installation package

下载安装包。

```bash
wget https://github.com/thingsboard/thingsboard-gateway/releases/download/2.0/python3-thingsboard-gateway.rpm
```
{: .copy-code}

### 步骤2.使用yum安装网关

以软件包形式安装ThingsBoard网关并以守护程序身份运行，请使用以下命令：<br> <br>

```bash
sudo yum install -y ./python3-thingsboard-gateway.rpm
```
{: .copy-code}  

### 步骤3.检查网关状态

```bash
systemctl status thingsboard-gateway
```
{: .copy-code}

出现如下信息则表示没有配置网关和ThingsBoard连接：

```text
... python3[7563]: ''2019-12-26 09:31:15' - ERROR - mqtt_connector - 181 - Default Broker connection FAIL with error 5 not authorised!'
... python3[7563]: ''2019-12-26 09:31:15' - DEBUG - mqtt_connector - 186 - "Default Broker" was disconnected.'
... python3[7563]: ''2019-12-26 09:31:16' - DEBUG - tb_client - 78 - connecting to ThingsBoard'
... python3[7563]: ''2019-12-26 09:31:17' - DEBUG - tb_client - 78 - connecting to ThingsBoard'
```

### 步骤4.配置网关

现在您可以转到[**配置指南**](/docs/iot-gateway/configuration/)来配置网关。
