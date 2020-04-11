---
layout: docwithnav
assignees:
- ashvayka
title: 审计日志
description: 管理IoT设备审计日志

---

ThingsBoard提供了跟踪用户操作以保留审计日志的功能。可以记录与主要实体相关的用户操作：assets, devices, dashboard, rules等。 

### 用户界面

租户管理员能够查看属于相应租户帐户的审计日志。管理员能够设置日期范围，并对获取的实体执行全文搜索。

![image](/images/user-guide/ui/audit-log.png)

"详细"按钮可以查看记录的详细信息

![image](/images/user-guide/ui/audit-log-details.png)

### REST API

可以通过REST API获取审核日志。有几种API调用，它们允许获取与指定用户，实体，客户相关的实体，或使用页面链接获取所有记录。  

### 常规配置

系统管理员可以使用[thingsboard.yml](/docs/user-guide/install/config/).配置审核日志级别。您可以在下面找到示例配置:

```yaml
# Audit log parameters
audit_log:
  # Enable/disable audit log functionality.
  enabled: "${AUDIT_LOG_ENABLED:true}"
  # Specify partitioning size for audit log by tenant id storage. Example MINUTES, HOURS, DAYS, MONTHS
  by_tenant_partitioning: "${AUDIT_LOG_BY_TENANT_PARTITIONING:MONTHS}"
  # Number of days as history period if startTime and endTime are not specified
  default_query_period: "${AUDIT_LOG_DEFAULT_QUERY_PERIOD:30}"
  # Logging levels per each entity type.
  # Allowed values: OFF (disable), W (log write operations), RW (log read and write operations)
  logging_level:
    mask:
      "device": "${AUDIT_LOG_MASK_DEVICE:W}"
      "asset": "${AUDIT_LOG_MASK_ASSET:W}"
      "dashboard": "${AUDIT_LOG_MASK_DASHBOARD:OFF}"
      "customer": "${AUDIT_LOG_MASK_CUSTOMER:W}"
      "user": "${AUDIT_LOG_MASK_USER:RW}"
      "rule": "${AUDIT_LOG_MASK_RULE:RW}"
      "plugin": "${AUDIT_LOG_MASK_PLUGIN:RW}"
```

此配置示例禁用与仪表板相关的任何操作的日志记录，并为用户和规则进行日志读取操作。对于所有其他实体，ThingsBoard将仅记录写级别的操作。

我们建议根据要记录的设备数和用户操作来修改“ by_tenant_partitioning”参数。您计划记录的动作越多，所需的分区就越精确。每个分区的大约记录量不应超过500000条记录。

### 外部日志接收配置

系统管理员可以配置与外部系统的连接。此连接将用于推送审核日志。内联文档对配置参数进行了详细记录。

```yaml
  sink:
    # Type of external sink. possible options: none, elasticsearch
    type: "${AUDIT_LOG_SINK_TYPE:none}"
    # Name of the index where audit logs stored
    # Index name could contain next placeholders (not mandatory):
    # @{TENANT} - substituted by tenant ID
    # @{DATE} - substituted by current date in format provided in audit_log.sink.date_format
    index_pattern: "${AUDIT_LOG_SINK_INDEX_PATTERN:@{TENANT}_AUDIT_LOG_@{DATE}}"
    # Date format. Details of the pattern could be found here:
    # https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html
    date_format: "${AUDIT_LOG_SINK_DATE_FORMAT:YYYY.MM.DD}"
    scheme_name: "${AUDIT_LOG_SINK_SCHEME_NAME:http}" # http or https
    host: "${AUDIT_LOG_SINK_HOST:localhost}"
    port: "${AUDIT_LOG_SINK_POST:9200}"
    user_name: "${AUDIT_LOG_SINK_USER_NAME:}"
    password: "${AUDIT_LOG_SINK_PASSWORD:}"      
```

## 下一步

{% assign currentGuide = "AdvancedFeatures" %}{% include templates/guides-banner.md %}
  