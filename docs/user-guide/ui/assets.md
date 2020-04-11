---
layout: docwithnav
assignees:
- ikulikov
title: 资产
description: Thingsboard IoT资产管理

---

Thingsboard支持以下使用Web UI和[REST API](/docs/reference/rest-api/)的资产管理功能。

* TOC
{:toc}

## 添加和删​​除资产

租户管理员可以注册新资产或将其从Thingsboard中删除。

![image](/images/user-guide/ui/assets.png)

## 获取资产ID
  
租户管理员和客户用户可以使用“复制资产ID”按钮将资产ID复制到剪贴板。

 ![image](/images/user-guide/ui/asset-id.png)

## 向客户分配资产

租户管理员可以将资产分配给某些[客户](/docs/user-guide/ui/customers/)。这将允许客户用户使用REST API或Web UI来获取资产数据。
 
 ![image](/images/user-guide/ui/assign-asset-to-customer.png)

## 管理资产属性

租户管理员和客户用户能够管理资产服务器端[属性](/docs/user-guide/attributes)。
 ![image](/images/user-guide/ui/asset-attributes.png)

## 浏览资产警报

租户管理员和客户用户可以浏览资产[警报](/docs/user-guide/alarms)。

 ![image](/images/user-guide/ui/asset-alarms.png)
 
## 浏览资产事件
  
租户管理员和客户用户可以使用“事件”选项卡浏览与特定资产相关的事件。生命周期事件和统计数据即将推出。

## 管理资产关系
 
租户管理员和客户用户可以管理资产[关系](/docs/user-guide/entities-and-relations)。

 ![image](/images/user-guide/ui/asset-relations.png)
 