---
layout: docwithnav
title: ThingsBoard微服务架构
description: ThingsBoard架构

---

* TOC
{:toc}

从ThingsBoard v2.2开始，该平台支持微服务部署模式。
本文由高层示意图，各种服务之间的数据流描述以及一些体系结构选择组成。

## 架构图

 <object width="80%" data="/images/reference/msa-architecture.svg"></object> 
  
## 传输微服务

ThingsBoard提供了基于MQTT，HTTP和CoAP的API，可用于您的设备应用程序/固件。
每个协议API都是由单独的服务器组件提供的，并且是ThingsBoard“应用层”的一部分。
下面列出了组件的完整列表和相应的文档页面：

* HTTP应用层微服务[API](/docs/reference/http-api/); 
* MQTT应用层微服务[API](/docs/reference/mqtt-api/)与[网关API](/docs/reference/gateway-mqtt-api/)；
* CoAP应用层微服务[API](/docs/reference/coap-api/)。

上面列出的每个传输服务器都使用Kafka与主要的ThingsBoard节点微服务进行通信。
[Apache Kafka](https://kafka.apache.org)是一个分布式，可靠且可扩展的持久消息队列和流平台。

发送消息至Kafka的[缓冲区](https://developers.google.com/protocol-buffers/)进行序列化并提供[消息定义](https://github.com/thingsboard/thingsboard/blob/master/common/transport/transport-api/src/main/proto/transport.proto)

**注意**：从v2.5开始，ThingsBoard PE将支持替代队列实现：Amazon DynamoDB。有关更多详细信息，请参见[路线图](/docs/reference/roadmap)。
 
传输层微服务使用两个主要主题。

第一个主题"tb.transport.api.requests"用于执行短期API请求，以检查设备凭据或代表网关创建设备。对此请求的响应将发送到特定于每个传输微服务的主题。此类“回调”主题的前缀默认为"tb.transport.api.responses"。

第二个主题"tb.rule-engine"用于存储来自设备的所有传入遥测消息，直到规则引擎不处理它们为止。如果规则引擎节点已关闭，则消息将保留并可供以后处理。

您可以在下面看到配置文件的一部分来指定这些属性：

```yaml
transport:
  type: "${TRANSPORT_TYPE:local}" # local or remote
  remote:
    transport_api:
      requests_topic: "${TB_TRANSPORT_API_REQUEST_TOPIC:tb.transport.api.requests}"
      responses_topic: "${TB_TRANSPORT_API_RESPONSE_TOPIC:tb.transport.api.responses}"
    rule_engine:
      topic: "${TB_RULE_ENGINE_TOPIC:tb.rule-engine}"
```    

由于ThingsBoard在传输和核心服务之间使用了非常简单的通信协议，因此实现对自定义传输协议的支持非常容易，例如：纯TCP上的CSV，UDP上的二进制payloads等。
我们建议您查看现有的运输工具[实现](https://github.com/thingsboard/thingsboard/tree/master/common/transport/mqtt)以开始使用或者如果需要请与[联系我们](/docs/contact-us/)您需要任何帮助。

## Web UI微服务

ThingsBoard提供了一个使用Express.js框架编写的轻量级组件，用于承载静态Web ui内容。这些组件是完全无状态的，没有太多可用的配置。

## JavaScript执行器微服务

ThingsBoard规则引擎允许用户指定自定义javascript函数来解析，过滤和转换消息。
由于这些功能是用户定义的，因此我们需要在隔离的上下文中执行它们，以避免影响主处理。
ThingsBoard提供了使用Node.js编写的轻量级组件，以远程执行用户定义的JavaScript函数，以将其与核心规则引擎组件隔离。

**注意**：ThingsBoard整体应用程序在Java嵌入式JS引擎中执行用户定义的函数，该函数不允许隔离资源消耗。
 
我们建议启动20多个单独的JavaScript执行器，以允许一定的并发级别和JS执行请求的负载平衡。
每个微服务都将作为单个使用者组的一部分订阅"js.eval.requests" kafka主题，以实现负载平衡。
使用内置的Kafka按键分区（键是脚本/规则节点ID），将对相同脚本的请求转发到相同的JS执行程序。

可以定义未决JS执行请求的最大数量和最大请求​​超时，以避免单个JS执行阻止JS exector微服务。
每个ThingsBoard核心服务都有针对JS函数的单独黑名单，并且不会调用被阻止的函数超过3（默认）次。

## ThingsBoard节点

ThingsBoard节点是用Java编写的核心服务，负责处理：
 
 * 调用[REST API](/docs/reference/rest-api/);
 * WebSocket [subscriptions](/docs/user-guide/telemetry/#websocket-api) 有关实体遥测和属性更改的信息；
 * 通过[规则引擎]处理消息(/docs/user-guide/rule-engine-2-0/re-getting-started/);
 * 监视设备 [connectivity state](/docs/user-guide/device-connectivity-status/) （活动/不活动）。
 
**注意**：已将ThingsBoard v2.5的规则引擎移动到单独的微服务中。有关更多详细信息请参见[路线图](/docs/reference/roadmap)。
 
ThingsBoard节点使用Akka actor系统来实现租户，设备，规则链和规则节点actor。
平台节点可以加入每个节点都相等的集群。服务发现是通过Zookeeper完成的。
ThingsBoard节点使用基于实体ID的一致哈希算法在彼此之间路由消息。
因此用于同一实体的消息在同一ThingsBoard节点上进行处理。平台使用[gRPC]（https://grpc.io/）在ThingsBoard节点之间发送消息。

**注意**：ThingsBoard作者考虑在将来的版本中从gRPC迁移到Kafka，以便在ThingsBoard节点之间交换消息。
主要思想是牺牲性能/等待时间的小损失，以支持由卡夫卡用户群提供的持久可靠的消息传递和自动负载平衡。

## Third-party  

### Kafka

[Apache Kafka](https://kafka.apache.org/) 是一个开放源代码的流处理软件平台。  ThingsBoard使用Kafka来保留来自 HTTP/MQTT/CoAP的传入遥测直到其被规则引擎处理为止。 ThingsBoard还使用Kafka在微服务之间进行一些API调用。

### Redis

[Redis](https://redis.io/) 是一个开源的（BSD许可）的内存数据结构存储在ThingsBoard用于缓存。
ThingsBoard缓存资产，实体视图，设备，设备凭证，设备会话和实体关系。

### Zookeeper

[Zookeeper]（https://zookeeper.apache.org/）是一种开源服务器，可实现高度可靠的分布式协调。
ThingsBoard使用Zookeeper来处理从单个实体（设备，资产，承租人）到特定ThingsBoard服务器的请求处理，并确保只有一个服务器在单个时间点处理来自特定设备的数据。

**注意**：Kafka也使用Zookeeper，因此几乎没有理由并行使用两种不同的协调服务（Consul等）。

### HAProxy（或其他LoadBalancer）

我们建议使用HAProxy进行负载平衡。
您可以找到参考 [haproxy.cfg](https://github.com/thingsboard/thingsboard/blob/release-2.4/docker/haproxy/config/haproxy.cfg) 
与以下架构图相对应的配置：

{% highlight conf %}
#HA Proxy Config
global
 ulimit-n 500000
 maxconn 99999
 maxpipes 99999
 tune.maxaccept 500

 log 127.0.0.1 local0
 log 127.0.0.1 local1 notice

 ca-base /etc/ssl/certs
 crt-base /etc/ssl/private

 ssl-default-bind-ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS
 ssl-default-bind-options no-sslv3

defaults

 log global

 mode http

 timeout connect 5000ms
 timeout client 50000ms
 timeout server 50000ms
 timeout tunnel  1h    # timeout to use with WebSocket and CONNECT

 default-server init-addr none

#enable resolving throught docker dns and avoid crashing if service is down while proxy is starting
resolvers docker_resolver
  nameserver dns 127.0.0.11:53

listen stats
 bind *:9999
 stats enable
 stats hide-version
 stats uri /stats
 stats auth admin:admin@123

listen mqtt-in
 bind *:${MQTT_PORT}
 mode tcp
 option clitcpka # For TCP keep-alive
 timeout client 3h
 timeout server 3h
 option tcplog
 balance leastconn
 server tbMqtt1 tb-mqtt-transport1:1883 check inter 5s resolvers docker_resolver resolve-prefer ipv4
 server tbMqtt2 tb-mqtt-transport2:1883 check inter 5s resolvers docker_resolver resolve-prefer ipv4

frontend http-in
 bind *:${HTTP_PORT}

 option forwardfor

 reqadd X-Forwarded-Proto:\ http

 acl acl_static path_beg /static/ /index.html
 acl acl_static path /
 acl acl_static_rulenode path_beg /static/rulenode/

 acl transport_http_acl path_beg /api/v1/
 acl letsencrypt_http_acl path_beg /.well-known/acme-challenge/

 redirect scheme https if !letsencrypt_http_acl !transport_http_acl { env(FORCE_HTTPS_REDIRECT) -m str true }

 use_backend letsencrypt_http if letsencrypt_http_acl
 use_backend tb-http-backend if transport_http_acl
 use_backend tb-web-backend if acl_static !acl_static_rulenode

 default_backend tb-api-backend

frontend https_in
  bind *:${HTTPS_PORT} ssl crt /usr/local/etc/haproxy/default.pem crt /usr/local/etc/haproxy/certs.d ciphers ECDHE-RSA-AES256-SHA:RC4-SHA:RC4:HIGH:!MD5:!aNULL:!EDH:!AESGCM

  option forwardfor

  reqadd X-Forwarded-Proto:\ https

  acl transport_http_acl path_beg /api/v1/

  acl acl_static path_beg /static/ /index.html
  acl acl_static path /
  acl acl_static_rulenode path_beg /static/rulenode/

  use_backend tb-http-backend if transport_http_acl
  use_backend tb-web-backend if acl_static !acl_static_rulenode

  default_backend tb-api-backend

backend letsencrypt_http
  server letsencrypt_http_srv 127.0.0.1:8080

backend tb-web-backend
  balance leastconn
  option tcp-check
  option log-health-checks
  server tbWeb1 tb-web-ui1:8080 check inter 5s resolvers docker_resolver resolve-prefer ipv4
  server tbWeb2 tb-web-ui2:8080 check inter 5s resolvers docker_resolver resolve-prefer ipv4
  http-request set-header X-Forwarded-Port %[dst_port]

backend tb-http-backend
  balance leastconn
  option tcp-check
  option log-health-checks
  server tbHttp1 tb-http-transport1:8081 check inter 5s resolvers docker_resolver resolve-prefer ipv4
  server tbHttp2 tb-http-transport2:8081 check inter 5s resolvers docker_resolver resolve-prefer ipv4

backend tb-api-backend
  balance leastconn
  option tcp-check
  option log-health-checks
  server tbApi1 tb1:8080 check inter 5s resolvers docker_resolver resolve-prefer ipv4
  server tbApi2 tb2:8080 check inter 5s resolvers docker_resolver resolve-prefer ipv4
  http-request set-header X-Forwarded-Port %[dst_port]
{% endhighlight %}

### Databases

有关更多详细信息请参见 "[SQL vs NoSQL vs Hybrid?](/docs/reference/#sql-vs-nosql-vs-hybrid-database-approach)" 。

## Deployment

您可以找到参考[docker-compose.yml](https://github.com/thingsboard/thingsboard/blob/release-2.4/docker/docker-compose.yml) 和相应的 [documentation](https://github.com/thingsboard/thingsboard/blob/master/docker/README.md) 它将帮助您在集群模式下（尽管在单个主机上）运行ThingsBoard容器。

{% highlight yaml %}
version: '2.2'

services:
  zookeeper:
    restart: always
    image: "zookeeper:3.5"
    ports:
      - "2181"
    environment:
      ZOO_MY_ID: 1
      ZOO_SERVERS: server.1=zookeeper:2888:3888;zookeeper:2181
  kafka:
    restart: always
    image: "wurstmeister/kafka"
    ports:
      - "9092:9092"
    env_file:
      - kafka.env
    depends_on:
      - zookeeper
  redis:
    restart: always
    image: redis:4.0
    ports:
      - "6379"
  tb-js-executor:
    restart: always
    image: "${DOCKER_REPO}/${JS_EXECUTOR_DOCKER_NAME}:${TB_VERSION}"
    scale: 20
    env_file:
      - tb-js-executor.env
    depends_on:
      - kafka
  tb1:
    restart: always
    image: "${DOCKER_REPO}/${TB_NODE_DOCKER_NAME}:${TB_VERSION}"
    ports:
      - "8080"
    logging:
      driver: "json-file"
      options:
        max-size: "200m"
        max-file: "30"
    environment:
      TB_HOST: tb1
      CLUSTER_NODE_ID: tb1
    env_file:
      - tb-node.env
    volumes:
      - ./tb-node/conf:/config
      - ./tb-node/log:/var/log/thingsboard
    depends_on:
      - kafka
      - redis
      - tb-js-executor
  tb2:
    restart: always
    image: "${DOCKER_REPO}/${TB_NODE_DOCKER_NAME}:${TB_VERSION}"
    ports:
      - "8080"
    logging:
      driver: "json-file"
      options:
        max-size: "200m"
        max-file: "30"
    environment:
      TB_HOST: tb2
      CLUSTER_NODE_ID: tb2
    env_file:
      - tb-node.env
    volumes:
      - ./tb-node/conf:/config
      - ./tb-node/log:/var/log/thingsboard
    depends_on:
      - kafka
      - redis
      - tb-js-executor
  tb-mqtt-transport1:
    restart: always
    image: "${DOCKER_REPO}/${MQTT_TRANSPORT_DOCKER_NAME}:${TB_VERSION}"
    ports:
      - "1883"
    environment:
      TB_HOST: tb-mqtt-transport1
      CLUSTER_NODE_ID: tb-mqtt-transport1
    env_file:
      - tb-mqtt-transport.env
    volumes:
      - ./tb-transports/mqtt/conf:/config
      - ./tb-transports/mqtt/log:/var/log/tb-mqtt-transport
    depends_on:
      - kafka
  tb-mqtt-transport2:
    restart: always
    image: "${DOCKER_REPO}/${MQTT_TRANSPORT_DOCKER_NAME}:${TB_VERSION}"
    ports:
      - "1883"
    environment:
      TB_HOST: tb-mqtt-transport2
      CLUSTER_NODE_ID: tb-mqtt-transport2
    env_file:
      - tb-mqtt-transport.env
    volumes:
      - ./tb-transports/mqtt/conf:/config
      - ./tb-transports/mqtt/log:/var/log/tb-mqtt-transport
    depends_on:
      - kafka
  tb-http-transport1:
    restart: always
    image: "${DOCKER_REPO}/${HTTP_TRANSPORT_DOCKER_NAME}:${TB_VERSION}"
    ports:
      - "8081"
    environment:
      TB_HOST: tb-http-transport1
      CLUSTER_NODE_ID: tb-http-transport1
    env_file:
      - tb-http-transport.env
    volumes:
      - ./tb-transports/http/conf:/config
      - ./tb-transports/http/log:/var/log/tb-http-transport
    depends_on:
      - kafka
  tb-http-transport2:
    restart: always
    image: "${DOCKER_REPO}/${HTTP_TRANSPORT_DOCKER_NAME}:${TB_VERSION}"
    ports:
      - "8081"
    environment:
      TB_HOST: tb-http-transport2
      CLUSTER_NODE_ID: tb-http-transport2
    env_file:
      - tb-http-transport.env
    volumes:
      - ./tb-transports/http/conf:/config
      - ./tb-transports/http/log:/var/log/tb-http-transport
    depends_on:
      - kafka
  tb-coap-transport:
    restart: always
    image: "${DOCKER_REPO}/${COAP_TRANSPORT_DOCKER_NAME}:${TB_VERSION}"
    ports:
      - "5683:5683/udp"
    environment:
      TB_HOST: tb-coap-transport
      CLUSTER_NODE_ID: tb-coap-transport
    env_file:
      - tb-coap-transport.env
    volumes:
      - ./tb-transports/coap/conf:/config
      - ./tb-transports/coap/log:/var/log/tb-coap-transport
    depends_on:
      - kafka
  tb-web-ui1:
    restart: always
    image: "${DOCKER_REPO}/${WEB_UI_DOCKER_NAME}:${TB_VERSION}"
    ports:
      - "8080"
    env_file:
      - tb-web-ui.env
  tb-web-ui2:
    restart: always
    image: "${DOCKER_REPO}/${WEB_UI_DOCKER_NAME}:${TB_VERSION}"
    ports:
      - "8080"
    env_file:
      - tb-web-ui.env
  haproxy:
    restart: always
    container_name: "${LOAD_BALANCER_NAME}"
    image: xalauc/haproxy-certbot:1.7.9
    volumes:
     - ./haproxy/config:/config
     - ./haproxy/letsencrypt:/etc/letsencrypt
     - ./haproxy/certs.d:/usr/local/etc/haproxy/certs.d
    ports:
     - "80:80"
     - "443:443"
     - "1883:1883"
     - "9999:9999"
    cap_add:
     - NET_ADMIN
    environment:
      HTTP_PORT: 80
      HTTPS_PORT: 443
      MQTT_PORT: 1883
      FORCE_HTTPS_REDIRECT: "false"
    links:
        - tb1
        - tb2
        - tb-web-ui1
        - tb-web-ui2
        - tb-mqtt-transport1
        - tb-mqtt-transport2
        - tb-http-transport1
        - tb-http-transport2
{% endhighlight %}




    
