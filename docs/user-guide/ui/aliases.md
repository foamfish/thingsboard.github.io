---
layout: docwithnav
assignees:
- yefimov-andrey
title: 别名
description: ThingsBoard别名

---

* TOC
{:toc}

创建仪表板必须首先创建别名，以便定义将使用实体的数据。别名可以像引用单个设备一样容易，也可以像为列表中的特定资产创建复杂的搜索查询一样复杂。

在本指南中，别名将在具有以下方案的系统上使用：

![image](/images/user-guide/ui/alias-scheme.png)

并且所有设备都具有“ water_level”生成的值。

## 别名类型

### 单一实体

该别名允许选择一个实体，可以是设备，资产，实体视图，租户，客户，仪表板，数据转换器，调度程序事件，blob实体或当前客户。

<img data-gifffer="/images/user-guide/ui/single-entity-alias.gif" />

此别名过滤一个设备，在这种情况下为设备A.

### 实体组

该别名允许选择单个实体组，可以是客户组，资产组或设备组。

<img data-gifffer="/images/user-guide/ui/group-entity-alias.gif" />

该别名过滤设备组在这种情况下为灌溉系统。

### 实体列表

该别名允许无需输入查询即可手动选择多个实体，这些实体可以是设备，资产，实体视图，租户，客户，仪表板，数据转换器），调度程序事件，blob实体或客户。

<img data-gifffer="/images/user-guide/ui/entity-list-alias.gif" />

### 实体名称

此别名允许选择一个或多个实体名称，该实体名称以输入的查询开头，可以是设备，资产，实体视图，承租人，客户，仪表板，数据转换器，调度程序事件，blob实体或客户。
 
<img data-gifffer="/images/user-guide/ui/entity-name-alias.gif" />

 该别名过滤名称以“ Device”开头的设备。
 
### 实体组列表

该别名允许无需输入查询即可手动选择多个实体组，可以是设备组，资产组，实体视图组，客户组，仪表板组或用户组。

<img data-gifffer="/images/user-guide/ui/entity-group-list-alias.gif" />
 
### 实体组名称

该别名允许选择几个以输入的查询开头的实体组名称，可以是设备组，资产组，实体视图组，客户组，仪表板组或用户组。

对于此示例，创建了一个名为“灌溉机器”的空设备组。

<img data-gifffer="/images/user-guide/ui/entity-group-name-alias.gif" />
 
 该别名过滤名称以“ Irrigation”开头的设备组。
 
### 仪表板状态中的实体

该别名允许从仪表板状态选择实体，这些实体可以是设备，资产，实体视图，租户，客户，仪表板，数据转换器，调度程序事件，blob实体或当前客户。

它用于过滤其他仪表板状态的数据，例如，如果“时间序列”小部件是在根仪表板状态上创建的，上面显示了几个实体，而您想创建一个仪表板状态，该状态将显示带有您单击的实体的小部件，您需要使用此别名。

在以下示例中，别名在创建组实体别名后使用。
 
 <img data-gifffer="/images/user-guide/ui/entity-dashboard-state-alias.gif" />

### 资产类型

该别名允许选择输入类型（如果需要）的资产，其名称以输入的查询开头。

 <img data-gifffer="/images/user-guide/ui/asset-type-alias.gif" />
 
该别名过滤类型为“ field”且名称以“ House”开头的资产。
 
### 设备类型

该别名允许选择输入类型（如果需要）的设备，其名称以输入的查询开头。

 <img data-gifffer="/images/user-guide/ui/device-type-alias.gif" />
 
 此别名过滤“设备”类型的设备。
 
### 实体视图类型

使用该别名可以选择输入类型（如果需要）的实体视图，其名称以输入的查询开头。

创建了一个名为“设备-D-实体视图”的实体视图，其类型为“示例类型”，可从设备D访问“ water_level”时间序列。

 <img data-gifffer="/images/user-guide/ui/entity-view-type-alias.gif" />
 
此别名过滤类型为“ example-type”且名称以“ Device”开头的实体视图。

### 关系查询

该别名允许选择与指定的发起者相关的实体，直到指定的级别和指定的方向。

 <img data-gifffer="/images/user-guide/ui/relations-query-alias.gif" />

此别名过滤从资产“街道A”到关系级别2之间具有任何关系的实体。

### 资产搜索查询

该别名允许选择与指定的发起者相关的指定类型的资产，直到指定级别和方向为止。

 <img data-gifffer="/images/user-guide/ui/Asset-search-query-alias.gif" />

此别名过滤类型为“字段”的资产，该资产与级别为1的设备“设备D”有任何关系。

### 设备搜索查询

该别名允许选择与指定的发起者相关的指定类型的设备，直到指定级别和指定方向为止。

 <img data-gifffer="/images/user-guide/ui/Device-search-query-alias.gif" />

此别名过滤类型为“设备”的设备，这些设备具有从资产“房屋C”到关系级别1的任何关系。

### 实体视图搜索查询

此别名允许选择指定类型的实体视图，这些视图与指定的始发者相关联，直到指定的级别和方向为止。

使用类型“ example-type”创建了一个名为“设备-D-实体视图”的实体视图，该视图与设备D具有“包含”关系，该实体视图提供了对设备D的“ water_level”时间序列的访问。

 <img data-gifffer="/images/user-guide/ui/entity-view-type-search-query-alias.gif" />
 
此别名过滤类型为“ example-type”的实体视图，该视图具有从设备“设备D”到关系级别1的任何关系。

