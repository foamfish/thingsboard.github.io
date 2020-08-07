---
layout: docwithnav
title: 车队跟踪和车队管理
description: 使用ThingsBoard IoT Platform进行车队跟踪和车队管理
horizontaltoc: "true"

---

## 概述

ThingsBoard平台提供了现成的组件和API，可大大减少产品上市时间和您的开发工作
车队跟踪解决方案和GPS车辆跟踪系统。
利用以下优势，为车队跟踪项目节省多达90％的开发时间：

 - 为您的车辆跟踪器和其他嵌入式传感器提供可靠且容错的数据收集；
 - 用于实时和历史车辆数据的先进且灵活的IoT数据可视化；
 - 可定制的最终用户仪表板，以与最终用户和客户共享来自车辆跟踪系统的数据；
 - 与第三方分析框架和解决方案集成，以获取高级见解，并将这些见解导入到您的车辆跟踪系统中。

该平台提供了可用于生产的服务器基础架构，以连接您的智能汽车和车辆，收集，存储和分析各种车辆数据，并与您的客户和最终用户共享分析结果。

## 车队追踪仪表板

实时演示服务器上托管的以下交互式仪表板代表了可能嵌入在IoT车队跟踪项目中的车辆路线和状态指示器可视化。请参阅下面的仪表板说明。

<iframe class="demoDashboardFrame" src="https://demo.thingsboard.io/dashboard/83cbe060-0edc-11e7-942c-bb0136cc33d0?publicId=963ab470-34c9-11e7-a7ce-bb0136cc33d0&source=docs" frameborder="0" width="100%"></iframe>
<div class="center" style="margin-bottom: 20px;">
<<<<<<< HEAD
    <a target="_blank" style="padding: 0 40px;" href="https://demo.thingsboard.io/dashboards/3d0bf910-ee09-11e6-b619-bb0136cc33d0?publicId=963ab470-34c9-11e7-a7ce-bb0136cc33d0&source=realtimeIotDashboards" class="button">在线实例</a>
=======
    <a target="_blank" style="padding: 0 40px;" href="https://demo.thingsboard.io/dashboard/3d0bf910-ee09-11e6-b619-bb0136cc33d0?publicId=963ab470-34c9-11e7-a7ce-bb0136cc33d0&source=realtimeIotDashboards" class="button">Live demo</a>
>>>>>>> master
</div>

随附的仪表板演示了使用ThingsBoard MQTT API收集的来自车辆传感器的实时数据。数据存储在我们的演示服务器上的Cassandra DB中。

我们要强调以下功能：

 - 使用网络套接字的低延迟更新；
 - 通过用鼠标选择时间范围来放大图表的能力；
 - 高级工具提示和图例；
 - 右上角的仪表板工具栏可启用全局时间选择器并在仪表板之间切换。

## 车队跟踪解决方案概述
 
下图确定了使用ThingsBoard平台收集和分析车辆数据的典型车队跟踪解决方案的数据流和集成点。

![Fleet tracking solution diagram](/images/iot-use-cases/fleet-tracking.svg)

您可能会注意到，车辆传感器有很多连接选项：直接连接到云或通过IoT网关。
平台支持行业标准的加密算法（SSL）和设备凭据类型（X.509证书和访问令牌）。
收集的数据存储在Cassandra中-容错且可靠的NoSQL数据库。
ThingsBoard规则引擎使您可以使用Kafka或其他消息总线将传入的数据转发到各种分析系统，例如Apache Spark或Hadoop。

## 学习更多

<a style="margin: 10px;" href="/docs/getting-started-guides/helloworld/" class="button">入门</a>
<a style="margin: 10px;" href="/industries/smart-buildings/" class="button">客户反馈</a>
<a style="margin: 10px;" href="/docs/#platform-features" class="button">平台功能</a>
<a style="margin: 10px;" href="/docs/reference/" class="button">架构</a>
<a style="margin: 10px;" href="/docs/contact-us/" class="button">联系我们</a>
