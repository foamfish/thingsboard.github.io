---
layout: docwithnav
title: ThingsBoard网关入门
description: 使用ThingsBoard网关应用于你的的第一个项目

---

* TOC
{:toc}

本指南涵盖了初始化网关的安装和配置。

我们将网关连接到ThingsBoard服务器，并可视化一些基本网关统计信息：连接的设备数量和已处理的消息。

我们还将配置MQTT和OPC-UA扩展以便订阅来自外部设备或应用程序的设备数据反馈。


### 先决条件

如果您没有权限访问正在运行的ThingsBoard实例，请使用[**在线演示**](https://demo.thingsboard.io/signup)或[**安装指南**](/docs/user-guide/install/installation-options/)解决这个问题。


## 步骤1：配置网关

为了将您的网关连接到ThingsBoard服务器，您需要首先配置网关凭据。我们将使用访问令牌凭证作为最简单的凭证。

有关更多详细信息，请参见[设备身份验证](/docs/user-guide/device-credentials/) 。

以租户管理员身份登录。如果是本地ThingsBoard服务器，请使用[默认凭据](/docs/samples/demo-account/#demo-tenant)。

打开**设备**，然后单击右下角的红色大“+”按钮。

{:refdef: style="text-align: center;"}
![image](/images/gateway/device-page.png)
{: refdef} 

填写您的网关名称，然后选择“Is gateway”复选框。

{:refdef: style="text-align: center;"}
![image](/images/gateway/device-add.png)
{: refdef} 

**NOTE:** 网关和设备名称在租户范围内必须是唯一的。

打开新的设备然后单击“Copy Access Token”按钮。

将令牌粘贴到安全的地方。在接下来的步骤中，我们将其用于ThingsBoard配置。

{:refdef: style="text-align: center;"}
![image](/images/gateway/device-token.png)
{: refdef} 

## 步骤2：安装网关

浏览可用的网关[**安装**](/docs/iot-gateway/installation/)并选择最合适的安装指南。

请遵循所选网关安装指南中的步骤。网关配置步骤在下面介绍。

## 步骤3：网关配置

打开网关配置文件夹，然后编辑**tb-gateway.yaml**文件。
```bash
/etc/thingsboard-gateway/config/tb_gateway.yaml
```
  
将*“thingsboard”*部分中的**host**和**port**属性更改为ThingsBoard主机。

将*"security"*部分中的**accessToken**属性更改为在步骤3中复制的令牌。

您的网关配置应类似于以下文件：

```yaml

thingsboard:
  host: demo.thingsboard.io
  port: 1883
  security:
    accessToken: FUH2Fonov6eajSHi0Zyw
storage:
  type: memory
  read_records_count: 10
  max_records_count: 1000
connectors:

  -
    name: MQTT Broker Connector
    type: mqtt
    configuration: mqtt.json

```

**您可以在[本文](/docs/iot-gateway/configuration/)中阅读有关配置文件及其属性的更多信息。**

##步 骤4：重新启动网关以接受新配置

此步骤取决于所选的安装类型。如果将Thingsboard网关安装为守护程序-应该使用以下命令：
```bash
systemctl restart thingsboard-gateway.service
```
在其他情况下，如果您已将网关安装为python模块-您应该只重新运行网关进程。

## 步骤5：查看网关统计信息

打开ThingsBoard服务器的Web UI，并查看从Thingsboard网关上传的统计信息。

以租户管理员身份登录并打开**Devices**页面。单击网关设备卡。

打开“Latest Telemetry”选项卡并查看以下统计信息：“**SummaryReceived**”，“**SummarySent**”以及提供有关每个连接器信息的参数。

所有值应设置为“ 0”。

## 步骤6：将连接器添加到主配置文件
 
为了连接到某些设备，我们使用连接器，它们连接到不同的设备和服务器以收集数据。

要提供有关所需连接器的网关信息-您应该在tb_gateway.yaml中的“连接器”部分中写入配置（至少需要一个连接器才能正常工作）。

为了进行正确的配置，请使用[本文](/docs/iot-gateway/configuration/#section-connectors)。
 
<<<<<<< HEAD
## 步骤7：配置连接器

成功安装后，您应该配置连接器以连接到不同的设备，请使用以文档对连接器进行相关配置：
 - [**MQTT**](/docs/iot-gateway/config/mqtt/)
 - [**OPC-UA**](/docs/iot-gateway/config/opc-ua/)
 - [**Modbus**](/docs/iot-gateway/config/modbus/)
 - [**BLE**](/docs/iot-gateway/config/ble/)
 - [**自定义**](/docs/iot-gateway/custom/)
=======
## Step 7: Configure connectors

After successful installation you should configure the connectors to connect to different devices, please use one (or more) following articles to configure connector files:  
 - [**MQTT** connector](/docs/iot-gateway/config/mqtt/)
 - [**OPC-UA** connector](/docs/iot-gateway/config/opc-ua/)
 - [**Modbus** connector](/docs/iot-gateway/config/modbus/)
 - [**BLE** connector](/docs/iot-gateway/config/ble/)
 - [**Request** connector](/docs/iot-gateway/config/request/)
 - [**CAN** connector](/docs/iot-gateway/config/can/)
 - [**Custom** connector](/docs/iot-gateway/custom/)
>>>>>>> 60998c9f8de4fabf0dca7662aa6da14e1bf9225a
