---
layout: docwithnav
assignees:
- ashvayka
title: 实体和关系
description: 通过ThingsBoard中实体和关系对IoT资产进行管理

---

* TOC
{:toc}

## 实体概述

ThingsBoard提供了用户界面和REST API，方便在IoT应用程序中配置和管理多种实体类型及其关系。支持的实体如下:
 
 - **租户** - 您可以将租户视为独立的业务实体：拥有或生产设备和资产的个人或组织；租户可能有多个租户管理员用户和数百万个客户；
 - **客户** - 客户也是一个独立的企业实体，购买或使用租户下的Device、Assets、Organization;客户可能有多个用户以及数百万个Device和Assets；
 - **用户** - 用户能够浏览仪表板和管理实体；
 - **设备** - 可以通过RPC命令处理Iot设备中的对象遥测数据。例如sensors（传感器）, actuators（执行器）, switches（开关）；
 - **资产** - Device与Assets相关联的抽象Iot对象。例如factory（工厂）, field（字段）, vehicle（车辆）；
 - **警报** - 提示Device和Assets以及Entity发生的事件；
 - **面板** - 通过Dashboards查看数据以及控制指定设备； 
 - **规则节点** - 通过消怎处理实体生命周期事件的单元；
 - **规则链** - 规则节点的逻辑单元；


实体支持如下:

 - **属性** - 与实体相关联的静态和半静态键值对。例如序列号，型号，固件版本；
 - **遥测数据** - 可用于存储，查询和可视化的时间序列数据点。例如温度，湿度，电池电量;
 - **关系** - 与其他实体的定向连接。例如包含，管理，拥有，生产.
 
此外，Device和Assets也具有一种类型。这允许区分它们并以不同方式处理与他们相关的数据。
   
本指南概述了上面列出的功能，一些有用的链接，以获取更多详细信息以及其用法的真实示例。 

## 应用场景

理解ThingsBoard概念的最简单方法是实现您的第一个ThingsBoard应用程序。假设我们要构建一个应用程序，该应用程序从土壤湿度和温度传感器收集数据，在仪表板上可视化该数据，检测问题，发出警报并控制灌溉。
我们还假设我们想用数百个传感器支持多个领域。字段也可以分组到地理区域。
我们认为应该遵循以下逻辑步骤来构建这样的应用程序：

### 步骤1: 实体和关系

我们可以按图中的层次关系在Thingsboard Web UI中进行设置:


 ![image](/images/user-guide/entities-and-relations.svg)
 
 
观看下方视频，可以了解如何使用ThingsBoard Web UI设置区域和字段资产及其关系

  
<div id="video">
    <div id="video_wrapper">
        <iframe src="https://www.youtube.com/embed/C-JoOfTBeT0" frameborder="0" allow="accelerometer; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>
</div>

观看下方视频，可以了解如何使用ThingsBoard Web UI配置设备及其与资产的关系


<div id="video">
    <div id="video_wrapper">
        <iframe src="https://www.youtube.com/embed/BUFinxvzIo4" frameborder="0" allow="accelerometer; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>
</div>

您可以使用ThingsBoard REST API操作执行POST请求创建一个新资产。URL格式如下：

```shell 
http(s)://host:port/api/asset
```

例如:

{% capture tabspec %}create-asset
A,create-asset.sh,shell,resources/create-asset.sh,/docs/user-guide/resources/create-asset.sh
B,create-asset.json,json,resources/create-asset.json,/docs/user-guide/resources/create-asset.json{% endcapture %}  
{% include tabs.html %}

**注意:** 如果你要执行此请求, 你需要求将 **$JWT_TOKEN** 进行替换成相关的JWT令牌。
此令牌属于**TENANT_ADMIN** 的角色用户. 你可以按照 [指南](/docs/reference/rest-api/#rest-api-auth) 获取令牌.

你可以使用POST请求设置一个新关系，URL格式如下：

```shell 
http(s)://host:port/api/relation
```

例如

{% capture tabspec %}create-relation
A,create-relation.sh,shell,resources/create-relation.sh,/docs/user-guide/resources/create-relation.sh
B,create-relation.json,json,resources/create-relation.json,/docs/user-guide/resources/create-relation.json{% endcapture %}  
{% include tabs.html %}

**注意:** 请将$FROM_ASSET_ID 和 $TO_ASSET_ID 替换成有效的参数值。
**注意:** 可以进行任何有效的关联，例如：资产到设备或资产到用户。你可以通过REST API进行设置或者基于后台界面（Web UI）。

### 步骤2: 分配资产属性
   
ThingsBoard提供了将属性分配给实体并对其进行管理的功能。点击以下链接了解详情
<p><a href="/docs/user-guide/attributes" class="button">设备属性</a></p>


### 步骤3: 上传设备遥测数据

ThingsBoard提供了使用设备和其他实体的遥测数据的功能。点击以下链接了解详情  
<p><a href="/docs/user-guide/telemetry" class="button">遥测数据</a></p>

### 步骤4: 创建报警规则

ThingsBoard提供了使用规则引擎为设备和其他实体引发警报的功能。点击以下链接了解详情
<p><a href="/docs/user-guide/alarms" class="button">警报</a></p>

### 步骤5: 设计仪表板

请[导入](/docs/user-guide/ui/dashboards/#dashboard-import)这个[**面板**](/docs/user-guide/resources/region_fields_dashboard.json)以便查Map、Alarm、Entity表格 和Charts部件.


 


 
    
