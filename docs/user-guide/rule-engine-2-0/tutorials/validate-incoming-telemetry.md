---
layout: docwithnav
title: 验证传入遥测
description: 验证传入遥测

---

* TOC
{:toc}

## 用例

假设您的设备正在使用DHT22传感器来收集温度读数并将其推送到ThingsBoard。

DHT22传感器适用于-40至80°C的温度读数。

在本教程中，我们将配置ThingsBoard规则引擎以存储-40至80°C范围内的所有温度，并将丢弃所有其他读数。

尽管这种情况是虚构的，但您将学习如何定义JS函数来验证传入的数据并在实际应用程序中使用此知识。

## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。

## 步骤1：添加温度验证节点

我们将修改默认规则链，并使用温度验证脚本添加[**filter**](/docs/user-guide/rule-engine-2-0/filter-nodes/#script-filter-node)规则节点。

我们将这个规则节点放置在默认的"message type switch"和"save timeseries"规则节点之间。

请注意，我们也从根规则链中删除了不相关的规则节点。

![image](/images/user-guide/rule-engine-2-0/tutorials/validation/rule-chain.png)

假设到达系统的数据可能具有"temperature"字段，也可能没有。

我们将所有没有"temperature"字段的数据视为有效。为此，我们将使用以下功能

```javascript
return typeof msg.temperature === 'undefined' || (msg.temperature >= -40 && msg.temperature <= 80);
```

## 步骤2：脚本调试验证

我们使用内置的"Test filter function"按钮来检查脚本是否正确

![image](/images/user-guide/rule-engine-2-0/tutorials/validation/node-config.png)

![image](/images/user-guide/rule-engine-2-0/tutorials/validation/test-function.png)

当温度未设置或超过指定阈值时，您可以查看更多其他情况。

## TL;DR

从本教程中下载并导入带有规则链的附件json[**文件**](/docs/user-guide/resources/validation-rule-chain.json)不要忘记将新规则链标记为"root"。

![image](/images/user-guide/rule-engine-2-0/tutorials/make-root.png)

 
## 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}






