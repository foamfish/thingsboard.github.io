---
layout: docwithnav
assignees:
- ashvayka
title: 部件库
description: ThingsBoard仪表板部件库

---

* TOC
{:toc}

## 简介

所有[IoT仪表板](/docs/user-guide/ui/dashboards/)都是使用ThingsBoard平台中的部件库中定义的Widget进行构建。每个部件都提供对应功能让用户进行操作，例如数据可视化，远程设备控制，警报管理以及显示静态自定义html内容。

## 部件类型

根据提供的功能每个部件窗口定义代表特定的部件类型窗口。平台提供五种类型：

 - [Latest values](#latest-values)
 - [Time-series](#time-series)
 - [RPC (Control widget)](#rpc-control-widget)
 - [Alarm widget](#alarm-widget)
 - [Static](#static)
 
每种部件类型都有自己特定的数据源配置和相应的部件API。

每个小部件都需要数据源才能进行数据可视化。

可用数据源的类型取决于窗口部件的窗口部件类型：

 - 目标设备 - RPC中使用此数据源类型。 基本上你需要为RPC部件指定目标设备
 - 警报源 - 警报部件中使用此数据源类型. 此数据源要求源实体显示相关的警报和相应的警报字段。
 - 实体 - 时间序列和最新值部件均使用此数据源类型. 基本上你需要指定目标实体和时间序列key或attribute名称。
 - 函数 - 此数据源类型在时间序列和最新值部件中均用于调试。基本上你可以指定一个javascript函数，该函数将模拟设备中的数据以调整可视化效果。

### Latest values

显示特定实体属性或时间序列数据点的最新值（例如，任何“仪表”部件或“实体列表”部件）。这种小部件使用实体属性或时间序列的值作为数据源。
 
![image](/images/user-guide/ui/widgets/latest-values-datasource.png)

以下是最新值部件的示例-显示当前功率值的数字仪表。

![image](/images/user-guide/ui/widgets/latest-values-widget-example.png)

### Time-series
       
显示选定时间段的历史值或特定时间窗口中的最新值（例如“时间序列-浮点”或“时间序列表”）。
这种部件仅将实体时间序列的值用作数据源。
为了指定显示值的时间范围，使用**Timewindow**设置。
可以在仪表板级别或部件级别指定Timewindow。
它可以是实时-动态更改某个最近间隔的时间范围，也可以是历史-固定历史时间范围。所有这些设置都是 **Time-series**窗口部件配置的一部分。 
 
![image](/images/user-guide/ui/widgets/time-series-datasource.png)

以下是时间序列部件的示例-“Timeseries - Flot”实时显示三个设备的安培数。

![image](/images/user-guide/ui/widgets/time-series-widget-example.png)

### RPC (Control部件)

允许将RPC命令发送到设备并处理/可视化来自设备的答复（例如“ Raspberry Pi GPIO控制”）。通过将目标设备指定为RPC命令的目标端点来配置RPC窗口小部件。

![image](/images/user-guide/ui/widgets/rpc-datasource.png)

以下是RPC小部件的示例-“基本GPIO控制”-发送GPIO切换命令并检测当前的GPIO切换状态。

![image](/images/user-guide/ui/widgets/rpc-widget-example.png)

### Alarm部件

在特定时间窗口中显示与指定实体相关的警报（例如“警报表”）。
通过将实体指定为警报源和相应的警报字段来配置警报窗口小部件。
像**Time-series widgets**一样，警报部件具有时间窗口配置，以便指定显示警报的时间范围。另外，配置还包含“Alarm status”和“Alarms polling interval”参数。“ Alarm status”参数指定正在获取的警报的状态。“Alarms polling interval”控制警报获取频率（以秒为单位）。

![image](/images/user-guide/ui/widgets/alarm-datasource.png) 

以下是“警报”窗口小部件的示例-“警报表”实时显示资产的最新警报

![image](/images/user-guide/ui/widgets/alarm-widget-example.png)

### Static

显示静态的可定制html内容（例如“ HTML卡”）。静态小部件不使用任何数据源，通常通过指定静态html内容和可选的CSS样式进行配置

![image](/images/user-guide/ui/widgets/static-html.png)

以下是一个静态小部件的示例-显示指定html内容的“ HTML卡”。

![image](/images/user-guide/ui/widgets/static-widget-example.png) 
 
## Widgets Library (Bundles)

部件定义根据其用途分为部件包。有系统级别和租户级别的**部件包**。
最初的ThingsBoard安装随附基本的系统级**部件包**。
现成可用的七个部件包中有三十多个部件。
系统级别部件包可以由**系统管理员管理**，并且可供系统中的任何租户使用。
租户级别部件包可以由**租户管理员管理**，并且只能由该租户及其客户使用。您始终可以按照本[指南](/docs/user-guide/contribution/widgets-development/)实施和添加小部件。
 
![image](/images/user-guide/ui/widget-bundles.png)
 
### Digital Gauges
 
用于可视化温度，湿度，速度和其他整数或浮点值。

![image](/images/user-guide/ui/digital-gauges.png)

### Analog Gauges
 
与数字压力计相似，但样式不同。

![image](/images/user-guide/ui/analog-gauges.png)


### Charts
 
使用时间窗口可视化历史或实时数据很有用。

![image](/images/user-guide/ui/charts.png)

### GPIO部件
 
用于可视化和控制目标设备的GPIO状态。

![image](/images/user-guide/ui/gpio-widgets.png)

### Control部件
 
用于可视化当前状态并将RPC命令发送到目标设备。

![image](/images/user-guide/ui/control-widgets.png)

### Maps部件
 
对于可视化设备地理位置以及跟踪实时和历史记录模式的设备路线很有用。

![image](/images/user-guide/ui/maps-widgets.png)

### Cards
 
用于可视化表格或卡片部件中的时间序列数据或属性。

![image](/images/user-guide/ui/cards.png)

### Alarm部件

对于实时和历史模式下特定实体的警报可视化很有用。

![image](/images/user-guide/ui/alarm-widgets.png)

### Gateway部件

对管理扩展很有用。

![image](/images/user-guide/ui/gateway-widgets.png)

### Input部件

对于更改实体属性很有用。

![image](/images/user-guide/ui/input-widgets.png)

## Widgets Bundles 导入/导出

#### Widgets Bundle导出


<<<<<<< HEAD
您可以将部件导出为JSON格式，然后将其导入相同或另一个ThingsBoard实例。

为了导出部件包，您应该导航到**Widgets Library**页面，然后单击位于特定部件包卡上的导出按钮。
=======
In order to export widgets bundle, you should navigate to the **Widgets Library** page and click on the export button located on the particular widgets bundle row.
 
>>>>>>> master
![image](/images/user-guide/ui/export-widgets-bundle.png)

#### Widgets Bundle导入

<<<<<<< HEAD
将需要导入窗口部件类型，您应该导航到“窗口小部件库”页面，然后打开窗口小部件包，然后单击屏幕右下方的大“ +”按钮，然后单击导入按钮。
=======
Similar, to import the widgets bundle you should navigate to the **Widgets Library** page and click on the "+" button in the top-right corner of the **Widgets Bundles** table and then choose "Import widgets bundle" option. 
>>>>>>> master

![image](/images/user-guide/ui/import-widgets-bundle.png)

窗口小部件类型导入窗口将显示一个弹出窗口，并且将提示您上传json文件。

![image](/images/user-guide/ui/import-widgets-bundle-window.png)

## Widgets Types 导入/导出

#### Widget Type导出

您可以将部件导出为JSON格式，然后将其导入相同或另一个ThingsBoard实例。

为了导出部件包，您应该导航到**Widgets Library**页面，然后单击位于特定部件包卡上的导出按钮。
 
![image](/images/user-guide/ui/export-widget-type.png)

#### Widget Type导入

将需要导入窗口部件类型，您应该导航到“窗口小部件库”页面，然后打开窗口小部件包，然后单击屏幕右下方的大“ +”按钮，然后单击导入按钮。

![image](/images/user-guide/ui/import-widget-type.png)

窗口小部件类型导入窗口将显示一个弹出窗口，并且将提示您上传json文件。

![image](/images/user-guide/ui/import-widget-type-window.png)
