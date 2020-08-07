---
layout: docwithnav
assignees:
- ashvayka
title: HTTP设备API参考
description: 支持物联网设备的HTTP API参考

---

* TOC
{:toc}

## 入门

##### HTTP基础

[HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)是可在IoT应用程序中使用的通用网络协议。您可以在此处找到有关HTTP的更多信息。HTTP协议基于TCP，并使用请求-响应模型。

ThingsBoard服务器节点充当同时支持HTTP和HTTPS协议的HTTP Server。

##### 客户端

您可以在网络上找到用于不同编程语言的HTTP客户端库。本文中的示例将基于[curl](https://en.wikipedia.org/wiki/CURL)。为了设置此工具，您可以使用我们的[Hello World](/docs/getting-started-guides/helloworld/)指南中的说明。

##### HTTP身份验证

我们将在本文中使用访问令牌设备凭证，这些凭证稍后将称为 **$ACCESS_TOKEN**。应用程序需要在每个HTTP请求中包括 **$ACCESS_TOKEN** 作为路径参数。可能的错误代码及其原因：

* **400** - 无效的请求地址.
* **401** - 无效的 **$ACCESS_TOKEN**.
* **404** - 未找到.

<<<<<<< HEAD
## Key-value格式

ThingsBoard支持JSON中的键值内容。键始终是一个字符串，而值可以是字符串，布尔值，双精度或长整数。也可以使用自定义二进制格式或某些序列化框架。有关更多详细信息，请参见[自定义协议](#protocol-customization)。

例如：

```json
{"stringKey":"value1", "booleanKey":true, "doubleKey":42.0, "longKey":73}
```
=======
{% include templates/api/key-value-format.md %}

>>>>>>> master

## 遥测上传API

为了将遥测数据发布到ThingsBoard服务器节点，请将POST请求发送到以下URL：
 
```shell
http(s)://host:port/api/v1/$ACCESS_TOKEN/telemetry
```

支持的最简单的数据格式是：

```json
{"key1":"value1", "key2":"value2"}
```

或者

```json
[{"key1":"value1"}, {"key2":"value2"}]
```

**请注意** 在这种情况下，服务器端时间戳将分配给上传的数据！

如果您的设备能够获得客户端时间戳，则可以使用以下格式:


```json
{"ts":1451649600512, "values":{"key1":"value1", "key2":"value2"}}
```

在上面的示例中，我们假设“ 1451649600512”是具有毫秒精度的[Unix时间戳](https://en.wikipedia.org/wiki/Unix_time)。例如，值'1451649600512'对应于'星期五，2016年1月1日12：00：00.512 GMT'

{% capture tabspec %}http-telemetry-upload
A,Example,shell,resources/http-telemetry.sh,/docs/reference/resources/http-telemetry.sh
B,telemetry-data-as-object.json,json,resources/telemetry-data-as-object.json,/docs/reference/resources/telemetry-data-as-object.json
C,telemetry-data-as-array.json,json,resources/telemetry-data-as-array.json,/docs/reference/resources/telemetry-data-as-array.json
D,telemetry-data-with-ts.json,json,resources/telemetry-data-with-ts.json,/docs/reference/resources/telemetry-data-with-ts.json{% endcapture %}
{% include tabs.html %}

 
## 属性API

ThingsBoard属性API能够使设备具备如下功能

* 将[客户端](/docs/user-guide/attributes/#attribute-types)设备属性上载到服务器
* 从服务器请求[客户端和共享](/docs/user-guide/attributes/#attribute-types)设备属性
* 从服务器订阅 [共享](/docs/user-guide/attributes/#attribute-types)设备属性
 
##### 将属性更新发布到服务器

为了将客户端设备属性发布到ThingsBoard服务器节点，请将POST请求发送到以下URL：

```shell
http(s)://host:port/api/v1/$ACCESS_TOKEN/attributes
```

{% capture tabspec %}http-attributes-upload
A,Example,shell,resources/http-attributes-publish.sh,/docs/reference/resources/http-attributes-publish.sh
C,new-attributes-values.json,json,resources/new-attributes-values.json,/docs/reference/resources/new-attributes-values.json{% endcapture %}
{% include tabs.html %}

##### 从服务器请求属性值

为了向ThingsBoard服务器节点请求客户端或共享设备属性，请将GET请求发送到以下URL：

```shell
http(s)://host:port/api/v1/$ACCESS_TOKEN/attributes?clientKeys=attribute1,attribute2&sharedKeys=shared1,shared2
```


{% capture tabspec %}http-attributes-request
A,Example,shell,resources/http-attributes-request.sh,/docs/reference/resources/http-attributes-request.sh
B,Result,json,resources/attributes-response.json,/docs/reference/resources/attributes-response.json{% endcapture %}
{% include tabs.html %}

**请注意**，客户端和共享设备属性键的交集是不好的做法！但是，对于客户端，共享甚至服务器端属性，仍然可能具有相同的密钥。

##### 从服务器订阅属性更新

为了订阅共享设备属性更改，请将带有可选“ timeout”请求参数的GET请求发送到以下URL：

```shell
http(s)://host:port/api/v1/$ACCESS_TOKEN/attributes/updates
```

一旦服务器端组件之一（REST API或规则链）更改了共享属性，客户端将收到以下更新：

{% capture tabspec %}http-attributes-subscribe
A,Example,shell,resources/http-attributes-subscribe.sh,/docs/reference/resources/http-attributes-subscribe.sh
B,Result,json,resources/attributes-response.json,/docs/reference/resources/attributes-response.json{% endcapture %}
{% include tabs.html %}

## RPC API

### 服务器端RPC

为了从服务器订阅RPC命令，请将带有可选的“超时”请求参数的GET请求发送到以下URL：

```shell
http(s)://host:port/api/v1/$ACCESS_TOKEN/rpc
```

订阅后，如果没有对特定设备的请求，则客户端可以接收rpc请求或超时消息。RPC请求正文的示例如下所示：

```json
{
  "id": "1",
  "method": "setGpio",
  "params": {
    "pin": "23",
    "value": 1
  }
}
```
 

 - **id** - 请求ID，int
 - **method** - RPC方法名称, string
 - **params** - -RPC方法参数，自定义json对象 

并可以使用POST请求回复以下网址：

```shell
http://host:port/api/v1/$ACCESS_TOKEN/rpc/{$id}
```

其中 **$id** 是整数请求标识符。

{% capture tabspec %}http-rpc-from-server
A,Example Subscribe,shell,resources/http-rpc-subscribe.sh,/docs/reference/resources/http-rpc-subscribe.sh
B,Example Reply,shell,resources/http-rpc-reply.sh,/docs/reference/resources/http-rpc-reply.sh
C,Reply Body,shell,resources/rpc-response.json,/docs/reference/resources/rpc-response.json{% endcapture %}
{% include tabs.html %}

### 客户端RPC

为了将RPC命令发送到服务器，请将POST请求发送到以下URL：

```shell
http://host:port/api/v1/$ACCESS_TOKEN/rpc
```

请求和响应主体都应该是有效的JSON文档。这些文档的内容特定于将处理您的请求的规则节点。

{% capture tabspec %}http-rpc-from-client
A,Example Request,shell,resources/http-rpc-from-client.sh,/docs/reference/resources/http-rpc-from-client.sh
B,Request Body,shell,resources/rpc-client-request.json,/docs/reference/resources/rpc-client-request.json
C,Response Body,shell,resources/rpc-server-response.json,/docs/reference/resources/rpc-server-response.json{% endcapture %}  
{% include tabs.html %}

## 声明设备

请参阅相应的文章以获取有关[声明设备](/docs/user-guide/claiming-devices)功能的更多信息。

为了启动声明设备，请将POST请求发送到以下URL：
 
```shell
http(s)://host:port/api/v1/$ACCESS_TOKEN/claim
```

支持的数据格式为：

```json
{"secretKey":"value", "durationMs":60000}
```

**请注意**，以上字段是可选的。如果未指定**secretKey**，则使用空字符串作为默认值。如果**durationMs**毫秒未指定时，系统参数**device.claim.duration**被使用（在文件**/etc/thingsboard/conf/thingsboard.yml**）。
  
## 自定义协议

通过更改相应的[模块](https://github.com/thingsboard/thingsboard/tree/master/transport/http)，可以针对特定用例完全定制HTTP传输。


## 下一步

{% assign currentGuide = "ConnectYourDevice" %}{% include templates/guides-banner.md %}
