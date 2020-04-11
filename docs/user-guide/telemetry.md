---
layout: docwithnav
assignees:
- ashvayka
title: 遥测数据
description: 基于相关IoT协议和ThingsBoard中遥测功能对IoT设备时间序列数据进行收集
---

* TOC
{:toc}

ThingsBoard提供与遥测数据操作相关的API：

 - **采集** 使用MQTT, CoAP or HTTP协议采集设备数据。
 - **存储** 在Cassandra（高效、可扩展、能容错的NoSQL数据库）中存储时序数据。
 - **查询** 查询最新时序数据值，或查询特定时间段内的所有数据。
 - **订阅** 使用websockets订阅数据更新(用于可视化或实时分析)。
 - **可视化** 使用可配置和可配置的小部件以及仪表盘可视化时序数据。
 - **过滤和分析** 使用灵活的规则引擎过滤和分析数据(/docs/user-guide/rule-engine/)。
 - **事件警报** 根据采集的数据触发事件警报。
 - **数据传输** 过规则引擎节点实现与外部数据交互（例如Kafka或RabbitMQ规则节点）。

本指南概述了上面列出的功能以及相关链接，可以获取更多信息。 

![image](/images/user-guide/telemetry.svg)

## 设备遥测上传API

ThingsBoard提供了一个用于上传时间序列键值对的数据的API。
键值格式的灵活性和简单性允许与市场上几乎所有IoT设备轻松无缝地集成。
遥测上传API基于特定的网络协议。您可以在相应的参考页面中查看API和示例：

 - [MQTT API遥测介绍](/docs/reference/mqtt-api/#telemetry-upload-api)
 - [CoAP API遥测介绍](/docs/reference/coap-api/#telemetry-upload-api)
 - [HTTP API遥测介绍](/docs/reference/http-api/#telemetry-upload-api)
  
## 遥测服务

遥测服务负责将时间序列数据持久保存至数据库中；提供服务器端API来查询和订阅数据更新服务。

### 数据存储

ThingsBoard使用Cassandra NoSQL数据库或SQL数据库来存储所有数据。

向服务器发送数据的设备将在数据存储在DB中后立即收到有关数据传递的确认。

MQTT客户端允许临时本地存储未交付的数据。

因此，即使ThingsBoard节点之一发生故障，该设备也不会丢失数据，并且能够将其推送到其他服务器。

服务器端应用程序还能够发布针对不同实体和实体类型有价值的遥测。

虽然您可以直接查询数据库，但是ThingsBoard提供了一组RESTful和Websocket API，可简化调用过程并应用某些安全策略：
 
 - Tenant管理员能够管理所拥有实体属性。
 - Customer用户只能管理Tenant分配的实体属性。
  
#### 数据查询API

遥测服务提供以下REST API来获取实体数据:

![image](/images/user-guide/telemetry-service/rest-api.png)

**注意:** 上图中的API可通过Swagger UI使用，如获取更多详细信息请查看[REST API](/docs/reference/rest-api/) 。
该API向后兼容TB v1.0 +，这是API调用URL包含“plugin”的主要原因。
##### Timeseries data keys API

您可以使用下面的GET请求地址获取指定entity类型和entity id的所有属性key列表  
 
```shell
http(s)://host:port/api/plugins/telemetry/{entityType}/{entityId}/keys/timeseries
```

{% capture tabspec %}get-telemetry-keys
A,get-telemetry-keys.sh,shell,resources/get-telemetry-keys.sh,/docs/user-guide/resources/get-telemetry-keys.sh
B,get-telemetry-keys-result.json,json,resources/get-telemetry-keys-result.json,/docs/user-guide/resources/get-telemetry-keys-result.json{% endcapture %}
{% include tabs.html %}

支持的实体类型为: TENANT, CUSTOMER, USER, DASHBOARD, ASSET, DEVICE, ALARM, ENTITY_VIEW

##### Timeseries data values API

您可以使用下面的GET请求地址获取指定entity类型和entity id的所有属性value列表
 
```shell
http(s)://host:port/api/plugins/telemetry/{entityType}/{entityId}/values/timeseries?keys=key1,key2,key3
```

{% capture tabspec %}get-latest-telemetry-values
A,get-latest-telemetry-values.sh,shell,resources/get-latest-telemetry-values.sh,/docs/user-guide/resources/get-latest-telemetry-values.sh
B,get-latest-telemetry-values-result.json,json,resources/get-latest-telemetry-values-result.json,/docs/user-guide/resources/get-latest-telemetry-values-result.json{% endcapture %}
{% include tabs.html %}

支持的实体类型为: TENANT, CUSTOMER, USER, DASHBOARD, ASSET, DEVICE, ALARM, ENTITY_VIEW

您可以使用下面的GET请求地址获取指定entity类型和entity id的所有属性历史value列表 
 
```shell
http(s)://host:port/api/plugins/telemetry/{entityType}/{entityId}/values/timeseries?keys=key1,key2,key3&startTs=1479735870785&endTs=1479735871858&interval=60000&limit=100&agg=AVG
```

支持参数如下所述：

 - **keys** - 以逗号分隔的要获取的遥测键列表。
 - **startTs** - Unix时间戳，标识间隔的开始（以毫秒为单位）。
 - **endTs** - -Unix时间戳，标识间隔的结束时间（以毫秒为单位）。
 - **interval** - 聚合间隔，以毫秒为单位。
 - **agg** - 聚合函数。MIN，MAX，AVG，SUM，COUNT，NONE之一。
 - **limit** - 要返回的最大数据点数或要处理的间隔。

hingsBoard将使用startTs，endTs和interval来标识聚合分区或子查询，并对利用内置聚合功能的数据库执行异步查询。

{% capture tabspec %}get-telemetry-values
A,get-telemetry-values.sh,shell,resources/get-telemetry-values.sh,/docs/user-guide/resources/get-telemetry-values.sh
B,get-telemetry-values-result.json,json,resources/get-telemetry-values-result.json,/docs/user-guide/resources/get-telemetry-values-result.json{% endcapture %}
{% include tabs.html %}

支持的实体类型为: TENANT, CUSTOMER, USER, DASHBOARD, ASSET, DEVICE, ALARM, ENTITY_VIEW

#### Websocket API

ThingsBoard Web UI使用Websocket API复制了REST API功能，并提供了订阅设备数据更改的功能。

您可以使用以下URL打开与遥测服务的Websocket连接。

```shell
ws(s)://host:port/api/ws/plugins/telemetry?token=$JWT_TOKEN
```

打开后，您可以发送
[订阅命令](https://github.com/thingsboard/thingsboard/blob/master/application/src/main/java/org/thingsboard/server/service/telemetry/cmd/TelemetryPluginCmdsWrapper.java) 
并接收
[订阅更新](https://github.com/thingsboard/thingsboard/blob/master/application/src/main/java/org/thingsboard/server/service/telemetry/sub/SubscriptionUpdate.java):

 - **cmdId** - 命令唯一标识（在相应的Websocket连接中）
 - **entityType** - 唯一实体类型标识. 支持实类型: TENANT, CUSTOMER, USER, DASHBOARD, ASSET, DEVICE, ALARM
 - **entityId** - 唯一实体标识符
 - **keys** - 逗号分隔的keys参数列表
 - **timeWindow** - 订阅的时间间隔，以毫秒为单位. 数据将在以下时间间隔 **[now()-timeWindow, now()]**
 - **startTs** - 获取历史数据查询的间隔的开始时间（以毫秒为单位）.
 - **endTs** - 获取历史数据查询的间隔的结束时间（以毫秒为单位）.
 
##### 实例 

更改以下变量的值: 

 - **token** - 指向JWT令牌，您可以使用[以下链接](https://thingsboard.io/docs/reference/rest-api/#rest-api-auth)获得该令牌.

 - **entityId** - 您的设备ID.
 
 如果是现场演示服务器: 
 
 - 用**demo-thingsboard.io**替换**host:port**并选择安全连接-**wss://**
 
 如果是本地安装:
 
 - 用**127.0.0.1:8080**替换**host:port**并选择**ws://**
 
{% capture tabspec %}web-socket
A,web-socket.html,html,resources/web-socket.html,/docs/user-guide/resources/web-socket.html{% endcapture %}  
{% include tabs.html %}

## 数据可视化

ThingsBoard提供了配置和自定义仪表板以进行数据可视化的功能。点击以下链接了解详情
<p><a href="/docs/user-guide/visualization" class="button">数据可视化指南</a></p>

## 规则引擎

ThingsBoard提供了配置数据处理规则的功能。每个规则包括

 - filters - 过滤传入的数据供稿, 
 - processor -生成警报或使用某些服务器端值丰富输入数据
 - action - 对过滤的数据应用某种逻辑.
点击以下链接了解详情    
<p><a href="/docs/user-guide/rule-engine" class="button">规则引擎指南</a></p>
    
