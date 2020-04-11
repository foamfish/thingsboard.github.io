---
layout: docwithnav
assignees:
- ashvayka
title: IoT设备警报
description: 通过ThingsBoard中报警功能对IoT设备进行报警管理
---

* TOC
{:toc}

Thingsboard能够创建和管理应用中实体（devices, assets, customers）相关的alarm功能。

### Alarm生命周期

alarm在应用程中是一个具有有生命周期功能，可以清除和确认应用中的每一个alarm。默认情况下alarm是处于活动和待确认状态。

### Alarm的创建，类型和传播

alarm发起者应负责触发警报的相关实体。默认情况下，警报会传播到所有相关实体（仅父级关系）。通过创建时间，创建者和类型来标识alarm。同一类型和不能有两个活动的alarm。

### Alarm级别

alarm支持级别如下：危急（CRITICAL）, 重要（MAJOR）, 次要（MINOR）, 警告（WARNING,） 不确定（INDETERMINATE）

### Alarm更新

alarm实体可以通过外部应用程序或ThingsBoard规则进行更新。警报会同时跟踪清除和确认时间以及最新更改作为结束时间。

### Alarm REST AP

ThingsBoard提供REST API来管理和查询alarm。 有关更多详细信息，请参见演示环境 [Alarm REST API](https://demo.thingsboard.io/swagger-ui.html#/alarm-controller)和常规[REST API](/docs/reference/rest-api/)文档。

### Alarm规则
通过ThingsBoard[规则引擎](/docs/user-guide/rule-engine-2-0/re-getting-started/)可以创建，更新和清除警报。

你可以通过以下链接了解更多Alarms相关信息:

- [创建Alarms](/docs/user-guide/rule-engine-2-0/action-nodes/#create-alarm-node)。
- [清除Alarms](/docs/user-guide/rule-engine-2-0/action-nodes/#clear-alarm-node)。
- [使用Alarms](/docs/user-guide/rule-engine-2-0/tutorials/create-clear-alarms/)。

    
