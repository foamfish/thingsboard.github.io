---
layout: docwithnav
assignees:
- ashvayka
title: 租户
description: ThingsBoard租户管理

---

* TOC
{:toc}

ThingsBoard开箱即用的[Multitenancy](https://en.wikipedia.org/wiki/Multitenancy)。你可以将ThingsBoard租户视为一个单独的业务实体：拥有或生产设备的个人或组织。

**系统管理员**可以创建租户实体

![image](/images/user-guide/ui/tenants.png)

系统管理员还可以为每个租户创建具有**租户管理员**角色的多个[用户](/docs/user-guide/ui/users)，通过在租户详细信息中按“管理租户管理员”按钮。
 
![image](/images/user-guide/ui/manage-tenant-admins.png) 
 
租户管理员可以执行以下操作:
 
 - 管理[设备](/docs/user-guide/ui/devices).
 - 管理[资产](/docs/user-guide/ui/assets).
 - 管理[客户](/docs/user-guide/ui/customers).
 - 管理[仪表板](/docs/user-guide/ui/dashboards).
 - 配置[规则引擎](/docs/user-guide/rule-engine-2-0/re-getting-started/)
 - [部件库](/docs/user-guide/ui/widget-library).
 
 以上所有操作也可以使用[REST API](/docs/reference/rest-api/)完成相同操作。