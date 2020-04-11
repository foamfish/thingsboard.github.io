---
layout: docwithnav
assignees:
- ashvayka
title: 设备属性
description: 通过ThingsBoard属性对IoT设备进行管理

---

* TOC
{:toc}

ThingsBoard能够为您的实体分配自定义属性并管理这些属性。属性被视为键值对。键值格式的灵活性和简单性允许与市场上几乎所有IoT设备轻松无缝地集成。


## 视频教程

<div id="video">
  <div id="video_wrapper">
    <iframe src="https://www.youtube.com/embed/JCW_hShAp7I" frameborder="0" allowfullscreen=""></iframe>
  </div>
</div>


## 属性类型

属性主要分为三种:

 - **服务端属性** - 由服务器管理和报告，在Thingsboard中使用规则会涉及某些机密数据，对设备应用程序不可见。在物联网规则可能使用其中一部份机密数据，但设备不会使用。在Thingsboard中任何实体都支持服务器端属性（如：Device, Asset, Customer, Tenant, Rules）
     
   {:refdef: style="text-align: center;"}
   ![image](/images/user-guide/server-side-attributes.svg)
   {: refdef}  

 - **客户端属性** - 查看设备特定的属性 
 - **共享属性** - 查看设备特定的属性


## 设备特定属性类型

所有属性可以在[规则引擎](/docs/user-guide/rule-engine)组件中使用；如（filters，processors，actions）
本指南概述了上面列出的功能以及相关链接，可以获取更多信息。  

设备特定属性可分为两种:
 
 - **客户端属性** - 属性由设备应用程序报告和管理。 例如当前软件的固件版本，硬件规格等。 

   {:refdef: style="text-align: center;"}
   ![image](/images/user-guide/client-side-attributes.svg)
   {: refdef}  
        
 - **共享属性** - 属性由服务器端应用程序报告和管理。 对设备是可见的。 （例如客户订阅，软件版本）
   
   {:refdef: style="text-align: center;"}
   ![image](/images/user-guide/shared-attributes.svg)
   {: refdef}  

## 设备属性API

ThingsBoard为设备应用程序提供以下API：
 
 - 上传客户端属性至服务器。
 - 请求服务器中的客户端属性和共享属性。
 - 订阅共享属性的更新。

属性API仅支持特定的网络协议。你可以在相应的参考页面中查看API和示例：

 - [MQTT API属性介绍](/docs/reference/mqtt-api/#attributes-api)
 - [CoAP API属性介绍](/docs/reference/coap-api/#attributes-api)
 - [HTTP API属性介绍](/docs/reference/http-api/#attributes-api)
  
## 遥测服务

遥测服务负责将属性数据持久保存到内部数据库中； 提供服务器端API来查询和订阅属性更新。

### 内部数据存储

ThingsBoard使用Cassandra NoSQL数据库或SQL数据库来存储所有数据。

虽然您可以直接查询数据库，但是ThingsBoard提供了一组RESTful和Websocket API，可简化调用过程并应用某些安全策略：
 
 - Tenant管理员能够管理所拥有实体属性。
 - Customer用户只能管理Tenant分配的实体属性。
  
### 数据查询API

遥测服务提供以下REST API来获取实体数据:

![image](/images/user-guide/telemetry-service/rest-api.png)

**注意:**上图中的API可通过Swagger UI使用，如获取更多详细信息请查看[REST API](/docs/reference/rest-api/) 。
该API向后兼容TB v1.0 +，这是API调用URL包含“plugin”的主要原因。

#### 属性keys API

您可以使用下面的GET请求地址获取指定entity类型和entity id的所有属性key列表
 
```shell
http(s)://host:port/api/plugins/telemetry/{entityType}/{entityId}/keys/attributes
```

{% capture tabspec %}get-attributes-keys
A,get-attributes-keys.sh,shell,resources/get-attributes-keys.sh,/docs/user-guide/resources/get-attributes-keys.sh
B,get-attributes-keys-result.json,json,resources/get-attributes-keys-result.json,/docs/user-guide/resources/get-attributes-keys-result.json{% endcapture %}
{% include tabs.html %}

支持的实体类型为: TENANT, CUSTOMER, USER, RULE, DASHBOARD, ASSET, DEVICE, ALARM

#### 属性values API


您可以使用下面的GET请求地址获取指定entity类型和entity id的所有属性value列表 
 
```shell
http(s)://host:port/api/plugins/telemetry/{entityType}/{entityId}/values/attributes?keys=key1,key2,key3
```

{% capture tabspec %}get-telemetry-values
A,get-attributes-values.sh,shell,resources/get-attributes-values.sh,/docs/user-guide/resources/get-attributes-values.sh
B,get-attributes-values-result.json,json,resources/get-attributes-values-result.json,/docs/user-guide/resources/get-attributes-values-result.json{% endcapture %}
{% include tabs.html %}

支持的实体类型为: TENANT, CUSTOMER, USER, RULE, DASHBOARD, ASSET, DEVICE, ALARM

### 遥测规则节点

规则引擎中有一些规则节点，可以与遥测服务一起使用。 请在节点描述中找到更多详细信息:

- [**Enrichment Nodes - 加载实体最新遥测数据**](/docs/user-guide/rule-engine-2-0/enrichment-nodes/)
- [**Save Timeseries**](/docs/user-guide/rule-engine-2-0/action-nodes/#save-timeseries-node)
- [**Save Attributes**](/docs/user-guide/rule-engine-2-0/action-nodes/#save-attributes-node)

## 数据可视化

ThingsBoard提供了配置和自定义仪表板以进行数据可视化的功能。点击以下链接了解详情
<p><a href="/docs/user-guide/visualization" class="button">数据可视化指南</a></p>

## 规则引擎

ThingsBoard提供了配置数据处理规则的功能。 设备属性可在规则过滤器内使用。 这允许基于某些设备属性应用规则。点击以下链接了解详情
<p><a href="/docs/user-guide/rule-engine" class="button">规则引擎指南</a></p>
    
