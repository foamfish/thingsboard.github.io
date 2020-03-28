---
layout: docwithnav
title: 变换传入遥测
description: 变换传入遥测

---

* TOC
{:toc}

## 用例

假设您的设备正在使用自定义传感器来收集温度读数并将其推送到ThingsBoard。

该传感器以°F为单位收集温度读数，您希望在将其存储到数据库和可视化之前将其转换为°C。

在本教程中，我们将配置ThingsBoard Rule Engine来根据以下公式修改温度读数：

```code
[°C] = ([°F] - 32) × 5/9.
```

## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门指南](/docs/getting-started-guides/helloworld/)。
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/)。

## 步骤1：添加温度transformation节点

我们将修改默认规则链，并使用温度转换脚本添加[**transformation**](/docs/user-guide/rule-engine-2-0/transformation-nodes/#script-transformation-node)规则节点。

我们将这个规则节点放置在默认的"message type switch"和"save timeseries"规则节点之间。

请注意我们也从根规则链中删除了不相关的规则节点。

![image](/images/user-guide/rule-engine-2-0/tutorials/transformation/rule-chain.png)

生成模拟数据

```javascript
function precisionRound(number, precision) {
  var factor = Math.pow(10, precision);
  return Math.round(number * factor) / factor;
}

if (typeof msg.temperature !== 'undefined'){
    msg.temperature = precisionRound((msg.temperature -32) * 5 / 9, 2);
}

return {msg: msg, metadata: metadata, msgType: msgType};
```

## 步骤2：验证脚本调试

我们使用内置的"Test transformer function"按钮来检查脚本是否正确

![image](/images/user-guide/rule-engine-2-0/tutorials/transformation/node-config.png)

![image](/images/user-guide/rule-engine-2-0/tutorials/transformation/test-function.png)

## TL;DR

从本教程中下载并导入带有规则链的附件json[**文件**](/docs/user-guide/resources/transformation-rule-chain.json)不要忘记将新规则链标记为"root"规则链。

![image](/images/user-guide/rule-engine-2-0/tutorials/make-root.png)

 

## 下一步

{% assign currentGuide = "DataProcessing" %}{% include templates/guides-banner.md %}





