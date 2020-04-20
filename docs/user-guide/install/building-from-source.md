---
layout: docwithnav
assignees:
- ashvayka
title: 从源代码安装
description: 从源代码安装ThingsBoard平台

---

* TOC
{:toc}

本指南将帮助您从源代码下载和安时ThingsBoard。下面列出的说明已在Ubuntu 16.04和CentOS 7.1上进行了测试

#### 必备工具

本节包含生成工具的安装说明。

##### Java

ThingsBoard是使用Java8生成的。您可以使用[安装说明](/docs/user-guide/install/linux#java)。

##### Maven

ThingsBoard生成需要Maven 3.1.0+。

{% capture tabspec %}maven-installation
A,Ubuntu,shell,resources/maven-ubuntu-installation.sh,/docs/user-guide/install/resources/maven-ubuntu-installation.sh
B,CentOS,shell,resources/maven-centos-installation.sh,/docs/user-guide/install/resources/maven-centos-installation.sh{% endcapture %}
{% include tabs.html %}

**注意**，在某些Linux计算机上，maven安装可能会将Java 7设置为默认JVM。

使用java安装[说明](#java)修复此问题。

#### 源代码

您可以从[github](https://github.com/thingsboard/thingsboard)克隆项目的源代码。

```bash
git clone git@github.com:thingsboard/thingsboard.git
# checkout latest release branch
git checkout release-2.4
```

#### 生成

从thingboard文件夹执行如下命令并生成项目：

```bash
mvn clean install
```

#### 生成本地docker映像

```bash
mvn clean install -Ddockerfile.skip=false
```

#### 生成文件

您可以在目标文件夹中找到debian，rpm和Windows软件包：
 
```bash
application/target
```
 