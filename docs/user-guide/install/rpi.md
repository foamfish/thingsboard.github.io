---
layout: docwithnav
assignees:
- ashvayka
title: 在树莓派3代B上安装ThingsBoard
description: 在树莓派3代B上安装ThingsBoard

---

{% include templates/live-demo-banner.md %}

* TOC
{:toc}

本指南介绍了如何在运行树莓派3代B上安装ThingsBoard。

### 第三方组件安装

### 步骤1.安装Java 8(OpenJDK) 

{% include templates/install/ubuntu-java-install.md %}

### 步骤2.ThingsBoard服务安装

下载安装包

```bash
wget https://github.com/thingsboard/thingsboard/releases/download/v2.4.1/thingsboard-2.4.1.deb
```
{: .copy-code}

安装ThingsBoard服务

```bash
sudo dpkg -i thingsboard-2.4.1.deb
```
{: .copy-code}

### 步骤3.配置ThingsBoard数据库

{% include templates/install/ubuntu-db-postgresql.md %}

### 步骤4.低性能电脑内存修改(1GB RAM)

{% include templates/install/memory-on-slow-machines.md %} 

### 步骤5.运行安装脚本
{% include templates/run-install.md %} 


### 步骤6.启动ThingsBoard服务

{% include templates/start-service.md %}

{% capture 90-sec-ui %}
Please allow up to 240 seconds for the Web UI to start. This is applicable only for slow machines with 1-2 CPUs or 1-2 GB RAM.{% endcapture %}
{% include templates/info-banner.md content=90-sec-ui %}

### 故障排除

{% include templates/install/troubleshooting.md %}

## 下一步

{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}
