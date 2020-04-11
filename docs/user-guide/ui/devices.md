---
layout: docwithnav
assignees:
- ashvayka
title: 设备
description: ThingsBoard IoT设备管理

---

ThingsBoard使用Web UI和[REST API](/docs/reference/rest-api/)支持以下设备管理功能。

* TOC
{:toc}

## 添加和删​​除设备

租户管理员可以注册新设备或从ThingsBoard删除它们。

![image](/images/user-guide/ui/devices.png)

## 管理设备凭证

租户管理员能够管理设备凭据。当前版本支持基于访问令牌和X.509证书的凭据。

![image](/images/user-guide/ui/manage-device-credentials.png)

## 获取设备ID
  
租户管理员和客户用户可以使用“复制设备ID”按钮将设备ID复制到剪贴板。

 ![image](/images/user-guide/ui/device-id.png)

## 将设备分配给客户

租户管理员可以将设备分配给某些[客户](/docs/user-guide/ui/customers/)。这将允许客户用户使用REST API或Web UI来获取设备数据。
 
 ![image](/images/user-guide/ui/assign-device-to-customer.png)

## 浏览设备属性

租户管理员和客户用户可以浏览设备[属性](/docs/user-guide/attributes)。

 ![image](/images/user-guide/ui/device-attributes.png)

## 浏览设备遥测

租户管理员和客户用户可以浏览设备[遥测数据](/docs/user-guide/telemetry)。

 ![image](/images/user-guide/ui/device-telemetry.png)

## 浏览设备警报

租户管理员和客户用户可以浏览设备[警报](/docs/user-guide/alarms)。

 ![image](/images/user-guide/ui/device-alarms.png)
 
## 浏览设备事件
  
租户管理员和客户用户可以使用“事件”选项卡浏览与特定设备有关的事件。生命周期事件和统计数据即将推出。

## 管理设备关系
 
租户管理员和客户用户可以管理设备[关系](/docs/user-guide/entities-and-relations)。

 ![image](/images/user-guide/ui/device-relations.png)
 