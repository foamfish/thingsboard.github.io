---
layout: docwithnav
assignees:
- ashvayka
title: 数据可视化
description: 使有IoT仪表板、小部件和实时图表为项目提供数据可视化
---

* TOC
{:toc}

ThingsBoard允许用户自定义IoT仪表板进行数据可视化展示，同时在每个仪表板中包含许多小部件，使用这些小部件可以处理来不同IoT设备的数据可视化展示，你可以将不同的IoT仪表板分配给你项目中不同的客户。

例如:你可以根据用户注册的IoT设备数据为第一个客户创建一个仪表板，也可以将新设备分配给客户通过脚本修改仪表板，所有这些操作可以手动进行也可以通过RESTAPI自动执行。

点击以下链接了解详情:

 - [Getting started guide](/docs/getting-started-guides/helloworld/) - 将介绍创建仪表板的基本步骤。
 - [IoT Dashboards](/docs/user-guide/ui/dashboards/) - 包含有关基本IoT仪表板操作的教程。
 - [Samples](/docs/samples/) - 包含几个示例，其中包括客户端应用程序和相应的数据可视化。
 - [部件库](/docs/user-guide/ui/widget-library/) - 包含仪表板小部件捆绑包的概述:
   - **Digital**和**analog**仪表可实现最新的实时值可视化 
   - 高度可定制的条形图和折线图，用于可视化历史展示 
   - **Map部件** 使用Google或OpenStreet地图对IoT设备最新位置和移动路径进行展示。
   - **GPIO** 允许发送GPIO命令至设备进行控制。
   - **Card** 基于部件可以将IoT设备的静态数据或最新遥测数据通HTML标签进行展示