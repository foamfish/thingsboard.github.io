---
layout: docwithnav
title: 不同AWS实例上的ThingsBoard性能
description: 不同AWS实例上的ThingsBoard性能

---

* TOC
{:toc}

ThingsBoard开源IoT平台的关键功能之一是数据收集，这是一项至关重要的功能，必须在大量长时间运行的消息上传中可靠地工作。

在本文中，我们将在不同的AWS实例上执行ThingsBoard的长期数据收集测试。
我们将检查每个实例每秒可以处理多少消息，并将提供CPU和内存负载统计信息。

考虑到测试结果和您的项目要求，您将能够确定哪种类型的实例最适合您的项目。

## 数据流和测试工具

物联网设备通过MQTT或HTTP设备API连接到ThingsBoard服务器，并将示例测试数据（*长型单遥测*）发送到平台。
ThingsBoard服务器处理MQTT或HTTP消息，并将它们异步存储到Cassandra / PostgreSQL。

作为测试工具，我们使用了[Performance Test Project](https://github.com/thingsboard/performance-tests)的更新版本，该版本能够以非常高效的异步方式通过MQTT/HTTP Device API发送消息。

考虑到ThingsBoard平台的微服务架构并以准确的方式衡量性能，我们创建了一个附加的规则链节点，该规则链节点能够计算每1秒已收到该节点的消息数量（这是一个可配置的参数，更改）并将此值存储为租户级别上的遥测。

该附加的规则链节点位于根链的“保存遥测”节点之后，并计算性能测试期间ThingsBoard实例处理了多少消息。此数据以预定义键前缀的遥测方式存储在租户级别。

![image](/images/reference/performance-aws-instances/modified-rule-chain.png)

测试完成后，性能测试工具将从平台获取此遥测值，并在测试控制台中提供结果：

```bash
12:20:03.772 [main] INFO  o.t.t.s.stats.StatisticsCollector - ============ Node [692dc903cf52] AVG is 500.0 per 1 second ============
12:20:03.772 [main] INFO  o.t.t.s.stats.StatisticsCollector - ============ Total AVG is 500.0 per 1 second ============
```

可以在ThingsBoard仪表板上显示此遥测值以及常规遥测。

**注意：**如果集群中有多个ThingsBoard节点，则附加的规则链节点将在租户级别的不同遥测键下保存统计信息，但是结果中的Performance Test Tool将这些值聚合为一个结果。

## 如何重复测试

有关更多详细信息，请使用[Performance Test Project](https://github.com/thingsboard/performance-tests/) 的文档。

## 按AWS实例类型的性能

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟|消息上限|
| --- | --- | --- | --- | --- | --- | --- |
| [t2.micro](#t2micro) | 1 vCPUs for a 2h 24m burst, 1GB | PostgreSQL | MQTT | 500 | 1000 ms | **~450/sec** | 
| [t2.medium](#t2medium)  | 2 vCPUs for a 4h 48m burst, 4GB | PostgreSQL | MQTT | 900 | 1000 ms | **~780/sec** |
| [c5.large](#c5large)  | 2 vCPUs , 4GB | PostgreSQL | MQTT | 1100 | 1000 ms | **~1020/sec** |
| t2.xlarge | 4 vCPUs for a 5h 24m burst, 16GB | PostgreSQL | MQTT |  1800 | 1000 ms | **~1700/sec** |
| t2.xlarge | 4 vCPUs for a 5h 24m burst, 16GB | Cassandra | MQTT | 3000 | 1000 ms | **~3000/sec** |
| [m5.xlarge](#m5xlarge)  | 4 vCPUs, 16GB, 150GB SSD mounted | Cassandra | MQTT | 3500 | 1000 ms | **~3500/sec** |
| [m5.xlarge](#m5xlarge)  | 4 vCPUs, 16GB, 150GB SSD mounted | Cassandra | HTTP | 2000 | 1000 ms | **~950/sec** |

## t2.micro

**性能结果**

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟|消息上限|
| --- | --- | --- | --- | --- | --- | --- |
| t2.micro | 1 vCPUs for a 2h 24m burst, 1GB | PostgreSQL | MQTT | 500 | 1000 ms | **~450/sec** |

测试运行配置(有关更多详细信息，请参见[性能测试项目](https://github.com/thingsboard/performance-tests/#running)):

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=500

PUBLISH_COUNT=300
PUBLISH_PAUSE=1000
...
```

### 测试运行#1

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）测试运行小时数|
| --- | --- | --- | --- | --- | --- | --- |
| t2.micro | 1 vCPUs for a 2h 24m burst, 1GB | PostgreSQL | MQTT | 50 | 1000 ms | 10 | 

**测试配置**

测试运行配置(有关更多详细信息，请参见[性能测试项目](https://github.com/thingsboard/performance-tests/#running)):

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=50

PUBLISH_COUNT=36000
PUBLISH_PAUSE=1000
...
```

**CPU/Memory Load**

| 属性 | 平均 | 最小 | 最大 |
| --- | --- | --- | --- |
| CPU Utilization (%) | 18 | 8.9 | 55 |
| Memory Utilization (%) | 96 | 81 | 97.36 |
| Used Physical Memory (MB) | 940 | 797 | 958 |

CPU Utilization (%)

![image](/images/reference/performance-aws-instances/t2-micro/postgresql-50msgs-cpu.png)

Memory Utilization (%)

![image](/images/reference/performance-aws-instances/t2-micro/postgresql-50msgs-memory.png)

Used Physical Memory (MB)

![image](/images/reference/performance-aws-instances/t2-micro/postgresql-50msgs-memory-1.png)

TB dashboard

![image](/images/reference/performance-aws-instances/t2-micro/postgresql-50msgs-tb.png)

### 测试运行#2

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）测试运行小时数|
| --- | --- | --- | --- | --- | --- | --- |
| t2.micro | 1 vCPUs for a 2h 24m burst, 1GB | PostgreSQL | MQTT | 100 | 1000 ms | 10 | 

**测试配置**

测试运行配置(有关更多详细信息，请参见[性能测试项目](https://github.com/thingsboard/performance-tests/#running)):

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=100

PUBLISH_COUNT=36000
PUBLISH_PAUSE=1000
...
```

**CPU/Memory Load**

| 属性 | 平均 | 最小 | 最大 |
| --- | --- | --- | --- |
| CPU Utilization (%) | 32 | 8.1 | 100 |
| Memory Utilization (%) | 97 | 81.3 | 98.37 | 
| Used Physical Memory (MB) | 952 | 800 | 968 |

CPU Utilization (%)

![image](/images/reference/performance-aws-instances/t2-micro/postgresql-100msgs-cpu.png)

Memory Utilization (%)

![image](/images/reference/performance-aws-instances/t2-micro/postgresql-100msgs-memory.png)

Used Physical Memory (MB)

![image](/images/reference/performance-aws-instances/t2-micro/postgresql-100msgs-memory-1.png)

TB dashboard

![image](/images/reference/performance-aws-instances/t2-micro/postgresql-100msgs-tb.png)

## t2.medium

**性能结果**

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟|消息上限|
| --- | --- | --- | --- | --- | --- | --- |
| t2.medium | 2 vCPUs for a 4h 48m burst, 4GB | PostgreSQL | MQTT | 900 | 1000 ms | **~780/sec** |

测试运行配置(有关更多详细信息，请参见[性能测试项目](https://github.com/thingsboard/performance-tests/#running)):

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=900

PUBLISH_COUNT=300
PUBLISH_PAUSE=1000
...
```

### 测试运行 #1

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）测试运行小时数|
| --- | --- | --- | --- | --- | --- | --- |
| t2.medium | 2 vCPUs for a 4h 48m burst, 4GB | PostgreSQL | MQTT | 150 | 1000 ms | 10 | 

**测试配置**

测试运行配置(有关更多详细信息，请参见[性能测试项目](https://github.com/thingsboard/performance-tests/#running)):

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=150

PUBLISH_COUNT=36000
PUBLISH_PAUSE=1000
...
```

**CPU/Memory Load**

| 属性 | 平均 | 最小 | 最大 |
| --- | --- | --- | --- |
| CPU Utilization (%) | 19 | 1.5 | 25 |
| Memory Utilization (%) | 25 | 3.54 | 28.3 |
| Used Physical Memory (MB) | 1014 | 551 | 1116 |

CPU Utilization (%)

![image](/images/reference/performance-aws-instances/t2-medium/postgresql-150msgs-cpu.png)

Memory Utilization (%)

![image](/images/reference/performance-aws-instances/t2-medium/postgresql-150msgs-memory.png)

Used Physical Memory (MB)

![image](/images/reference/performance-aws-instances/t2-medium/postgresql-150msgs-memory-1.png)

TB dashboard

![image](/images/reference/performance-aws-instances/t2-medium/postgresql-150msgs-tb.png)

### 测试运行 #2

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）测试运行小时数|
| --- | --- | --- | --- | --- | --- | --- |
| t2.medium | 2 vCPUs for a 4h 48m burst, 4GB | PostgreSQL | MQTT | 200 | 1000 ms | 10 | 

**测试配置**

测试运行配置(有关更多详细信息，请参见[性能测试项目](https://github.com/thingsboard/performance-tests/#running)):

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=200

PUBLISH_COUNT=36000
PUBLISH_PAUSE=1000
...
```

**CPU/Memory Load**

结果显示，由于[AWS CPU Credit Balance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-credits-baseline-concepts.html),**t2.medium** AWS实例类型每秒不能处理200个以上的请求。

这是每秒发布200条消息期间**t2.medium**的“贷方余额”图表线。

该行下降，经过一段时间后，CPU将大大减少实例（占总数的20％）。

![image](/images/reference/performance-aws-instances/t2-medium/postgresql-200msgs-failing-cpu-credit.png)

### 测试运行 #3

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）测试运行小时数|
| --- | --- | --- | --- | --- | --- | --- |
| t2.medium | 2 vCPUs for a 4h 48m burst, 4GB | PostgreSQL | MQTT | 300 | 1000 ms | 10 | 

**测试配置**

测试运行配置(有关更多详细信息，请参见[性能测试项目](https://github.com/thingsboard/performance-tests/#running)):

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=300

PUBLISH_COUNT=36000
PUBLISH_PAUSE=1000
...
```

**CPU/Memory Load**

结果与上次运行相同，但CPU信用余额图表线下降得更快。

![image](/images/reference/performance-aws-instances/t2-medium/postgresql-300msgs-failed-cpu-credit.png)

## c5.large

**性能结果**

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）每秒最大消息数|
| --- | --- | --- | --- | --- | --- | --- |
| c5.large | 2 vCPUs , 4GB | PostgreSQL | MQTT | 1100 | 1000 ms | **~1020/sec** |

测试运行配置(有关更多详细信息，请参见[性能测试项目](https://github.com/thingsboard/performance-tests/#running))：

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=1100

PUBLISH_COUNT=300
PUBLISH_PAUSE=1000
...
```

### 测试运行 #1

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）测试运行小时数|
| --- | --- | --- | --- | --- | --- | --- |
| c5.large | 2 vCPUs, 4GB | PostgreSQL | MQTT |  500 | 1000 ms | 10 |

**测试配置**

测试运行配置(有关更多详细信息，请参见[性能测试项目]（https://github.com/thingsboard/performance-tests/#running))：


```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=500

PUBLISH_COUNT=36000
PUBLISH_PAUSE=1000
...
```

**CPU/Memory Load**

**c5.large** AWS实例没有CPU爆裂，这就是为什么在这种情况下CPU信用余额不适用于验证的原因。

但是为了能够支持每秒500个请求，必须设置正确的卷-具有足够的[IOPS][IOPS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-io-characteristics.html)限制。

对于PostgreSQL数据库，每秒500个请求等于〜500 IOPS。

因此，必须为该测试的AWS卷配备至少600 IOPS。

| 属性 | 平均 | 最小 | 最大 |
| --- | --- | --- | --- |
| CPU Utilization (%) | 48 | 3 | 57.4 |
| Memory Utilization (%) | 34 | 32.79 | 37.76 |
| Used Physical Memory (MB) | 1254 | 1215 | 1399 |

CPU Utilization (%)

![image](/images/reference/performance-aws-instances/c5-large/postgresql-500msgs-cpu.png)

Memory Utilization (%)

![image](/images/reference/performance-aws-instances/c5-large/postgresql-500msgs-memory.png)

Used Physical Memory (MB)

![image](/images/reference/performance-aws-instances/c5-large/postgresql-500msgs-memory-1.png)

TB dashboard

![image](/images/reference/performance-aws-instances/c5-large/postgresql-500msgs-tb.png)

AWS write IOPS for the volume

![image](/images/reference/performance-aws-instances/c5-large/postgresql-500msgs-iops.png)

![image](/images/reference/performance-aws-instances/c5-large/postgresql-500msgs-iops-1.png)

### 测试运行 #2

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）测试运行小时数|
| --- | --- | --- | --- | --- | --- | --- |
| c5.large | 2 vCPUs, 4GB | PostgreSQL | MQTT |  700 | 1000 ms | 10 |

**测试配置**

测试运行配置(有关更多详细信息，请参见[性能测试项目]（https://github.com/thingsboard/performance-tests/#running))：

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=700

PUBLISH_COUNT=36000
PUBLISH_PAUSE=1000
...
```
 
**CPU/Memory Load**
 
| 属性 | 平均 | 最小 | 最大 |
| --- | --- | --- | --- |
| CPU Utilization (%) | 70 | 66.8 | 76.1 |
| Memory Utilization (%) | 33 | 33.39 | 33.85 |
| Used Physical Memory (MB) | 1241 | 1237 | 1254 |

CPU Utilization (%)

![image](/images/reference/performance-aws-instances/c5-large/postgresql-700msgs-cpu.png)

Memory Utilization (%)

![image](/images/reference/performance-aws-instances/c5-large/postgresql-700msgs-memory.png)

Used Physical Memory (MB)

![image](/images/reference/performance-aws-instances/c5-large/postgresql-700msgs-memory-1.png)

TB dashboard

![image](/images/reference/performance-aws-instances/c5-large/postgresql-700msgs-tb.png)

AWS write IOPS for the volume

![image](/images/reference/performance-aws-instances/c5-large/postgresql-700msgs-iops.png)

![image](/images/reference/performance-aws-instances/c5-large/postgresql-700msgs-iops-1.png)

## m5.xlarge

**性能结果**

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）每秒最大消息数|
| --- | --- | --- | --- | --- | --- | --- |
| m5.xlarge | 4 vCPUs, 16GB, 150GB SSD mounted | Cassandra | MQTT | 3500 | 1000 ms | **~3500/sec** |

测试运行配置(有关更多详细信息，请参见[性能测试项目]（https://github.com/thingsboard/performance-tests/#running))：

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=3500

PUBLISH_COUNT=300
PUBLISH_PAUSE=1000
...
```

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）每秒最大消息数|
| --- | --- | --- | --- | --- | --- | --- |
| m5.xlarge |  4 vCPUs, 16GB, 150GB SSD mounted | Cassandra | HTTP | 2000 | 1000 ms | **~950/sec** |

试运行配置：

```bash
...
DEVICE_API=HTTP
DEVICE_START_IDX=0
DEVICE_END_IDX=2000

PUBLISH_COUNT=300
PUBLISH_PAUSE=1000
...
```

### 测试运行 #1

|实例类型|实例详细信息|数据库类型|设备API |设备数量|消息延迟（以毫秒为单位）测试运行小时数|
| --- | --- | --- | --- | --- | --- | --- |
| m5.xlarge | 4 vCPUs, 16GB | Cassandra | MQTT | 2100 | 1000 ms | 10 |

**测试配置**

测试运行配置(有关更多详细信息，请参见[性能测试项目]（https://github.com/thingsboard/performance-tests/#running))：

```bash
...
DEVICE_API=MQTT
DEVICE_START_IDX=0
DEVICE_END_IDX=2100

PUBLISH_COUNT=36000
PUBLISH_PAUSE=1000
...
```

**CPU/memory load**

| 属性 | 平均 | 最小 | 最大 |
| --- | --- | --- | --- |
| CPU Utilization (%) | 36 | 8.3 | 61.2 |
| Memory Utilization (%) | 40 | 39.83 | 40.14 |
| Used Physical Memory (MB) | 6235 | 6205 | 6252 |

CPU Utilization (%)

![image](/images/reference/performance-aws-instances/m5-xlarge/cassandra-2100msgs-cpu.png)

Memory Utilization (%)

![image](/images/reference/performance-aws-instances/m5-xlarge/cassandra-2100msgs-memory.png)

Used Physical Memory (MB)

![image](/images/reference/performance-aws-instances/m5-xlarge/cassandra-2100msgs-memory-1.png)

TB dashboard

![image](/images/reference/performance-aws-instances/m5-xlarge/cassandra-2100msgs-tb.png)
