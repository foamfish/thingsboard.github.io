---
layout: docwithnav
title: ThingsBoard数据收集性能
description: ThingsBoard数据收集性能概述

---

* TOC
{:toc}

ThingsBoard开源物联网平台的关键功能之一就是数据收集，这是一项必须在高负载下可靠运行的关键功能。
在本文中，我们将描述为确保ThingsBoard服务器的单个实例而进行的步骤和改进。
可以每秒不断处理 **20,000+** 个设备和 **30,000+** MQTT发布消息，
总而言之，这使我们**每分钟发布200万条消息**。

## 架构

ThingsBoard的性能利用了三个主要项目：

 - [Netty](http://netty.io/) 用于IoT设备的高性能MQTT server/broker。
 - [Akka](http://akka.io/) 用于高性能actor系统，以协调数百万个设备之间的消息。
 - [Cassandra](http://cassandra.apache.org/) 用于可扩展的高性能NoSQL DB，用于存储来自设备的时间序列数据。
 
我们还使用 [Zookeeper](https://zookeeper.apache.org/) 进行协调，并在集群模式下使用 [gRPC](http://www.grpc.io/) 有关更多详细信息，请参见[平台架构](/docs/reference/)。

## 数据流和测试工具
 
物联网设备通过MQTT连接到ThingsBoard服务器，并使用JSON payload发出"publish"命令。
单个发布消息的大小约为100个字节。
[MQTT](http://mqtt.org/) 是轻量级的发布/订阅消息传递协议，与HTTP请求/响应协议相比，具有许多优点。
 
![image](/images/reference/performance/performance-diagram-0.svg)

ThingsBoard服务器处理MQTT发布消息，并将消息异步存储到Cassandra。
服务器还可以从Web UI仪表板（如果存在）将数据推送到websocket订阅。
我们尝试避免任何阻塞操作，这对于整体系统性能至关重要。
ThingsBoard支持MQTT QoS级别1，这意味着客户端仅在将数据存储到Cassandra DB之后才收到对发布消息的响应。
QoS级别为1时可能出现的数据重复只是对相应Cassandra行的覆盖，因此不存在于持久数据中。
此功能提供了可靠的数据传递和持久性。

我们使用了 [Gatling](http://gatling.io/) 负载测试框架，该框架也基于Akka和Netty。
Gatling能够使用5到10％的2核CPU模拟10K MQTT客户端。
有关我们如何改进非官方的Gatling MQTT插件以支持我们的用例，请参见我们单独的 [article](/docs/reference/performance-tools)。

## 性能改进步骤

### 步骤1.异步Cassandra驱动程序API

在配备SSD的现代4核笔记本电脑上进行首次性能测试的结果相当差。该平台每秒只能处理200条消息。
根本原因和主要性能瓶颈很明显并且很容易找到。
看来处理不是100％异步的，我们正在[Telemetry Service](/docs/user-guide/telemetry/) actor中执行Cassandra驱动程序的阻塞API调用。
服务实施的快速重构导致性能提高了10倍以上，我们每秒从1000台设备收到大约2500条发布的消息。
我们想推荐有关对Cassandra进行异步查询的 [this article](http://www.datastax.com/dev/blog/java-driver-async-queries)。 

### 步骤2.连接池

我们已决定移至AWS EC2实例，以便能够共享结果和我们执行的测试。我们开始在[c4.xlarge](http://www.ec2instances.info/?selected=c4.xlarge)实例（4个vCPU和7.5 Gb RAM）上运行测试，并将Cassandra和ThingsBoard服务并置。

![image](/images/reference/performance/performance-diagram-1.svg)

测试规格：

 - 设备数量：10000
 - 每台设备的发布频率：每秒一次
 - 总负载：每秒10000条消息
 
最初的测试结果显然是不可接受的：

![image](/images/reference/performance/single_node_no_fix_stats.png) 
 

上面的巨大响应时间是由于服务器根本无法每秒处理10 K条消息并且它们正在排队而造成的。

我们已经开始通过监视测试实例上的内存和CPU负载进行调查。
最初，我们对性能不佳的猜测是由于CPU或RAM的负载过重。
但是实际上，在负载测试期间，我们发现特定时刻的CPU处于空闲状态几秒钟。
此“暂停”事件每3-7秒发生一次，请参见下表。
 
![image](/images/reference/performance/single_node_no_fix_rps.png) 

下一步，我们决定在这些暂停期间进行线程转储。
我们期望看到线程被阻塞，这可能会给我们一些线索，让我们了解暂停时发生了什么。
因此，我们打开了一个单独的控制台来监视CPU负载，并打开另一个控制台来执行线程转储，同时使用以下命令执行压力测试：

```bash

kill -3 THINGSBOARD_PID

```

我们已经确定，在暂停期间，始终有一个线程处于TIMED_WAITING状态，并且根本原因是Cassandra驱动程序的awaitAvailableConnection方法：

```bash
java.lang.Thread.State: TIMED_WAITING (parking)
at sun.misc.Unsafe.park(Native Method)
parking to wait for  <0x0000000092d9d390> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
at java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:215)
at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(AbstractQueuedSynchronizer.java:2163)
at com.datastax.driver.core.HostConnectionPool.awaitAvailableConnection(HostConnectionPool.java:287)
at com.datastax.driver.core.HostConnectionPool.waitForConnection(HostConnectionPool.java:328)
at com.datastax.driver.core.HostConnectionPool.borrowConnection(HostConnectionPool.java:251)
at com.datastax.driver.core.RequestHandler$SpeculativeExecution.query(RequestHandler.java:301)
at com.datastax.driver.core.RequestHandler$SpeculativeExecution.sendRequest(RequestHandler.java:281)
at com.datastax.driver.core.RequestHandler.startNewExecution(RequestHandler.java:115)
at com.datastax.driver.core.RequestHandler.sendRequest(RequestHandler.java:91)
at com.datastax.driver.core.SessionManager.executeAsync(SessionManager.java:132)
at org.thingsboard.server.dao.AbstractDao.executeAsync(AbstractDao.java:91)
at org.thingsboard.server.dao.AbstractDao.executeAsyncWrite(AbstractDao.java:75)
at org.thingsboard.server.dao.timeseries.BaseTimeseriesDao.savePartition(BaseTimeseriesDao.java:135)
```

结果，我们意识到cassandra驱动程序的默认连接池配置在我们的用例中导致了不好的结果。

连接池[官方配置](http://docs.datastax.com/en/developer/java-driver/2.1/manual/pooling/)包含特殊选项
**'每个连接的并发请求'**，您可以调整每个连接的并发请求。
我们使用cassandra驱动程序协议v3，默认情况下使用以下值：
 
 - LOCAL主机为1024。
 - 远程主机为256。

考虑到实际上我们实际上是从10,000个设备中提取数据，因此默认值绝对不够。
因此，我们对本地和远程主机的代码和更新值进行了更改，并将它们设置为最大可能值：

```java
poolingOptions
    .setMaxRequestsPerConnection(HostDistance.LOCAL, 32768)
    .setMaxRequestsPerConnection(HostDistance.REMOTE, 32768);
```

应用的更改后的测试结果如下所示。

![image](/images/reference/performance/single_node_with_fix_stats.png)
 
![image](/images/reference/performance/single_node_with_fix_rps.png) 

结果要好得多，但是每分钟甚至不到一百万条消息。在c4.xlarge上的测试期间，我们再也没有看到CPU负载的暂停。
在整个测试过程中，CPU负载很高（80-95％）。我们已经完成了几个线程转储，以验证cassandra驱动程序不会等待可用的连接，实际上我们再也没有看到此问题。

### 步骤3：垂直缩放
 
我们已决定在具有8个vCPU和15Gb RAM的功能更强大的节点 [c4.2xlarge](http://www.ec2instances.info/?selected=c4.2xlarge) 的两倍上运行相同的测试。
性能提升不是线性的，并且CPU仍处于加载状态（80-90％）。

![image](/images/reference/performance/single_node_x2_with_fix_stats.png)

我们注意到响应时间有了显着改善。在测试开始出现明显峰值后，最大响应时间在200ms以内，平均响应时间为~ 50ms。

![image](/images/reference/performance/single_node_x2_with_fix_time.png)

每秒的请求数约为1万

![image](/images/reference/performance/single_node_x2_with_fix_rps.png)

我们还在具有16个vCPU和30Gb RAM的[c4.4xlarge](http://www.ec2instances.info/?selected=c4.4xlarge)上执行了测试，但是尚未注意到明显的改进，因此决定分离并移动ThingsBoard服务器Cassandra以三个节点为集群。

### 步骤4：水平缩放

我们的主要目标是确定使用在 [c4.2xlarge](http://www.ec2instances.info/?selected=c4.2xlarge)上运行的单个ThingsBoard服务器可以处理多少MQTT消息。
我们将在另一篇文章中介绍ThingsBoard集群的水平可伸缩性。
因此，我们决定将Cassandra移至具有默认配置的三个[c4.xlarge]（单独的实例），并从两个单独的[c4.xlarge]启动加特林压力测试工具同时（）（http://www.ec2instances.info/?selected=c4.xlarge）实例，以最大程度地减少第三方对延迟和吞吐量的可能影响。

![image](/images/reference/performance/performance-diagram-2.svg)

测试规格：

 - 设备数量：20000
 - 每台设备的发布频率：每秒两次
 - 总负载：每秒40 000条消息

下面列出了在不同客户端计算机上启动的两个同时测试运行的统计信息。
 
![image](/images/reference/performance/cluster_stats.png)
![image](/images/reference/performance/cluster_rps.png)
![image](/images/reference/performance/cluster_responses_ps.png)

根据两次同时运行的数据，我们已经**每秒发布3万条消息**，相当于**每分钟180万条**。

## 如何重复测试

我们已经为对这些测试的复制感兴趣的任何人准备了几个AWS AMI。请参阅单独的[文档页面](/docs/reference/performance-tests) 以及详细说明。

## 结论

这项性能测试演示了一个小型的ThingsBoard集群如何轻松接收（每小时费用约为**1$**），
存储和可视化来自您设备的**100 million messages**。
我们将继续进行性能改进工作，并将在下一篇博客文章中发布ThingsBoard服务器集群的性能结果。
我们希望本文对正在评估该平台并希望自己执行性能测试的人员有用。
我们也希望性能改进步骤对使用类似技术的任何工程师都是有用的。

请让我们知道您的反馈，并在[**Github**](https://github.com/thingsboard/thingsboard)或[**Twitter**](https://twitter.com/thingsboard)上关注我们的项目。
