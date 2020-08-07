---
layout: docwithnav
assignees:
- ashvayka
title: MQTT设备API参考
description: 支持物联网设备的MQTT API参考

---

* TOC
{:toc}

## 入门

##### MQTT基础

[MQTT](https://en.wikipedia.org/wiki/MQTT)是一种轻量级的发布-订阅消息传递协议，它可能最适合各种物联网设备。你可以在[此处](http://mqtt.org/)找到有关MQTT的更多信息。

ThingsBoard服务器节点充当支持QoS级别0（最多一次）和QoS级别1（至少一次）以及一组预定义主题的MQTT代理。

##### 客户端

你可以在网上找到大量的MQTT客户端库。本文中的示例将基于Mosquitto和MQTT.js。您可以使用我们的[Hello World](/docs/getting-started-guides/helloworld/)指南中的说明。


##### MQTT连接

我们将在本文中使用令牌凭据对进行设备访问，这些凭证稍后将称为  **$ACCESS_TOKEN** 。应用程序需要发送用户名包含  **$ACCESS_TOKEN** 的MQTT CONNECT消息。
连接期间可能的返回码及其原因：

<<<<<<< HEAD
* **0x00 连接成功** - 成功连接
* **0x04 连接失败** - 用户名或密码错误。
* **0x05 连接未授权** - -用户名包含无效的 **$ACCESS_TOKEN**。

## Key-value格式

ThingsBoard支持JSON以key-value的格式。键始终是一个字符串，而值可以是字符串，布尔值，双精度或长整数。也可以使用自定义二进制格式或某些序列化框架。有关更多详细信息，请参见协议[自定义](#protocol-customization)。

例如：

```json
{"stringKey":"value1", "booleanKey":true, "doubleKey":42.0, "longKey":73}
```
=======
{% include templates/api/key-value-format.md %}
>>>>>>> master

## 遥测上传API

为了将遥测数据发布到ThingsBoard服务器节点，请将PUBLISH消息发送到以下主题：
 
```shell
v1/devices/me/telemetry
```

支持的最简单的数据格式是：

```json
{"key1":"value1", "key2":"value2"}
```

或者

```json
[{"key1":"value1"}, {"key2":"value2"}]
```

**请注意** 在这种情况下，服务器端时间戳将分配给上传的数据!

如果您的设备能够获得客户端时间戳，则可以使用以下格式：


```json
{"ts":1451649600512, "values":{"key1":"value1", "key2":"value2"}}
```

在上面的示例中，我们假设“1451649600512”是具有毫秒精度的[Unix时间戳](https://en.wikipedia.org/wiki/Unix_time)。例如，值'1451649600512'对应于'星期五，2016年1月1日12：00：00.512 GMT'


{% capture tabspec %}mqtt-telemetry-upload
A,Mosquitto,shell,resources/mosquitto-telemetry.sh,/docs/reference/resources/mosquitto-telemetry.sh
B,MQTT.js,shell,resources/mqtt-js-telemetry.sh,/docs/reference/resources/mqtt-js-telemetry.sh
C,telemetry-data-as-object.json,json,resources/telemetry-data-as-object.json,/docs/reference/resources/telemetry-data-as-object.json
D,telemetry-data-as-array.json,json,resources/telemetry-data-as-array.json,/docs/reference/resources/telemetry-data-as-array.json
E,telemetry-data-with-ts.json,json,resources/telemetry-data-with-ts.json,/docs/reference/resources/telemetry-data-with-ts.json{% endcapture %}
{% include tabs.html %}

 
## 属性API

ThingsBoard属性API能够使设备具备如下功能

* 将[客户端](/docs/user-guide/attributes/#attribute-types)设备属性上载到服务器
* 从服务器请求[客户端和共享](/docs/user-guide/attributes/#attribute-types)设备属性
* 从服务器订阅 [共享](/docs/user-guide/attributes/#attribute-types)设备属性
 
##### 将属性更新发布到服务器

为了将客户端设备属性发布到ThingsBoard服务器节点，请将PUBLISH消息发送到以下主题:

```shell
v1/devices/me/attributes
```

{% capture tabspec %}mqtt-attributes-upload
A,Mosquitto,shell,resources/mosquitto-attributes-publish.sh,/docs/reference/resources/mosquitto-attributes-publish.sh
B,MQTT.js,shell,resources/mqtt-js-attributes-publish.sh,/docs/reference/resources/mqtt-js-attributes-publish.sh
C,new-attributes-values.json,json,resources/new-attributes-values.json,/docs/reference/resources/new-attributes-values.json{% endcapture %}
{% include tabs.html %}

##### 从服务器请求属性值

为了向ThingsBoard服务器节点请求客户端或共享设备属性，请将PUBLISH消息发送到以下主题:

```shell
v1/devices/me/attributes/request/$request_id
```

其中 **$request_id** 是您的整数请求标识符。在发送带有请求的PUBLISH消息之前，客户端需要订阅

```shell
v1/devices/me/attributes/response/+
```

以下示例是用javascript编写的，基于mqtt.js。纯命令行示例不可用，因为订阅和发布需要在同一mqtt会话中进行。

{% capture tabspec %}mqtt-attributes-request
A,MQTT.js,shell,resources/mqtt-js-attributes-request.sh,/docs/reference/resources/mqtt-js-attributes-request.sh
B,mqtt-js-attributes-request.js,javascript,resources/mqtt-js-attributes-request.js,/docs/reference/resources/mqtt-js-attributes-request.js
C,Result,json,resources/attributes-response.json,/docs/reference/resources/attributes-response.json{% endcapture %}
{% include tabs.html %}

**请注意**, 客户端和共享设备属性键的交集是不好的做法！但是，对于客户端，共享甚至服务器端属性，仍然可能具有相同的密钥。

##### 从服务器订阅属性更新

为了订阅共享设备属性更改，请发送SUBSCRIBE消息到以下主题：

```shell
v1/devices/me/attributes
```

当服务器端组件之一（例如REST API或规则链）更改了共享属性时，客户端将收到以下更新：

```json
{"key1":"value1"}
```

{% capture tabspec %}mqtt-attributes-subscribe
A,Mosquitto,shell,resources/mosquitto-attributes-subscribe.sh,/docs/reference/resources/mosquitto-attributes-subscribe.sh
B,MQTT.js,shell,resources/mqtt-js-attributes-subscribe.sh,/docs/reference/resources/mqtt-js-attributes-subscribe.sh{% endcapture %}
{% include tabs.html %}

## RPC API

### 服务器端RPC

为了从服务器订阅RPC命令，请将SUBSCRIBE消息发送到以下主题：

```shell
v1/devices/me/rpc/request/+
```

订阅后，客户端将收到一条单独的命令，作为对相应主题的发布消息：

```shell
v1/devices/me/rpc/request/$request_id
```

其中 **$request_id** 是整数请求标识符。

客户应发布对以下主题的响应：

```shell
v1/devices/me/rpc/response/$request_id
```

以下示例是用javascript编写的，基于mqtt.js。纯命令行示例不可用，因为订阅和发布需要在同一mqtt会话中进行。

{% capture tabspec %}mqtt-rpc-from-server
A,MQTT.js,shell,resources/mqtt-js-rpc-from-server.sh,/docs/reference/resources/mqtt-js-rpc-from-server.sh
B,mqtt-js-rpc-from-server.js,javascript,resources/mqtt-js-rpc-from-server.js,/docs/reference/resources/mqtt-js-rpc-from-server.js{% endcapture %}  
{% include tabs.html %}

### 客户端RPC

为了将RPC命令发送到服务器，请将PUBLISH消息发送到以下主题：

```shell
v1/devices/me/rpc/request/$request_id
```

其中 **$request_id** 是整数请求标识符。来自服务器的响应将发布到以下主题：

```shell
v1/devices/me/rpc/response/$request_id
```

以下示例是用javascript编写的，基于mqtt.js。纯命令行示例不可用，因为订阅和发布需要在同一mqtt会话中进行。

{% capture tabspec %}mqtt-rpc-from-client
A,MQTT.js,shell,resources/mqtt-js-rpc-from-client.sh,/docs/reference/resources/mqtt-js-rpc-from-client.sh
B,mqtt-js-rpc-from-client.js,javascript,resources/mqtt-js-rpc-from-client.js,/docs/reference/resources/mqtt-js-rpc-from-client.js{% endcapture %}  
{% include tabs.html %}

## 声明设备

请参阅相应的文章以获取有关[声明设备](/docs/user-guide/claiming-devices)功能的更多信息。

为了启动声明设备，请向以下主题发送PUBLISH消息：

```shell
v1/devices/me/claim
```

支持的数据格式为：

```json
{"secretKey":"value", "durationMs":60000}
```

**请注意** 以上字段是可选的。如果未指定**secretKey**，则使用空字符串作为默认值。**device.claim.duration**毫秒未指定时，系统参数device.claim.duration被使用（在文件 **/etc/thingsboard/conf/thingsboard.yml** ）

## 自定义协议

通过更改相应的[模块](https://github.com/thingsboard/thingsboard/tree/master/transport/mqtt)，可以针对特定用例完全定制MQTT传输。


## 下一步

{% assign currentGuide = "ConnectYourDevice" %}{% include templates/guides-banner.md %}
