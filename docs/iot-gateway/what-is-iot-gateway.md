---
layout: docwithnav
title: ThingsBoard网关是什么？
description: ThingsBoard网关的功能和优势

---

Thingsboard**网关**是一个开放源代码的解决方案可让您使用Thingsboard集成连接到旧系统和第三方系统的设备。

Thingsboard是用于数据收集，处理，可视化和设备管理的开源物联网平台。如果您是新的初次使用请参见[**什么是Thingsboard**](/docs/getting-started-guides/what-is-thingsboard/)。

#### 网关特色

ThingsBoard网关提供以下特色：

 - [**MQTT**](/docs/iot-gateway/config/mqtt/)用于控制，配置和收集来自使用现有协议连接到外部MQTT代理的IoT设备的数据。
 - [**OPC-UA**](/docs/iot-gateway/config/opc-ua/)用于从连接到OPC-UA服务器的设备收集数据。
 - [**Modbus**](/docs/iot-gateway/config/modbus/)用于收集通过Modbus协议连接的设备的数据。
 - [**BLE**](/docs/iot-gateway/config/ble/)从使用低功耗蓝牙连接的设备收集数据。
 - [**Custom**](/docs/iot-gateway/custom/)用于从通过不同协议连接的IoT设备收集数据。
 - **持久性**所收集数据的以确保在发生网络或硬件故障时能够进行数据传递。
 - **自动重新连接**至ThingsBoard集群。
 - 将传入的数据和消息简单而强大地**映射为统一格式**。


#### 架构

网关是一个软件组件旨在在支持**Python 3.5+**的基于Linux的微型计算机上运行。

下面列出了ThingsBoard网关的主要组件。

**连接器**

该组件的目的是连接到外部系统（例如MQTT代理或OPC-UA服务器）或直接连接到设备（例如Modbus或BLE）。

连接后，连接器要么从那些系统中轮询数据，要么订阅更新。轮询与订阅取决于协议功能。

例如，我们对MQTT连接器使用订阅模型，对Modbus使用轮询模型。

连接器还能够直接或通过外部系统将更新推送到设备。

可以使用[自定义指南](/docs/iot-gateway/custom/)定义自己的连接器。

**转换器**
 
转换器负责将数据从特定的协议格式转换为ThingsBoard格式。

转换器由连接器调用。转换器通常特定于连接器支持的协议。

有上行链路和下行链路转换器。上行转换器用于将数据从特定协议转换为ThingsBoard格式。

下行转换器用于将消息从ThingsBoard转换为特定的协议格式。

可以使用[自定义指南](/docs/iot-gateway/custom/)定义自己的转换器。

**事件存储**

事件存储用于临时存储连接器产生的遥测和其他事件，直到将它们传送到ThingsBoard。

事件存储支持两种实现：内存队列和持久性文件存储。

两种实现方式都可以确保在网络中断的情况下最终提交设备数据。

内存中队列可最大程度地减少IO操作，但如果网关进程重新启动，则可能会丢失消息。

持久性文件存储在重新启动过程后仍然有效，但会对文件系统执行IO操作。

**ThingsBoard客户端**

网关通过MQTT协议与ThingsBoard通信，并使用[此处](/docs/reference/gateway-mqtt-api/)中所述的API。

ThingsBoard客户端是一个单独的线程，该线程轮询事件存储并在与ThingsBoard的连接处于活动状态后传递消息。

ThingsBoard客户端支持监视连接性，批处理事件以提高性能和许多其他功能。

**网关服务**

网关服务负责连接器，事件存储和ThingsBoard客户端的引导。

该服务收集并定期向ThingsBoard报告有关传入消息和连接的设备的统计信息。

网关服务会保留已连接设备的列表，以便在网关重新启动的情况下能够重新订阅设备配置更新。

{:refdef: style="text-align: center;"}
![ThingsBoard IoT Gateway architecture](/images/gateway/python-gateway.png)
{: refdef}
  

#### 路线图

<p><a href="/docs/iot-gateway/roadmap" class="button">网关</a></p>

#### 下一步

<p><a href="/docs/iot-gateway/getting-started" class="button">入门指南</a></p>
