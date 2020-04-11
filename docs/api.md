---
layout: docwithnav
title: ThingsBoard API指南
description: ThingsBoard API和支持的IOT协议

---

ThingsBoard API包含两个主要部分：设备API和服务器端API。

设备API按支持的通信协议分组：

* [**MQTT API**](/docs/reference/mqtt-api)
* [**CoAP API**](/docs/reference/coap-api)
* [**HTTP API**](/docs/reference/http-api)

[**Gateway MQTT API**](/docs/reference/gateway-mqtt-api) 允许您使用 **[ThingsBoard Gateway](/docs/iot-gateway/what-is-iot-gateway/)**
或使用自己的网关将**现有**设备连接到平台。

服务器端API可以作为REST API使用:

* [**Administration REST API**](/docs/reference/rest-api) - 服务器端核心API。
* [**Attributes query API**](/docs/user-guide/attributes/#data-query-api) - [Telemetry Service](/docs/user-guide/attributes/)提供的服务端API.
* [**Timeseries query API**](/docs/user-guide/telemetry/#data-query-api) - [Telemetry Service](/docs/user-guide/telemetry/)提供的服务端API.
* [**RPC API**](/docs/user-guide/rpc/#server-side-rpc-api) - [RPC Service](/docs/user-guide/rpc/)提供的服务端API.
