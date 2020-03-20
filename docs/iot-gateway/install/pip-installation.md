---
layout: docwithnav
title: Pip方式安装网关

---

### 软件包管理器安装

将ThingsBoard网关安装为python模块，您应该遵循以下步骤：

**1. 使用apt将所需的库安装到系统中**

```bash
sudo apt install python3-dev python3-pip libglib2.0-dev 
```
{: .copy-code}

**2. 使用pip安装ThingsBoard网关模块：**

```bash
sudo pip3 install thingsboard-gateway
```
{: .copy-code}

**3. 下载配置示例，创建日志文件夹：**

 - 下载配置示例：

```bash
wget https://github.com/thingsboard/thingsboard-gateway/releases/download/2.0/configs.tar.gz
```
{: .copy-code}

 - 创建配置目录：
```bash
sudo mkdir /etc/thingsboard-gateway
```
{: .copy-code}

 - 创建日志目录：
```bash
sudo mkdir /var/log/thingsboard-gateway
```
{: .copy-code}

 - 解压配置：
```bash
sudo tar -xvzf configs.tar.gz -C /etc/thingsboard-gateway
```
{: .copy-code}


**4. 使用命令检查安装**（由于没有配置网关因此会出现连接错误*有关配置请使用[配置指南](/docs/iot-gateway/configuration-guide))：*

```bash
thingsboard-gateway
```
{: .copy-code}