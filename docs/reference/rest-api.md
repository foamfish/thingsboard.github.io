---
layout: docwithnav
assignees:
- ashvayka
title: REST API
description: 支持的REST API引用，用于物联网项目的服务器端集成

---

* TOC
{:toc}

## Swagger UI

使用 **[Swagger UI](https://demo.thingsboard.io/swagger-ui.html)** 调用ThingsBoard REST API。
使用 **[Swagger UI](https://cloud.thingsboard.io/swagger-ui.html)** 调用ThingsBoard专业版REST API服务。

只要安装了ThingsBoard服务，就可以使用以下URL打开UI：
    
``` 
http://YOUR_HOST:PORT/swagger-ui.html
```

## REST API 认证

ThingsBoard使用JWT进行请求身份验证。
您将需要使用Swagger UI右上角的”Authorization“按钮填充“X-Authorization”标题。

 ![image](/images/reference/swagger-ui.png)

获得JWT令牌，你需要执行以下请求：

本地安装：
 
 - 替换 **$THINGSBOARD_URL** 为 **127.0.0.1:8080**

在线演示:
 
 - 替换 **$THINGSBOARD_URL** 为 **demo.thingsboard.io**
 - 替换演示帐号 为 **tenant@thingsboard.org** 用户名
 - 替换演示密码 为 **tenant** 密码
获取更多详细信息，请参见[在线演示](/docs/user-guide/live-demo/)页面。

{% capture tabspec %}token
A,get-token.sh,shell,resources/get-token.sh,/docs/reference/resources/get-token.sh
B,response.json,json,resources/get-token-response.json,/docs/reference/resources/get-token-response.json{% endcapture %}
{% include tabs.html %}

<<<<<<< HEAD
 - 现在你应该将“X-Authorization”设置为“Bearer $YOUR_JWT_TOKEN”
=======
 - Now, you should set  'X-Authorization' to "Bearer $YOUR_JWT_TOKEN"
 
 
## Java REST API Client

ThingsBoard team provides client library written in Java to simplify consumption of the REST API.
Please see Java REST API Client [documentation page](/docs/reference/rest-client/) for more details.
>>>>>>> master
