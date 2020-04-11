---
layout: docwithnav
title: 智慧农业和智慧农业解决方案
description: 使用ThingsBoard智慧农业
horizontaltoc: "true"

---

## 概述

ThingsBoard平台提供了现成的组件和API，可大大缩短产品上市时间，并减少您开发智能农业解决方案和项目的工作量。

该平台与设备无关，因此您可以从任何传感器，连接的设备或应用程序中馈送和分析数据。

利用以下优势，为您的智能农业项目节省多达90％的开发时间：

 - 从您的IoT设备和传感器中收集可靠且容错的数据，以监控设施状态，作物生长特征，湿度水平等；
 - 先进而灵活的数据可视化功能，可对未来农场进行实时和历史监控；
 - 可定制的最终用户仪表板，以共享服务器场监视结果；
 - 与第三方分析框架和解决方案集成，以进行高级分析和机器学习；
 - 根据分析结果远程配置IoT设备，从而在优化输入收益的同时保留资源。

该平台提供了可用于生产的服务器基础架构，以连接您的IoT设备，存储和分析收集的IoT数据，优化输入和资源的回报。

## 智慧农业仪表板

实时演示服务器上托管的以下交互式仪表板代表了智能农业IoT数据可视化，该可视化可能已嵌入到IoT农业项目或未来的农场解决方案中。请参阅下面的仪表板说明。

<iframe class="demoDashboardFrame" src="https://demo.thingsboard.io/dashboards/198c2b60-0edc-11e7-942c-bb0136cc33d0?publicId=963ab470-34c9-11e7-a7ce-bb0136cc33d0&source=docs" frameborder="0" width="100%"></iframe>
<div class="center" style="margin-bottom: 20px;">
    <a target="_blank" style="padding: 0 40px;" href="https://demo.thingsboard.io/dashboards/1f9828d0-058e-11e7-87f7-bb0136cc33d0?publicId=963ab470-34c9-11e7-a7ce-bb0136cc33d0&source=realtimeIotDashboards" class="button">在线实例</a>
</div>

随附的仪表板演示了使用ThingsBoard MQTT API收集的来自IoT传感器的实时数据。数据存储在我们的演示服务器上的Cassandra DB中。

我们要强调以下功能：

 - 使用网络套接字的低延迟更新；
 - 通过用鼠标选择时间范围来放大图表的能力；
 - 高级工具提示和图例；
 - 右上角的仪表板工具栏可启用全局时间选择器并在仪表板之间切换。

## 智慧农业解决方案概述
 
下图确定了典型的智能农业解决方案的数据流和集成点，该解决方案使用ThingsBoard平台从IoT传感器收集和分析智能农业数据。

![智慧农业解决方案图](/images/iot-use-cases/smart-farming.svg)

您可能会注意到IoT传感器有很多连接选项：直接连接到云或通过IoT网关。
平台支持行业标准的加密算法（SSL）和设备凭据类型（X.509证书和访问令牌）。
收集的数据存储在Cassandra中-容错且可靠的NoSQL数据库。
ThingsBoard Rule Engine允许使用Kafka或其他消息总线将传入的数据转发到各种分析系统，例如Apache Spark或Hadoop。


## 学习更多

<a style="margin: 10px;" href="/docs/getting-started-guides/helloworld/" class="button">入门</a>
<a style="margin: 10px;" href="/industries/smart-buildings/" class="button">客户反馈</a>
<a style="margin: 10px;" href="/docs/#platform-features" class="button">平台功能</a>
<a style="margin: 10px;" href="/docs/reference/" class="button">架构</a>
<a style="margin: 10px;" href="/docs/contact-us/" class="button">联系我们</a>
