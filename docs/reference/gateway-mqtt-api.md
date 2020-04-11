---
layout: docwithnav
assignees:
- ashvayka
title: MQTT网关API参考
description: 支持物联网设备的MQTT网关API参考
---

* TOC
{:toc}

## 集成

网关是ThingsBoard中的一种特殊类型的设备，能够充当连接到不同系统的外部设备和ThingsBoard之间的桥梁。
网关API提供了使用**单个MQTT**连接在**多个设备**和平台之间交换数据的功能。
网关还充当ThingsBoard设备，并且可以利用现有的[MQTT设备API](/docs/reference/mqtt-api/)来报告统计信息，接收配置更新等等。

以下列出的API由[ThiTngsBoard开源IoT网关](/docs/iot-gateway/what-is-iot-gateway/)使用。

## 基础MQTT API

请参考[通用MQTT设备](/docs/reference/mqtt-api/)API以获取有关数据格式，身份验证选项等的信息
 
## 设备连接API

通知ThingsBoard设备已连接到网关，需要发布以下消息：
 
```shell
Topic: v1/gateway/connect
Message: {"device":"Device A"}
```

**Device A** 代表你的设备名称。

收到后ThingsBoard将查找或创建具有指定名称的设备。另外，ThingsBoard还将向该网关发布有关特定设备的新属性更新和RPC命令的消息。

## 设备断开API

为了通知ThingsBoard设备已与网关断开连接，需要发布以下消息：
 
```shell
Topic: v1/gateway/disconnect
Message: {"device":"Device A"}
```

**Device A** 代表你的设备名称。

一旦收到ThingsBoard将不再将此特定设备的更新发布到此网关。

## 属性API

ThingsBoard属性API能够使设备具备如下功能

* 将[客户端](/docs/user-guide/attributes/#attribute-types)设备属性上载到服务器
* 从服务器请求[客户端和共享](/docs/user-guide/attributes/#attribute-types)设备属性
* 从服务器订阅 [共享](/docs/user-guide/attributes/#attribute-types)设备属性
 
##### 将属性更新发布到服务器

为了将客户端设备属性发布到ThingsBoard服务器节点，请将PUBLISH消息发送到以下主题：

```shell
Topic: v1/gateway/attributes
Message: {"Device A":{"attribute1":"value1", "attribute2": 42}, "Device B":{"attribute1":"value1", "attribute2": 42}}
```

其中**Device A**和**Device B**是你的设备名称，**attribute1**和**attribute2**是属性键。

##### 从服务器请求属性值

为了向ThingsBoard服务器节点请求客户端或共享设备属性，请将PUBLISH消息发送到以下主题：

```shell
Topic: v1/gateway/attributes/request
Message: {"id": $request_id, "device": "Device A", "client": true, "key": "attribute1"}
```

其中 **$request_id** 是您的整数请求标识符，**Device A**是你的设备名称，**client**标识客户端或共享属性范围，而**key**是属性键。

在发送带有请求的PUBLISH消息之前，客户端需要订阅

```shell
Topic: v1/gateway/attributes/response
```

消息结果格式如下：

```shell
Message: {"id": $request_id, "device": "Device A", "value": "value1"}
```

##### 从服务器订阅属性更新

为了订阅共享设备属性更改，请发送SUBSCRIBE消息到以下主题

```shell
v1/gateway/attributes
```

消息结果格式如下：

```shell
Message: {"device": "Device A", "data": {"attribute1": "value1", "attribute2": 42}}
```

## 遥测上传API

为了将设备遥测发布到ThingsBoard服务器节点，请将PUBLISH消息发送到以下主题：

```shell
Topic: v1/gateway/telemetry
```

消息结构:

```json
{
  "Device A": [
    {
      "ts": 1483228800000,
      "values": {
        "temperature": 42,
        "humidity": 80
      }
    },
    {
      "ts": 1483228801000,
      "values": {
        "temperature": 43,
        "humidity": 82
      }
    }
  ],
  "Device B": [
    {
      "ts": 1483228800000,
      "values": {
        "temperature": 42,
        "humidity": 80
      }
    }
  ]
}
```

其中**Device A**和**Device B**是你的设备名称，“**temperature**和**humidity**”是遥测键，**ts**是unix时间戳（以毫秒为单位）。

## RPC API

### 服务器端RPC

为了从服务器订阅RPC命令，请将SUBSCRIBE消息发送到以下主题：

```shell
v1/gateway/rpc
```

使用以下带有单个命令的消息格式：

```shell
{"device": "Device A", "data": {"id": $request_id, "method": "toggle_gpio", "params": {"pin":1}}}
```

设备处理完命令后，网关可以使用以下格式将命令发送回：

```shell
{"device": "Device A", "id": $request_id, "data": {"success": true}}
```
**$request_id**是你的整数请求标识符，**Device A**是你的设备名称，方法是您的RPC方法**method**。 

## 声明设备API

请参阅相应的文章以获取有关[声明设备](/docs/user-guide/claiming-devices)功能的更多信息。

为了启动声明设备，请向以下主题发送PUBLISH消息：

```shell
Topic: v1/gateway/claim
```

消息结构:

```json
{
  "Device A": {
    "secretKey": "value_A",
    "durationMs": 60000
  },
  "Device B": {
    "secretKey": "value_B",
    "durationMs": 60000
  }
}
```

其中**Device A** 和 **Device B**是你的设备名称，**secretKey** 和 **durationMs**是可选密钥。如果未指定 **secretKey** ，则使用空字符串作为默认值。万一 **device.claim.duration** 毫秒未指定时，系统参数 **device.claim.duration** 被使用（在文件 **/etc/thingsboard/conf/thingsboard.yml** ）。

## 自定义协议

通过更改相应的[模块](https://github.com/thingsboard/thingsboard/tree/master/transport/mqtt)，可以针对特定用例完全定制MQTT传输。

## 下一步

{% assign currentGuide = "ConnectYourDevice" %}{% include templates/guides-banner.md %}
