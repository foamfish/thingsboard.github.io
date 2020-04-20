---
layout: docwithnav
assignees:
- ikulikov
title: 在Windows上安装ThingsBoard
description: 在Windows上安装ThingsBoard

---

{% include templates/live-demo-banner.md %}

* TOC
{:toc}

### 先决条件

本指南介绍了如何在Windows计算机上安装ThingsBoard。

以下说明适用于Windows10/8.1/8/7 32位/64位。

硬件要求取决于选择的数据库和连接到系统的设备数量。

一台PC机运行ThingsBoard和PostgreSQL要求最低内存配置2Gb。

一台PC机运行ThingsBoard和Cassandra要求最低内存配置8Gb。

### 步骤1.安装Java 8(OpenJDK)

{% include templates/install/windows-java-install.md %}

### 步骤2.ThingsBoard服务安装

下载并解压缩软件包。

```bash
https://github.com/thingsboard/thingsboard/releases/download/v2.4.1/thingsboard-windows-2.4.1.zip
```
{: .copy-code}

**注意：** 我们假设您已将ThingsBoard软件包解压缩到默认位置:*C:\Program Files (x86)\thingsboard*

### 步骤3.配置ThingsBoard数据库

{% include templates/install/install-db.md %}

{% capture contenttogglespec %}
PostgreSQL <small>(建议 < 5K msg/sec)</small>%,%postgresql%,%templates/install/windows-db-postgresql.md%br%
Hybrid <br/>PostgreSQL+Cassandra<br/><small>(建议 > 5K msg/sec)</small>%,%hybrid%,%templates/install/windows-db-hybrid.md{% endcapture %}

{% include content-toggle.html content-toggle-id="ubuntuThingsboardDatabase" toggle-spec=contenttogglespec %} 

### 步骤4. [可选]低性能电脑内存修改(1GB RAM)

{% include templates/install/windows-memory-on-slow-machines.md %} 

### 步骤5.运行安装脚本

以管理员身份启动Windows Shell（命令提示符）。将目录更改为ThingsBoard安装目录。

执行**install.bat**脚本以将ThingsBoard作为Windows服务安装（或运行**“install.bat--loadDemo”**以安装和添加演示数据）。

这意味着它将在系统启动时自动启动。

执行**uninstall.bat**将从Windows服务中删除ThingsBoard。

输出执行结果：
  
  ```text
C:\Program Files (x86)\thingsboard>install.bat --loadDemo
Detecting Java version installed.
CurrentVersion 18
Java 1.8 found!
Installing thingsboard ...
...
ThingsBoard installed successfully!
```

### 步骤6.启动ThingsBoard服务

{% include templates/windows-start-service.md %}

{% capture 90-sec-ui %}
Please allow up to 90 seconds for the Web UI to start. This is applicable only for slow machines with 1-2 CPUs or 1-2 GB RAM.{% endcapture %}
{% include templates/info-banner.md content=90-sec-ui %}


### 故障排除

日志文件位于**logs**文件夹中（在本例中为"C:\Program Files (x86)\thingsboard\logs"）。

**thingsboard.log**文件应包含以下行：

```text
YYYY-MM-DD HH:mm:ss,sss [main] INFO  o.t.s.ThingsboardServerApplication - Started ThingsboardServerApplication in x.xxx seconds (JVM running for x.xxx)

```

如果有任何不清楚的错误，请使用常规的[故障排除指南](/docs/user-guide/troubleshooting/#getting-help)或[联系我们](/docs/contact-us/)。

### Windows防火墙设置

允许外部访问ThingsBoard Web UI和设备连接(HTTP, MQTT, CoAP)
您需要使用具有高级安全性的Windows防火墙创建新的入站规则。
 
- 从“控制面板”中打开“Windows防火墙”：

![image](/images/user-guide/install/windows/windows7-firewall-1.png)

- 点击左侧面板上的“高级设置”：

![image](/images/user-guide/install/windows/windows7-firewall-2.png)

- 在左侧面板上选择“入站规则”，然后在右侧“操作”面板上单击“新建规则...”：

![image](/images/user-guide/install/windows/windows7-firewall-3.png)

- 现在将打开新的“新建入站规则向导”窗口。在第一步“规则类型”中选择“端口”选项：

![image](/images/user-guide/install/windows/windows7-firewall-4.png)

- 在“协议和端口”步骤中选择“TCP”协议然后在“特定本地端口”字段中输入端口列表**8080、1883、5683**：

![image](/images/user-guide/install/windows/windows7-firewall-5.png)

- 在“操作”步骤中，选择“允许连接”选项：

![image](/images/user-guide/install/windows/windows7-firewall-6.png)

- 在“配置文件”步骤中，选择何时应用此规则的Windows网络配置文件：

![image](/images/user-guide/install/windows/windows7-firewall-7.png)

- 最后，为该规则命名（例如“ ThingsBoard服务网络”）然后单击“完成”。

![image](/images/user-guide/install/windows/windows7-firewall-8.png)



## 下一步

{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}
