---
layout: docwithnav
assignees:
- vparomskiy
title: 规则链
description: ThingsBoard 规则链管理

---

* TOC
{:toc}

## 规则链页面

<<<<<<< HEAD
规则链管理UI页面显示配置的租户规则链表。每个规则链都有一个单独的卡。您可以执行以下操作：
=======
Rule Chains Administration UI page displays a table of configured tenant rule chains.
You are able to do following operations:
>>>>>>> master

 - 导入或创建新规则链
 - 将规则链导出到JSON
 - 将规则链标记为**Root Rule Chain**
 - 删除规则链
 
有关更多详细信息，请参见[**规则引擎**](/docs/user-guide/rule-engine-2-0/re-getting-started/)文档。

![image](/images/user-guide/ui/rule-chain-page.png)

## 规则链导入/导出

#### 规则链出口

您可以将规则链导出为JSON格式，并将其导入到相同或其他ThingsBoard实例。

<<<<<<< HEAD
为了导出规则链，您应该导航到**规则链**页面，然后单击位于特定规则链卡上的导出按钮。
=======
In order to export rule chain, you should navigate to the **Rule Chains** page and click on the export button located on the particular rule chain row.
>>>>>>> master
 
![image](/images/user-guide/ui/export-rule-chain.png)

#### 规则导入

<<<<<<< HEAD
要导入规则链你应该导航到**规则链**页面，然后单击屏幕右下角的大“+”按钮然后单击导入按钮。
=======
Similar, to import the rule chain you should navigate to the **Rule Chains** page and click on the "+" button located in the top-right corner of the **Rule chains** table and then choose "Import rule chain" option. 
>>>>>>> master

![image](/images/user-guide/ui/rule-import.png)

**注意1:** 所有导入的规则链不是**Root Rule Chains**。
 
**注意2:** 如果导入的规则链包含对其他规则链的引用（通过“规则链”节点），则在保存规则链之前，您将需要更新这些引用。

#### 故障排除

导入规则时可能出现的问题：

 - 保存更改之前应更新通过**Rule Chain**节点对其他规则链的引用。
