---
layout: docwithnav
assignees:
- ashvayka
title: CoAP设备API参考

---

* TOC
{:toc}

## 入门

##### CoAP基础

[CoAP](https://en.wikipedia.org/wiki/Constrained_Application_Protocol)是轻量级物联网协议用于受限的设备。您可以在[此处](https://tools.ietf.org/html/rfc7252)找到有关CoAP的更多信息。CoAP协议基于UDP，但与HTTP类似它使用请求-响应模型。CoAP观察[选项](https://tools.ietf.org/html/rfc7641)允许订阅资源并接收有关资源更改的通知。

ThingsBoard服务器节点充当支持常规请求和观察请求的CoAP服务器。

##### 客户端

你可以在网上找到针对不同编程语言的CoAP客户端库。本文中的示例将基于[CoAP cli](https://www.npmjs.com/package/coap-cli)作为使用工具，您可以使用我们的[Hello World](/docs/getting-started-guides/helloworld/)指南中的说明。

##### CoAP身份验证和错误代码

我们将在本文中使用令牌凭证访问设备，这些凭证稍后将称为 **$ACCESS_TOKEN**。应用程序需要在每个CoAP请求中包括 **$ACCESS_TOKEN** 作为路径参数。可能的错误代码及其原因：

* **4.00 请求无效** - 请求参数与正文错误，请求无效。
* **4.01 未授权** - **$ACCESS_TOKEN**无效。
* **4.04 未找到** - 找不到资源。

<<<<<<< HEAD
## Key-value格式

ThingsBoard支持JSON中的key-value内容。键始终是一个字符串，而值可以是字符串，布尔值，双精度或长整数。也可以使用自定义二进制格式或某些序列化框架。有关更多详细信息，请参见[自定义协议](#protocol-customization)协议。
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
coap://host/api/v1/$ACCESS_TOKEN/telemetry
```

支持的最简单的数据格式是：

```json
{"key1":"value1", "key2":"value2"}
```

或者

```json
[{"key1":"value1"}, {"key2":"value2"}]
```

**请注意**，在这种情况下，服务器端时间戳将分配给上传的数据！

如果您的设备能够获得客户端时间戳，则可以使用以下格式：


```json
{"ts":1451649600512, "values":{"key1":"value1", "key2":"value2"}}
```

In the example above, we assume that "1451649600512" is a [unix timestamp](https://en.wikipedia.org/wiki/Unix_time) with milliseconds precision.
For example, the value '1451649600512' corresponds to 'Fri, 01 Jan 2016 12:00:00.512 GMT'

{% capture tabspec %}coap-telemetry-upload
A,Example,shell,resources/coap-telemetry.sh,/docs/reference/resources/coap-telemetry.sh
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
coap://host/api/v1/$ACCESS_TOKEN/attributes
```

{% capture tabspec %}coap-attributes-upload
A,Example,shell,resources/coap-attributes-publish.sh,/docs/reference/resources/coap-attributes-publish.sh
B,new-attributes-values.json,json,resources/new-attributes-values.json,/docs/reference/resources/new-attributes-values.json{% endcapture %}
{% include tabs.html %}

##### 从服务器请求属性值

为了向ThingsBoard服务器节点请求客户端或共享设备属性，请将GET请求发送到以下URL：

```shell
coap://host/api/v1/$ACCESS_TOKEN/attributes?clientKeys=attribute1,attribute2&sharedKeys=shared1,shared2
```


{% capture tabspec %}coap-attributes-request
A,Example,shell,resources/coap-attributes-request.sh,/docs/reference/resources/coap-attributes-request.sh
B,Result,json,resources/attributes-response.json,/docs/reference/resources/attributes-response.json{% endcapture %}
{% include tabs.html %}

**请注意**，客户端和共享设备属性键的交集是不好的做法！但是，对于客户端，共享甚至服务器端属性，仍然可能具有相同的密钥。

##### 从服务器订阅属性更新

为了订阅共享设备属性更改，请将带有Observe选项的GET请求发送到以下URL：

```shell
coap://host/api/v1/$ACCESS_TOKEN/attributes
```

一旦服务器端组件之一（REST API或规则链）更改了共享属性，客户端将收到以下更新：

{% capture tabspec %}coap-attributes-subscribe
A,Example,shell,resources/coap-attributes-subscribe.sh,/docs/reference/resources/coap-attributes-subscribe.sh
B,Result,json,resources/attributes-response.json,/docs/reference/resources/attributes-response.json{% endcapture %}
{% include tabs.html %}

## RPC API

##### 服务器端RPC

为了从服务器订阅RPC命令，请将带有观察标志的GET请求发送到以下URL：

```shell
coap://host/api/v1/$ACCESS_TOKEN/rpc
```

订阅后，客户端可以接收rpc请求。RPC请求正文的示例如下所示：

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
coap://host/api/v1/$ACCESS_TOKEN/rpc/{$id}
```

其中 **$id** 是整数请求标识符。

{% capture tabspec %}coap-rpc-command
A,Example Subscribe,shell,resources/coap-rpc-subscribe.sh,/docs/reference/resources/coap-rpc-subscribe.sh
B,Example Reply,shell,resources/coap-rpc-reply.sh,/docs/reference/resources/coap-rpc-reply.sh
C,Reply Body,shell,resources/rpc-response.json,/docs/reference/resources/rpc-response.json{% endcapture %}
{% include tabs.html %}

##### 客户端RPC

为了将RPC命令发送到服务器，请将POST请求发送到以下URL：

```shell
coap://host/api/v1/$ACCESS_TOKEN/rpc
```

请求和响应主体都应该是有效的JSON文档。文档的内容特定于将处理您的请求的规则节点。

{% capture tabspec %}coap-rpc-from-client
A,Example Request,shell,resources/coap-rpc-from-client.sh,/docs/reference/resources/coap-rpc-from-client.sh
B,Request Body,shell,resources/rpc-client-request.json,/docs/reference/resources/rpc-client-request.json
C,Response Body,shell,resources/rpc-server-response.json,/docs/reference/resources/rpc-server-response.json{% endcapture %}
{% include tabs.html %}
  
## 声明设备

请参阅相应的文章以获取有关[声明设备](/docs/user-guide/claiming-devices)功能的更多信息。

为了启动声明设备，请将POST请求发送到以下URL：
 
```shell
coap://host/api/v1/$ACCESS_TOKEN/claim
```

支持的数据格式为：

```json
{"secretKey":"value", "durationMs":60000}
```

**请注意**，以上字段是可选的。如果未指定**secretKey**，则使用空字符串作为默认值。万一**durationMs**毫秒未指定时，系统参数**device.claim.duration**被使用（在文件**/etc/thingsboard/conf/thingsboard.yml**）。
  
## 自定义协议

通过更改相应的[模型](https://github.com/thingsboard/thingsboard/tree/master/transport/coap)，可以针对特定用例完全定制CoAP传输。


## 下一步

{% assign currentGuide = "ConnectYourDevice" %}{% include templates/guides-banner.md %}
