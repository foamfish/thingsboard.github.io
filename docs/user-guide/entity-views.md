---
layout: docwithnav
assignees:
- ashvayka
title: 实体视频
description: IoT设备和资产实体视图
redirect_from: "/docs/user-guide/ui/entity-views"
---

* TOC
{:toc}

## 功能概述

从v2.2开始Thingsboard实体视图功能可以使用，它与SQL数据库的视图类似，Thingsboard EV限制了基础表向外公开数据的可见程度同时还限制了设备、资产[遥测](/docs/user-guide/telemetry/)和[属性](/docs/user-guide/attributes/)向[客户](/docs/user-guide/ui/customers/)公开的可见程度。

Tenant管理员可以为每一个设备或创建多个视图（EV）并为之分配给不同的客户。

支持说明如下:
 
 - 允许同时将指定设备或资产数据**共享**给多个客户，由于ThingsBoard安全模型的限制以前的实体视图（EV）无法实现。
 - 允许指定的用户查看采集的数据（例如：传感器数据），但隐藏调试信息例如电量、系统错误等。
 - 设备即服务(**DaaS**)模型，表示设备在不同时间段收集的数据属于不同的客户。

## 架构

实体视图包含如下信息:

 - **TenantId** - 表示视图所属租户;
 - **CustomerId** - 表示视图所属访问者;
 - **EntityId** - 表示视图所属设备、资产;
 - **Name and type** - 表示在ThingsBoard中进行常规搜索的字段;
 - **Start and end time** - 表示目标设备遥测访问的时间间隔，客户将不可见超出时间间隔的实体; 
 - **Timeseries keys** - 表示可以访问数据序列键列表;
 - **Attribute keys** - 表示可以访问的属性键列表;

![image](/images/user-guide/entity-views/new-entity-view.png) 
 
了解ThingsBoard如何处理遥测和属性及修改将如何影响实体视图。
  
#### Timeseries data视图
 
所有时间序列数据都保存在目标数据库中，将不会存在相同数据。当用户打开仪表板或通过EntityID执行RESTAPI调用时会发生如下操作:
     
 - 通过验证请求的开始时间戳和结束时间戳并将有效数据进行返回，如果Dashboard获取1年的数据，但是实体视图（EV）配置为只能获取6个月的数据所以请求将只返回6个月数据。
 - 通过验证请求时间序列数据的密钥将有效数据返回，如果Dashboard获取禁止视图的遥测键时将会失败。
 
#### 属性视图
 
每次保存或更新该实体视图时，实体视图都会自动从目标实体复制指定的属性。出于性能原因，每次属性更改时，目标实体属性都不会传播到实体视图。您可以通过在规则链中配置“Copy to VIew”规则节点和“Post attributes”和“Attributes Updated”消息链接到新规则节点来启用自动传播。
 
![image](/images/user-guide/entity-views/rule-chain.png) 

## 改进和不足

ThingsBoard路线图:

 - 添加对PRC启用/禁用请求到设备视图的功功能。
 - 添加指定视图可以访问（传播）警报列表的功能。

## 下一步

{% assign currentGuide = "AdvancedFeatures" %}{% include templates/guides-banner.md %}



 


 
    
