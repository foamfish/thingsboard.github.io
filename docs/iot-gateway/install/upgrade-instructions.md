---
layout: docwithnav
title: 网关升统说明

---


### 升级说明

有两种升级ThingsBoard网关的方法，具体取决于所需的版本（(**Release** 或 **Develop**)。

* 升级到**Release**版本，您应该使用以下命令：

```
sudo pip3 install thingsboard-gateway --upgrade
```
{: .copy-code}

* 升级到**Develop**版本，您应该使用[本指南](/docs/iot-gateway/install/source-installation/)。

升级ThingsBoard网关docker安装，请使用[Docker安装指南](/docs/iot-gateway/install/docker-linux/#upgrading)中的**升级**步骤。


**注意：**如果您在升级方面遇到问题，请尝试从系统中（sudo，user，local）的pip中删除软件包。

通过此命令可以进行移除：
```bash
sudo pip3 uninstall thingsboard-gateway -y
```
{: .copy-code}

移除后您需要使用上面的命令再次安装网关。
