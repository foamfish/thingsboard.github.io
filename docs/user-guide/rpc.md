---
layout: docwithnav
assignees:
- ashvayka
title: RPC功能
description: 通过ThingsBoardr提供的RPC功能对IoT设备进行IoT云远程控制
---

* TOC
{:toc}

ThingsBoard允许你从服务器端应用程序向设备发送远程RPC调用，你也可以将命令发送到设备并接收命令执行的结果。同样，您可以执行来自设备的请求，在后端应用进行某些计算或服务器端逻辑处理然后将结果反馈到设备。

本指南涵盖ThingsBoard RPC功能，阅读本指南后你将掌握以下内容：

- RPC调用类型
- 基础RPC实例
- RPC客户端和服务器端API
- RPC部件

## RPC调用类型

Thinsboard RPC功能可以分为两种类型：设备发起的RPC调用和服务器发起的RPC调用。
为了使用更熟悉的名称，我们将源自设备的RPC调用命名为**客户端**RPC调用，将源自服务器的RPC调用命名为**服务器端**RPC调用。
  
   {:refdef: style="text-align: center;"}
   ![image](/images/user-guide/client-side-rpc.svg)
   {: refdef}  

服务器端RPC调用可以分为单向和双向：
 
 - 	单向RPC请求直接发送请求，并且不对设备响应做任何处理。在超时期间内没有与之连接，RPC调用才可能失败。
   
   {:refdef: style="text-align: center;"}
   ![image](/images/user-guide/one-way-rpc.svg)
   {: refdef}
   
 - 双向RPC请求会发送到设备，并在超时期间内接收到来自设备的响应。在服务端没有找到请求目标会被断开。

   {:refdef: style="text-align: center;"}
   ![image](/images/user-guide/two-way-rpc.svg)
   {: refdef}


## 设备RPC API

ThingsBoard提供了便捷的API，可以从设备上运行的应用程序发送和接收RPC命令。你可以在相应的参考页面中查看API和示例：

 - [MQTT RPC API介绍](/docs/reference/mqtt-api/#rpc-api)
 - [CoAP RPC API介绍](/docs/reference/coap-api/#rpc-api)
 - [HTTP RPC API介绍](/docs/reference/http-api/#rpc-api) 

## 服务器端RPC API

ThingsBoard提供了**系统RPC服务**，可以将RPC调用从服务器端应用程序发送到设备。为了发送RPC请求，你需要对以下URL执行HTTP POST请求

```shell
http(s)://host:port/api/plugins/rpc/{callType}/{deviceId}
```
url说明：
 - **callType** 表示 **单向**或者**双向**
 - **deviceId** 表示 [设备ID](/docs/user-guide/ui/devices/#get-device-id)

必填参数: 
 
 - **method** - 表示json字符串格式方法名
 - **params** - 表示json对象格式参数列表

例如:

{% capture tabspec %}mqtt-rpc-from-client
A,set-gpio-request.sh,shell,resources/set-gpio-request.sh,/docs/user-guide/resources/set-gpio-request.sh
B,set-gpio-request.json,json,resources/set-gpio-request.json,/docs/user-guide/resources/set-gpio-request.json{% endcapture %}  
{% include tabs.html %}

**注意** 为了执行此请求，您将需要用有效的JWT令牌替换**$JWT_TOKEN**
该令牌应属于

 - 拥有**TENANT_ADMIN**角色的用户
 - 拥有**CUSTOMER_USER**角色的用户，该用户拥有**$DEVICE_ID**标识的设备
 
您可以按照以下[指南](/docs/reference/rest-api/#rest-api-auth)获取令牌.

## RPC规则节点
可以将RPC动作集成到处理工作流程中。有2个用于处理RPC请求的规则节点。

-  [RPC回复](/docs/user-guide/rule-engine-2-0/action-nodes/#rpc-call-reply-node) 
-  [RPC请求](/docs/user-guide/rule-engine-2-0/action-nodes/#rpc-call-request-node) 

## RPC部件

了解更多详细信息 点击[部件](/docs/user-guide/ui/widget-library/#gpio-widgets)查看更多内容。