---
layout: docwithnav
title: 物联网智能物联网解决方案和物联网智能仪表数据可视化
description: 物联网智能物联网解决方案和物联网智能仪表数据可视化
horizontaltoc: "true"

---

## 物联网和智能电表

传统上，智能电表是电网基础设施的一部分，是一种电子设备，可以远程监控和记录能耗。但是，在物联网和物联网平台时代，独立的智能电表被更高级和多功能的智能电表解决方案所取代。这些解决方案提供了广泛的远程监视和警报功能，并提供了强大的数据分析工具，以帮助公司和个人用户优化能源，水，天然气或燃料的消耗。

对于实施智能电表的公司而言，典型的挑战是如何将其集成到基础架构中，并设置量身定制的智能电表用例。实现这些目标的最佳方法是使用IoT平台，该平台提供开箱即用的解决方案和智能计量的模板，例如ThingsBoard。企业级物联网平台的最大优势之一是其数据处理功能。您不仅可以集中地从各种智能电表收集数据，还可以设置自定义可视化仪表板，配置用户警报和通知，并将收集的数据馈送到其他应用程序或数据存储中。

另一个关键优势是智能计量实施的成本。使用IoT平台，您可以立即拥有所有必要的功能，而专注于构建特定的智能电表用例，从而节省时间并避免与内部IoT开发相关的风险。

### ThingsBoard智能计量解决方案

<table style="border: none; width: initial;">
<tbody>
    <tr>
        <td><i class="fa fa-cloud-upload" style="font-size: 48px; color: #008b8b;" aria-hidden="true"></i></td>
        <td style="font-size: 20px;">使用不同的<a href="/docs/getting-started-guides/connectivity/">连接性方法</a>从智能电表中收集数据</td>
    </tr>    
    <tr>
        <td><i class="fa fa-dashboard" style="font-size: 48px; color: #008b8b;" aria-hidden="true"></i></td>
        <td style="font-size: 20px;"><a href="/docs/user-guide/visualization/">可视化</a>上收集的数据<a href="/docs/iot-video-tutorials/#visualization">自定义信息中心</a></td>
    </tr>    
    <tr>
        <td><i class="fa fa-line-chart" style="font-size: 48px; color: #008b8b;" aria-hidden="true"></i></td>
        <td style="font-size: 20px;"><a href="/docs/user-guide/rule-engine-2-0/re-getting-started/#typical-use-cases">
分析传入的智能电表数据以得出可行的见解
        </a></td>
    </tr>    
    <tr>
        <td><i class="fa fa-database" style="font-size: 48px; color: #008b8b;" aria-hidden="true"></i></td>
        <td style="font-size: 20px;">存储<a href="/docs/user-guide/reporting/">报告</a>和历史分析</td>
    </tr>    
    <tr>
        <td><i class="fa fa-money" style="font-size: 48px; color: #008b8b;" aria-hidden="true"></i></td>
        <td style="font-size: 20px;">将已处理的智能计量数据馈送到<a href="/docs/user-guide/rule-engine-2-0/external-nodes/">第三方应用程序</a>进行结算和计费</td>
    </tr>    
</tbody>
</table>


## 使用ThingsBoard构建端到端智能计量解决方案

ThingsBoard IoT平台提供了现成的组件和API，可显着降低创建智能计量解决方案所需的工作量，从而大大缩短了解决方案的上市时间，可靠性和竞争力。据我们估计，使用ThingsBoard的以下功能和优点，公司可以节省多达90％的产品开发时间：

- 为您的智能水表，能量监测器，智能电表等提供可靠且容错的数据收集；
- 先进的，可定制的[数据可视化](/docs/user-guide/visualization/)，用于实时和历史智能计量监控；
- [Alarm widgets](/docs/user-guide/ui/widget-library/#alarm-widgets)可将任何重大事件或异常消耗水平立即通知用户和/或操作员；
- 设备管理，可让您按特定属性在[groups](/docs/user-guide/groups/)中组织端点，简化不同类型[entities](/docs/user-guide/entities-and-relations/)和端点组，并根据您的自定义组启用更灵活的数据分析；
- 可定制的 [end-user dashboards](/docs/user-guide/ui/dashboards/) （具有向下钻取功能），以分析和共享智能计量监视的结果；
- [第三方分析框架和解决方案](/docs/samples/analytics/spark-integration-with-thingsboard/) 集成，以进行智能计量数据和报告的高级处理；
- 通过使用[ThingsBoard API](/docs/api/)来控制和管理智能电表，从而实现智能电表管理。

ThingsBoard IoT平台提供了可用于生产的服务器基础架构，以连接您的智能电表设备，收集，存储和分析智能电表数据，并与您的客户和最终用户共享分析结果。

## 智能计量仪表板

实时演示服务器上托管的以下交互式仪表板代表了可能嵌入在IoT项目或解决方案中的智能计量IoT数据可视化。请参阅下面的仪表板说明。

<iframe class="demoDashboardFrame" src="https://demo.ThingsBoard.io/dashboards/3a1026e0-83f6-11e7-b56d-c7f326cba909?publicId=322a2330-7c36-11e7-835d-c7f326cba909" frameborder="0" width="100%"></iframe>
<div class="center" style="margin-bottom: 20px;">
    <a target="_blank" style="padding: 0 40px;" href="https://demo.ThingsBoard.io/dashboards/3a1026e0-83f6-11e7-b56d-c7f326cba909?publicId=322a2330-7c36-11e7-835d-c7f326cba909" class="button">在线实例</a>
</div>

随附的仪表板演示了使用[ThingsBoard MQTT API](/docs/reference/mqtt-api/)收集的来自智能电表的实时数据。数据存储在我们的演示服务器上的Cassandra DB中。

我们想强调以下功能：

 - 使用网络套接字的低延迟更新；
 - 通过用鼠标选择时间范围来放大图表的能力；
 - 高级工具提示和图例；
 - 右上角的仪表板工具栏可启用全局时间选择器并在仪表板之间切换。

## 智能计量解决方案概述
 
下图确定了典型的智能计量解决方案的数据流和集成点，该解决方案使用ThingsBoard平台收集和分析来自智能电表的监控数据。

<br/>

![Smart metering solution diagram](/images/iot-use-cases/smart-energy-monitoring.svg)

您可能会注意到，智能电表有很多连接选项：通过直接连接到云和通过[平台集成](/docs/user-guide/integrations/)均可实现。 
ThingsBoard平台支持行业标准的加密算法 [(SSL)](/docs/user-guide/mqtt-over-ssl/)和[设备凭证](/docs/user-guide/device-credentials/)类型（X. 509个证书和访问令牌）。
收集的数据存储在Cassandra中-一个流行的NoSQL数据库，该数据库因其容错性和可靠性而广为人知。

ThingsBoard Rule Engine支持使用Kafka或其他消息总线将传入的数据转发到各种分析系统，例如Apache Spark或Hadoop。

## 学习更多

<a style="margin: 10px;" href="/docs/getting-started-guides/helloworld/" class="button">入门</a>
<a style="margin: 10px;" href="/industries/smart-buildings/" class="button">客户反馈</a>
<a style="margin: 10px;" href="/docs/#platform-features" class="button">平台功能</a>
<a style="margin: 10px;" href="/docs/reference/" class="button">架构</a>
<a style="margin: 10px;" href="/docs/contact-us/" class="button">联系我们</a>
