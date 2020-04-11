---
layout: docwithnav
title: API & Rate Limits
description: Configuring API Limits

--- 

API和速率限制功能可通过限制单个时间单位（分钟，小时等）中来自单个主机/设备/租户的请求数量来控制API使用。

**默认情况**下API和费率限制为**禁用状态**。系统管理员可以使用[thingsboard.yml](/docs/user-guide/install/config/)配置速率限制。

#### REST API限制

各种UI组件都使用REST API调用，并且可能使用一些代表客户用户或租户用户启动的自动脚本。限制租户或客户的API调用数量至关重要，以避免由于自定义窗口小部件或脚本中的错误而导致服务器超载。

所述rest.limits.tenant.enabled参数或TB_SERVER_REST_LIMITS_TENANT_ENABLED环境属性启用/禁用租户级限制。

该rest.limits.tenant.configuration参数或TB_SERVER_REST_LIMITS_TENANT_CONFIGURATION的REST API调用的环境性能配置最高金额。例如，值“ 100：1,2000：60”表示每秒不超过100个请求，每分钟不超过2000个请求。

```yaml
server:
  ...
  rest:
    limits:
      tenant:
        enabled: "${TB_SERVER_REST_LIMITS_TENANT_ENABLED:false}"
        configuration: "${TB_SERVER_REST_LIMITS_TENANT_CONFIGURATION:100:1,2000:60}"
      customer:
        enabled: "${TB_SERVER_REST_LIMITS_CUSTOMER_ENABLED:false}"
        configuration: "${TB_SERVER_REST_LIMITS_CUSTOMER_CONFIGURATION:50:1,1000:60}"
```

### Websocket限制

Websocket用于将有关新遥测值的实时通知从设备传递到仪表板。

所述ws.send_timeout参数或TB_SERVER_WS_SEND_TIMEOUT环境属性控制的最大时间为成功的WebSocket消息递送到客户端。如果客户端太慢，则会话将关闭。

所述ws.limits.max_queue_per_ws_session参数或TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_QUEUE_PER_WS_SESSION 环境属性控制正在等待递送到客户端最大消息。如果客户端太慢，则会话将关闭。

该ws.limits.max_sessions_per_ *参数或TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SESSIONS_PER_ *环境属性控制每特定实体的活动连接的最大数量：租户，客户，公众或普通用户。如果每个特定标准的会话过多，则将断开新连接。

该ws.limits.max_subscriptions_per_ *参数或TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SUBSCRIPTIONS_PER_ *租户，客户，公众或普通用户：每特定实体的所有会话中的环境属性控制活动订阅的最大数量。如果每个特定标准的订阅过多，则将不接受新的订阅。

所述ws.limits.max_updates_per_session参数或TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_UPDATES_PER_SESSION 消息的环境属性配置最大金额从服务器为每个会话发送到客户端。例如，值“ 300：1,3000：60”表示每秒不超过300次更新，每分钟不超过3000次更新。

您可以在下面找到示例配置：

```yaml
server:
  ...
  ws:
    send_timeout: "${TB_SERVER_WS_SEND_TIMEOUT:5000}"
    limits:
      # Limit the amount of sessions and subscriptions available on each server. Put values to zero to disable particular limitation
      max_sessions_per_tenant: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SESSIONS_PER_TENANT:0}"
      max_sessions_per_customer: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SESSIONS_PER_CUSTOMER:0}"
      max_sessions_per_regular_user: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SESSIONS_PER_REGULAR_USER:0}"
      max_sessions_per_public_user: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SESSIONS_PER_PUBLIC_USER:0}"
      max_queue_per_ws_session: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_QUEUE_PER_WS_SESSION:500}"
      max_subscriptions_per_tenant: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SUBSCRIPTIONS_PER_TENANT:0}"
      max_subscriptions_per_customer: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SUBSCRIPTIONS_PER_CUSTOMER:0}"
      max_subscriptions_per_regular_user: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SUBSCRIPTIONS_PER_REGULAR_USER:0}"
      max_subscriptions_per_public_user: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_SUBSCRIPTIONS_PER_PUBLIC_USER:0}"
      max_updates_per_session: "${TB_SERVER_WS_TENANT_RATE_LIMITS_MAX_UPDATES_PER_SESSION:300:1,3000:60}"
```

### 数据库速率限制

尽管我们通过REST API调用的数量来限制用户，但是某些调用可能会产生不止一个数据库查询。同样，规则链也可能在消息处理期间引起很多查询。单个遥测上传还导致数据库查询将数据写入DB。

您可以指定Cassandra DB的限制，也可以指定参数以打印统计信息。例如：

```log
2018-11-29 10:51:25,020 [SockJS-1] INFO  o.t.s.d.n.CassandraBufferedRateExecutor - 
Permits queueSize [0] totalAdded [6395] totalLaunched [6395] totalReleased [6396] totalFailed [0] totalExpired [0] 
totalRejected [0] totalRateLimited [0] totalRateLimitedTenants [0] currBuffer [0]
```  

（可选）您可以指定打印违反阈值的租户名称。

该cassandra.query.tenant_rate_limits.configuration参数或CASSANDRA_QUERY_TENANT_RATE_LIMITS_CONFIGURATION每个租户查询的环境性能配置最高金额。例如，值“ 1000：1,30000：60”表示每秒不超过1000次更新，每分钟不超过30000次更新


```yaml
# Cassandra driver configuration parameters
cassandra:
  ...
  # Cassandra cluster connection query parameters
  query:
  ...
    rate_limit_print_interval_ms: "${CASSANDRA_QUERY_RATE_LIMIT_PRINT_MS:10000}"
    tenant_rate_limits:
      enabled: "${CASSANDRA_QUERY_TENANT_RATE_LIMITS_ENABLED:false}"
      configuration: "${CASSANDRA_QUERY_TENANT_RATE_LIMITS_CONFIGURATION:1000:1,30000:60}"
      print_tenant_names: "${CASSANDRA_QUERY_TENANT_RATE_LIMITS_PRINT_TENANT_NAMES:false}"
```

### 传输速度限制

能够限制单个设备或每个租户从所有设备接受的消息量非常重要。在将消息推送到规则引擎之前，此限制适用于传输级别。

所述transport.rate_limits.tenant.configuration参数或TB_TRANSPORT_RATE_LIMITS_TENANT从每个租户所有设备的消息的环境属性配置最大金额。例如，值“ 1000：1,20000：60”表示每秒不超过1000条消息，每分钟不超过20000条更新。

所述transport.rate_limits.tenant.configuration参数或TB_TRANSPORT_RATE_LIMITS_DEVICE从单个装置的消息的环境属性配置最大金额。例如，值“ 10：1,300：60”表示每秒不超过10条消息，每分钟不超过300条更新。


```yaml
transport:
...
  rate_limits:
    enabled: "${TB_TRANSPORT_RATE_LIMITS_ENABLED:false}"
    tenant: "${TB_TRANSPORT_RATE_LIMITS_TENANT:1000:1,20000:60}"
    device: "${TB_TRANSPORT_RATE_LIMITS_DEVICE:10:1,300:60}"
```

## 下一步

{% assign currentGuide = "AdvancedFeatures" %}{% include templates/guides-banner.md %}
