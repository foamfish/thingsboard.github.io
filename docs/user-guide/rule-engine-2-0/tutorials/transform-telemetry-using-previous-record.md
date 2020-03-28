---
layout: docwithnav
title: 使用以前的记录进行遥测
description: 使用以前的记录进行遥测

---

* TOC
{:toc}

## 用例

假设您的设备报告的绝对"counter"与水消耗相对应。

但是，您不希望可视化“绝对”值，而是可视化“增量”值，例如在最后一天，一周，一个月内消耗了多少水。

在本教程中，我们将基于当前和先前的读数计算计数器读数的"delta"。

假设先前报告的counter值为90，我们将转换输入遥测：

```json
{
  "counter": 100
}
```

到

```json
{
  "counter": 100,
  "delta": 10
}
```

## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。
  * [变换传入遥测](/docs/user-guide/rule-engine-2-0/tutorials/transform-incoming-telemetry/) 。

## 步骤1: 添加enrichment节点

我们将修改默认规则链，通过[**enrichment**](/docs/user-guide/rule-engine-2-0/enrichment-nodes/#originator-attributes)规则节点从以下位置获取先前的遥测值：数据库并将其放入消息metadata。

![image](/images/user-guide/rule-engine-2-0/tutorials/previous/rule-chain.png)

我们将使用以下节点配置：

![image](/images/user-guide/rule-engine-2-0/tutorials/previous/node-config-step-1.png)

请注意，如果缺少"counter"值，则规则节点将返回失败。

我们将在下一步中设置默认的上一个计数器，以防止这种故障。

## 步骤2：默认的上一个计数器节点

这个[**transformation**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#script-transformation-node)节点将默认计数器设置为来自传入消息的元数据。下一步将用于将默认"delta"值设置为0。

![image](/images/user-guide/rule-engine-2-0/tutorials/previous/node-config-step-2.png)

## 步骤3：Delta transformation节点

[**transformation**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#script-transformation-node)节点将从消息中根据metadata中的先前计数器值和当前值来计算增量。

![image](/images/user-guide/rule-engine-2-0/tutorials/previous/node-config-step-3.png)

## 步骤4：设置信息中心以查看数据

我们添加了简单的卡片小部件以显示规则链生成的最新值

![image](/images/user-guide/rule-engine-2-0/tutorials/previous/dashboard.png)

## TL;DR

从本教程中下载并导入带有规则链的附件json[**文件**](/docs/user-guide/resources/previous-telemetry-rule-chain.json)。不要忘记将新规则链标记为“根”。

![image](/images/user-guide/rule-engine-2-0/tutorials/make-root.png)

从本教程中下载并导入带有仪表板的附件json[**文件**](/docs/user-guide/resources/previous-telemetry-dashboard.json) 。


## 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}

