---
layout: docwithnav
title: 智能能源监控，数据可视化和能效分析
description: 使用ThingsBoard IoT Platform进行智能能源监控和数据可视化
horizontaltoc: "true"

---

## 概述

ThingsBoard平台提供了现成的组件和API，可大大缩短产品上市时间，并减少您创建智能能源解决方案和能源监控系统的工作量。
利用以下优势，为您的智能能源解决方案节省多达90％的开发时间：

 - 为您的智能电表和电量监测器提供可靠且容错的数据收集；
 - 先进而灵活的数据可视化，用于实时和历史智能能源监控；
 - 可定制的最终用户仪表板，以分析和共享能效监测结果；
 - 与第三方分析框架和解决方案集成，以进行高级用电量监控；
 - 利用ThingsBoard API来控制和管理智能电表，从而实现能源管理。
 
该平台提供了可用于生产的服务器基础架构，以连接您的智能电表设备，收集，存储和分析能源监控数据，并与您的客户和最终用户共享分析结果。

## 智能能源仪表板

实时演示服务器上托管的以下交互式仪表板代表了可能嵌入在IoT项目或解决方案中的智能能源IoT数据可视化。请参阅下面的仪表板说明。

<iframe class="demoDashboardFrame" src="https://demo.thingsboard.io/dashboards/e5e72680-0eda-11e7-942c-bb0136cc33d0?publicId=963ab470-34c9-11e7-a7ce-bb0136cc33d0&source=docs" frameborder="0" width="100%"></iframe>
<div class="center" style="margin-bottom: 20px;">
    <a target="_blank" style="padding: 0 40px;" href="https://demo.thingsboard.io/dashboards/e8e409c0-f2b5-11e6-a6ee-bb0136cc33d0?publicId=963ab470-34c9-11e7-a7ce-bb0136cc33d0&source=realtimeIotDashboards" class="button">在线实例</a>
</div>

随附的仪表板演示了使用ThingsBoard MQTT API收集的来自智能电表的实时数据。数据存储在我们的演示服务器上的Cassandra DB中。

我们要强调以下功能：

 - 使用网络套接字的低延迟更新；
 - 通过用鼠标选择时间范围来放大图表的能力；
 - 高级工具提示和图例；
 - 右上角的仪表板工具栏可启用全局时间选择器并在仪表板之间切换。

## 智能能源解决方案概述
 
下图确定了典型的智能能源解决方案的数据流和集成点，该解决方案使用ThingsBoard平台收集和分析来自智能电表的能源监控数据。

![智能能源解决方案图](/images/iot-use-cases/smart-energy-monitoring.svg)

您可能会注意到，智能电表有很多连接选项：直接连接到云或通过IoT网关。
平台支持行业标准的加密算法（SSL）和设备凭据类型（X.509证书和访问令牌）。
收集的数据存储在Cassandra中-容错且可靠的NoSQL数据库。
ThingsBoard Rule Engine允许使用Kafka或其他消息总线将传入的数据转发到各种分析系统，例如Apache Spark或Hadoop。

## 学习更多

<a style="margin: 10px;" href="/docs/getting-started-guides/helloworld/" class="button">入门</a>
<a style="margin: 10px;" href="/industries/smart-buildings/" class="button">客户反馈</a>
<a style="margin: 10px;" href="/docs/#platform-features" class="button">平台功能</a>
<a style="margin: 10px;" href="/docs/reference/" class="button">架构</a>
<a style="margin: 10px;" href="/docs/contact-us/" class="button">联系我们</a>
