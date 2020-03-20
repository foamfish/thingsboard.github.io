---
layout: docwithnav
title: Ubuntu服务系统下安装ThingsBoard网关

---

### 先决条件

本指南描述了如何在Ubuntu Server 18.04上安装ThingsBoard网关。

操作系统配置为官方[最低配置](https://help.ubuntu.com/lts/serverguide/preparing-to-install.html#system-requirements)。

### 步骤1.下载deb文件

下载安装包。

```bash
wget https://github.com/thingsboard/thingsboard-gateway/releases/latest/download/python3-thingsboard-gateway.deb
```
{: .copy-code}

### 步骤2.使用apt安装网关

以软件包形式安装ThingsBoard网关并以守护程序身份运行，请使用以下命令：<br> <br>

```bash
sudo apt install ./python3-thingsboard-gateway.deb -y
```
{: .copy-code}

deb软件包将自动安装必要的库，以使IOT网关正常工作：

1. 系统库：*libffi-dev，libglib2.0-dev，libxml2-dev，libxslt-dev，libssl-dev，zlib1g-dev，python3-dev，python3-pip*。

2. Python模块：*importlib，importlib-metadata，jsonschema，pymodbus，lxml，jsonpath-rw，paho-mqtt，pyserial，PyYAML，simplejson，pysistent*。

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


