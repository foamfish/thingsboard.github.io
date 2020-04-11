---
layout: docwithnav
assignees:
- vsosliuk
title: 设备认证
description: ThingsBoard IoT设备认证。

---

设备凭证用于在设备上运行的应用程序连接到ThingsBoard服务器。

ThingsBoard旨在支持不同的设备凭据，目前有两种受支持的凭据类型：

 - [**访问令牌**](/docs/user-guide/access-token/) - 基于访问令牌身份验证模式可以在未加密或单向SSL模式下使用，适合于通用的设备认证。
   - **优点:** 易于配置和使用且占用带宽低和支持设备广泛。
   - **缺点:** 使用未加密码的网络易被拦截。
 - [**X.509凭据**](/docs/user-guide/certificates/) - X.509认证凭据是基于[PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure)和[TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) 标准的双向SSL模式认证。 
   - **优点:** 使用加密的凭据进行连接安全性较高。
   - **缺点:** 某些设备不支持耗电并消耗CPU资源较高。

需要将设备凭据提供给服务器上相应的设备实体，有如下方法可以做到这一点;

 - **自动**：使用ThingsBoard [REST API](/docs/reference/rest-api/) 应用场景在制造、QA或采购时使用。
 - **手动**：使用ThingsBoard [Web UI](/docs/user-guide/ui/devices/#manage-device-credentials) 应用场景在开发或系统管理时使用。


