---
layout: docwithnav
title: 轨迹动画部件
description: 轨迹动画部件 

---

{% assign feature = "Platform Integrations" %}

* TOC
{:toc}

## 概述

在此示例中，我们将研究画部件功能。

该小部件可能适用于不同的用例，但主要是可以用于实时跟踪，研究实体的运动并对其进行可视化。

本指南是在ThingsBoard专业版的[cloud]（https://cloud.thingsboard.io）版本2.4.2上编写的，因此某些步骤与Community Edition有所不同。

它具有所有其他版本的功能。

## 轨迹设置动画部件

首先，您需要创建一个将收集遥测信息的设备。

另外，您可以使用具有遥测坐标（经度和纬度）的设备。

这可以是任何实时接收其坐标的设备。

我们的设备会接收其经度，纬度，速度，状态和多边形坐标。

经度和纬度是地图可视化的关键数据，因此您将在所选仪表板的小部件上看到它。

在我们的示例中，我们使用[模拟器](/docs/user-guide/resources/timeseries-map-bus.js)
用javascript编写以接收遥测并在仪表板上可视化它。

### 创建仪表板

我们需要创建一个仪表板，在其中可以看到遥测。如果您的目标是跟踪实体在特定时期内的移动方式，这可能会很有用。

我们可以使用现有的或为我们的新用例创建一个新的仪表板。

在我们的示例中，出于指导目的，我们创建了一个名为“ Dashboard1”的新仪表板。

### 添加部件

现在，我们将打开空的仪表板并进行编辑。

![image](/images/user-guide/ui/widgets/trip-animation-widget/1.png)

现在我们有一个空的仪表板。让我们填充一些内容。

![image](/images/user-guide/ui/widgets/trip-animation-widget/2.png)

首先，我们需要创建一个**alias**以指定将从中接收遥测数据的实体。

本指南中的实体将是我们先前创建的 **“Tracker1”** 设备。我们将使用 **“GeoData1”** 名称作为别名。

![image](/images/user-guide/ui/widgets/trip-animation-widget/3.png)

现在我们要添加一个小部件！

![image](/images/user-guide/ui/widgets/trip-animation-widget/4.png)

轨迹动画部件位于地图中

![image](/images/user-guide/ui/widgets/trip-animation-widget/5.png)

In our widget we add **coordinates**, **latitude**, **longitude**, speed and status from our alias **“GeoData1”** as our parameters. 

They have the same labels as their keys are. Secondly, we create a widget on which we will visualize our telemetry. 

We use **Trip Animation Widget** in our guide. It’s located in **Maps Bundle, Time series tab**. 

Also, we’ll go for “Use dashboard timewindow” so that we’ll make it easier to synchronise our data. 

![image](/images/user-guide/ui/widgets/trip-animation-widget/6.png)

In addition to this, we’ll use last minute received data to visualize and change aggregation function to None, because we don’t need to guess possible data value for the next time period, we receive data in realtime without any errors.

![image](/images/user-guide/ui/widgets/trip-animation-widget/8.png)

Finally, we turn on our emulator (link on it you may find below, in "Device emulator" section).

![image](/images/user-guide/ui/widgets/trip-animation-widget/9.png)

### 部件已准备就绪

现在，我们可以实时查看设备在最后一分钟的运行情况。

我们还可以将时间轴游标加速多达1,5,10,25倍，以便我们可以更快地检查其路线。

不要忘记按“Start”按钮。

![image](/images/user-guide/ui/widgets/trip-animation-widget/3.gif)

## 定制

### 设置标签

现在，当我们了解了小部件可以提供的基本知识后，让我们开始编辑其设置，使其更具功能性和醒目性。首先，我们进入设置，在那里我们可以指定：

* 部件的标题，其样式

* 标题图标，图标颜色，像素大小（以px为单位）

* 标题工具提示显示/隐藏

* 启用/禁用阴影

* 启用/禁用小部件的全屏模式

* 更改部件样式

* 启用/禁用数据导出

* 背景颜色，文本颜色，填充，边距

* 指定手机设置


让我们看看它是如何工作的。

![image](/images/user-guide/ui/widgets/trip-animation-widget/4.gif)

### 高级标签

在设置选项卡中，我们可以为轨迹动画部件指定唯一的参数，以实现仅其可以提供的功能。我们有：

![image](/images/user-guide/ui/widgets/trip-animation-widget/15.png)

* 地图提供者

![image](/images/user-guide/ui/widgets/trip-animation-widget/16.png)

* 归一化数据步长（ms）

* 纬度和经度键名称-您可以根据要更新的小部件指定名称。它使用基于数据标签的数据。这样您就可以为经度键值指定标签“data-1”，并在我们将经度键名称编辑为“data-1”后从别名中获取经度。

![image](/images/user-guide/ui/widgets/trip-animation-widget/7.gif)

* 部件标签，或指定标签功能（您可以根据数据，dsData，dsIndex更改小部件标签中包含的数据）

![image](/images/user-guide/ui/widgets/trip-animation-widget/5.gif)
 
Label function:
```javascript
var speed = dsData['speed'];
var res;
if (speed > 55) {
    res = "Too Fast"
} else {
    res = "Everything is OK"
}
return res;

```

* 显示/隐藏工具提示，其颜色，其字体颜色，工具提示和工具提示文本的不透明度或使用工具提示功能（您可以根据数据，dsData，dsIndex更改工具提示中包含的数据）

![image](/images/user-guide/ui/widgets/trip-animation-widget/6.gif)

工具提示功能：
```javascript
var speed = dsData['speed'];
var res;
if (speed > 0) {
    res = "${entityName} <b>Speed:</b> " + String(speed)
} else {
    res = "On The stop"
}
return res;

```

* 路径颜色或指定路径颜色功能（您可以根据数据，dsData，dsIndex更改工具提示中包含的数据）-标记移动的颜色

![image](/images/user-guide/ui/widgets/trip-animation-widget/26.png)

路径颜色功能：
```javascript
var speed = dsData['speed'];
var res;
if (speed > 50) {
    res = "red"
} else {
    res = "green"
}
return res;
```

* 路径装饰器，其大小（以px为单位），结束/起点偏移量，装饰器中继器，笔触粗细和笔触不透明度

![image](/images/user-guide/ui/widgets/trip-animation-widget/27.png)

#### 多边形选项

什么是多边形？这是由有限数量的点描述的平面图形。我们使用的多边形基于所使用设备中指定的坐标，但是您可以使用任何其他实体。

您可以使用多边形选项标记您的资产和其他任何实体。对于多边形，我们可以指定下一个设置。多边形坐标的接收格式如下：

```
[[1CoordinateLatitude,1CoordinateLatitude],[2CoordinateLatitude,2CoordinateLatitude]...[nCoordinateLatitude,nCoordinateLatitude]]
``` 

其中**n**-描述多边形的坐标数。

* 显示/隐藏多边形

* 多边形工具提示文本或多边形工具提示功能（您可以根据数据，dsData，dsIndex更改包含在多边形工具提示中的数据）

* 多边形颜色，不透明度

* 多边形边框颜色，不透明度，权重

* 多边形颜色功能（您可以根据数据，dsData和dsIndex更改多边形颜色）

![image](/images/user-guide/ui/widgets/trip-animation-widget/28.png)

#### 点数选项

下一个选项是显示点选项。点是遥测数据更新，因此您可以检查每个点。对于这些点，可以使用下一个选项。

* 显示/隐藏点

* 点颜色

* 点大小px

* 将点用作锚点，将点用作锚点函数（您可以根据数据，dsData，dsIndex更改包含在多边形工具提示中的数据）

* 独立点工具提示

* 自动关闭点弹出

![image](/images/user-guide/ui/widgets/trip-animation-widget/2.gif)

#### 标记选项

除此之外，标记还有一些设置，您可以为其指定下一个设置：

* 默认标记的颜色

* 自定义标记图像

* 自定义标记图片大小px

* 设置标记的附加旋转角度

* 标记图像功能（您可以根据数据，dsData，dsIndex更改标记图像，标记图像颜色）

* 指定其他可能的标记图像，可以在标记图像功能中使用

![image](/images/user-guide/ui/widgets/trip-animation-widget/1.gif)

标记图像功能：
```javascript
var speed = dsData['speed'];
var res;
if (speed > 55) {
    res = images[0];
} else {
    res = images[1];
}
return res;
```

## 视频教程
 
我们也建议您阅读此视频教程。

  
<div id="video">  
    <div id="video_wrapper">
        <iframe src="https://www.youtube.com/embed/qWCmDjca-T8" frameborder="0" allowfullscreen></iframe>
    </div>
</div>


## 设备模拟器
 
[模拟器](/docs/user-guide/resources/timeseries-map-bus.js)

为了执行脚本，请使用命令行：
```bash
node timeseries-map-bus.js $ACCESSTOKEN
```
其中 **$ACCESSTOKEN** 是您的 **Device** **access token**。

**$ACCESSTOKEN** 位于 **Device details**。
![image](/images/user-guide/ui/widgets/trip-animation-widget/34.png)

模拟器与Node.js v8.10.0兼容
