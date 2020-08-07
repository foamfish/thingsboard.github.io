---
layout: docwithnav
title: MQTT配置
description: ThingsBoard网关的MQTT协议支持

---

* TOC
{:toc}

本指南将帮助您熟悉ThingsBoard网关的MQTT连接器配置。
使用[常规配置](/docs/iot-gateway/configuration/)启用此连接器。

该连接器的目的是连接到外部MQTT broker并订阅来自设备的数据收集。

连接器还能够根据ThingsBoard的updates/commands将数据推送到MQTT broker。

当您的应用网络中有MQTT broker并且您希望将数据从相关broker推送到ThingsBoard时应选择MQTT连接器。

我们将在下面描述连接器配置文件。

## 连接器配置: mqtt.json

连接器配置是一个JSON文件其中包含有关如何连接到外部MQTT broker信息，订阅数据收集时使用哪些topics以及如何处理数据信息。

让我们使用以下示例来查看配置文件的格式。

<br>
<details>

<summary>
<b>MQTT连接器配置文件示例</b>
</summary>

下面列出的示例将连接到IP地址：192.168.1.100服务器上部署在本地网络中的MQTT broker。

使用户名和密码作为MQTT的基础身份验证。

连接器将使用映射部分中的topic过滤器订阅主题列表。在下面的说明中查看更多信息。

{% highlight json %}

{
  "broker": {
    "name":"Default Local Broker",
    "host":"192.168.1.100",
    "port":1883,
    "security": {
      "type": "basic",
      "username": "user",
      "password": "password"
    }
  },
  "mapping": [
    {
      "topicFilter": "/sensor/data",
      "converter": {
        "type": "json",
        "deviceNameJsonExpression": "${serialNumber}",
        "deviceTypeJsonExpression": "${sensorType}",
        "timeout": 60000,
        "attributes": [
          {
            "type": "string",
            "key": "model",
            "value": "${sensorModel}"
          }
        ],
        "timeseries": [
          {
            "type": "double",
            "key": "temperature",
            "value": "${temp}"
          },
          {
            "type": "double",
            "key": "humidity",
            "value": "${hum}"
          }
        ]
      }
    },
    {
      "topicFilter": "/sensor/+/data",
      "converter": {
        "type": "json",
        "deviceNameTopicExpression": "(?<=sensor\/)(.*?)(?=\/data)",
        "deviceTypeTopicExpression": "Thermometer",
        "timeout": 60000,
        "attributes": [
          {
            "type": "string",
            "key": "model",
            "value": "${sensorModel}"
          }
        ],
        "timeseries": [
          {
            "type": "double",
            "key": "temperature",
            "value": "${temp}"
          },
          {
            "type": "double",
            "key": "humidity",
            "value": "${hum}"
          }
        ]
      }
    },
    {
      "topicFilter": "/custom/sensors/+",
      "converter": {
        "type": "custom",
        "extension": "CustomMqttUplinkConverter",
        "extension-config": {
            "temperatureBytes" : 2,
            "humidityBytes" :  2,
            "batteryLevelBytes" : 1
        }
      }
    }
  ],
  "connectRequests": [
    {
      "topicFilter": "sensor/connect",
      "deviceNameJsonExpression": "${SerialNumber}"
    },
    {
      "topicFilter": "sensor/+/connect",
      "deviceNameTopicExpression": "(?<=sensor\/)(.*?)(?=\/connect)"
    }
  ],
  "disconnectRequests": [
    {
      "topicFilter": "sensor/disconnect",
      "deviceNameJsonExpression": "${SerialNumber}"
    },
    {
      "topicFilter": "sensor/+/disconnect",
      "deviceNameTopicExpression": "(?<=sensor\/)(.*?)(?=\/disconnect)"
    }
  ],
  "attributeUpdates": [
    {
      "deviceNameFilter": "SmartMeter.*",
      "attributeFilter": "uploadFrequency",
      "topicExpression": "sensor/${deviceName}/${attributeKey}",
      "valueExpression": "{\"${attributeKey}\":\"${attributeValue}\"}"
    }
  ],
  "serverSideRpc": [
    {
      "deviceNameFilter": ".*",
      "methodFilter": "echo",
      "requestTopicExpression": "sensor/${deviceName}/request/${methodName}/${requestId}",
      "responseTopicExpression": "sensor/${deviceName}/response/${methodName}/${requestId}",
      "responseTimeout": 10000,
      "valueExpression": "${params}"
    },
    {
      "deviceNameFilter": ".*",
      "methodFilter": "no-reply",
      "requestTopicExpression": "sensor/${deviceName}/request/${methodName}/${requestId}",
      "valueExpression": "${params}"
    }
  ]
}



{% endhighlight %}

</details>

### "broker"说明

| **参数** | **默认值**              | **描述**                                        |
|:-|:-|-
| name          | **Default Broker**             | Broker name for logs and saving to persistent devices. |
| host          | **localhost**                  | Mqtt broker hostname or ip address.                    |
| port          | **1883**                       | Mqtt port on the broker.                               |
|---

#### "security"说明

MQTT Broker在客户端上的安全配置。
 
{% capture mqttconnectorsecuritytogglespec %}
Basic<small>Recommended</small>%,%accessToken%,%templates/iot-gateway/mqtt-connector-basic-security-config.md%br%
Anonymous<small>No security</small>%,%anonymous%,%templates/iot-gateway/mqtt-connector-anonymous-security-config.md%br%
Certificates<small>For advanced security</small>%,%tls%,%templates/iot-gateway/mqtt-connector-tls-security-config.md{% endcapture %}

{% include content-toggle.html content-toggle-id="mqttConnectorCredentialsConfig" toggle-spec=mqttconnectorsecuritytogglespec %}  

### "mapping"说明

mapping部分包含网关连接到代理后将订阅的主题数组和有关处理传入消息（转换器）的设置。

|**参数**|**默认值**|**描述**|
|:-|:-|-
| topicFilter | **/sensor/data** | Topic address for subscribing. |
|---


**topicFilter**支持特殊符号‘＃’和‘+’以允许订阅多个topic。

假设我们要订阅并处理来自温度计设备的如下数据：

|**Example Name**|**Topic**|**Topic Filter**|**Payload**|**Comments**|
|:-|:-|:-|-
| Example 1 | /sensor/data | /sensor/data | {"serialNumber": "SN-001", "sensorType": "Thermometer", "sensorModel": "T1000", "temp":  42, "hum": 58} | Device Name is part of the payload|
| Example 2 | /sensor/SN-001/data | /sensor/+/data | { "sensorType": "Thermometer", "sensorModel": "T1000", "temp":  42, "hum": 58} | Device Name is part of the topic|
|---

现在，让我们回顾一下如何配置JSON转换器来解析此数据

#### "converter"说明
本小节包含用于处理传入消息的配置。

mqtt转换器的类型：

1. json -- 默认转换器
2. custom -- 自定义转换器（您可以自己编写，它将用于转换来自代理的传入数据。）

{% capture mqttconvertertypespec %}
json<small>Recommended if json will be received in response</small>%,%json%,%templates/iot-gateway/mqtt-converter-json-config.md%br%
custom<small>Recommended if bytes or anything else will be received in response</small>%,%custom%,%templates/iot-gateway/mqtt-converter-custom-config.md{% endcapture %}

{% include content-toggle.html content-toggle-id="MqttConverterTypeConfig" toggle-spec=mqttconvertertypespec %}


**注意**：您可以在数组中指定多个映射对象。

Mapping使用映射对象的**topicFilter**订阅MQTT主题。

分析其他设备或应用程序为此主题发布的每条消息，以提取设备名称，类型和数据（属性/时间序列值）。

默认情况下网关使用Json转换器，但是可以提供自定义转换器。请参阅源代码中的示例。

### "connectRequests"说明

ThingsBoard允许向设备发送RPC命令和有关设备属性更新的通知。

但是为了发送它们，平台需要知道目标设备是否已连接以及当前使用什么网关或会话来连接设备。

如果您的设备不断发送遥测数据-ThingsBoard已经知道如何推送通知。

如果您的设备仅连接到MQTT代理并等待commands/updates，则需要向网关发送消息并通知该设备已连接到代理。
 
**1. Name in a message from broker:**

| **参数**                 | **默认值**                     | **描述**                                                                                   |
|:-|:-|-
| topicFilter                   | **sensors/connect**                   | Topic address on the broker, where the broker sends information about new connected devices.      |
| deviceNameJsonExpression      | **${SerialNumber}**                 | JSON-path expression, for looking the new device name.                                            |
|---

**2. Name in topic address:**

| **参数**                 | **默认值**                     | **描述**                                                                                   |
|:-|:-|-
| topicFilter                   | **sensors/+/connect**                 | Topic address on the broker, where the broker sends information about new connected devices.      |
| deviceNameTopicExpression     | **(?<=sensor\/)(.\*?)(?=\/connect)**  | Regular expression for looking the device name in topic path.                                     |
|---

示例如下：
```json
  "connectRequests": [
    {
      "topicFilter": "sensors/connect",
      "deviceNameJsonExpression": "${$.SerialNumber}"
    },
    {
      "topicFilter": "sensor/+/connect",
      "deviceNameTopicExpression": "(?<=sensor\/)(.*?)(?=\/connect)"
    }
  ]
```

在这种情况下以下消息有效：

```bash
mosquitto_pub -h YOUR_MQTT_BROKER_HOST -p YOUR_MQTT_BROKER_PORT -t "sensors/connect" -m '{"serialNumber":"SN-001"}'
mosquitto_pub -h YOUR_MQTT_BROKER_HOST -p YOUR_MQTT_BROKER_PORT -t "sensor/SN-001/connect" -m ''
```


### "disconnectRequest"说明

此配置部分是可选的。
本节提供的配置将用于从代理获取有关断开设备的信息。
如果您的设备仅与MQTT代理断开连接并等待commands/updates，则需要向网关发送消息，并通知设备已与代理断开连接。
 
**1. Name in a message from broker:**

| **参数**                 | **默认值**                     | **描述**                                                                                   |
|:-|:-|-
| topicFilter                   | **sensors/disconnect**                | Topic address on the broker, where the broker sends information about disconnected devices.       |
| deviceNameJsonExpression      | **${SerialNumber}**                 | JSON-path expression, for looking the new device name.                                            |
|---

**2. Name in topic address:**

| **参数**                 | **默认值**                     | **描述**                                                                                   |
|:-|:-|-
| topicFilter                   | **sensors/+/disconnect**              | Topic address on the broker, where the broker sends information about disconnected devices.       |
| deviceNameTopicExpression     | **(?<=sensor\/)(.\*?)(?=\/connect)**  | Regular expression for looking the device name in topic path.                                     |
|---

示例如下：

```json
  "disconnectRequests": [
    {
      "topicFilter": "sensors/disconnect",
      "deviceNameJsonExpression": "${SerialNumber}"
    },
    {
      "topicFilter": "sensor/+/disconnect",
      "deviceNameTopicExpression": "(?<=sensor\/)(.*?)(?=\/disconnect)"
    }
  ]
```

在这种情况下以下消息有效：

```bash
mosquitto_pub -h YOUR_MQTT_BROKER_HOST -p YOUR_MQTT_BROKER_PORT -t "sensors/disconnect" -m '{"serialNumber":"SN-001"}'
mosquitto_pub -h YOUR_MQTT_BROKER_HOST -p YOUR_MQTT_BROKER_PORT -t "sensor/SN-001/disconnect" -m ''
```

### "attributeUpdates"说明

此配置部分是可选的。
ThingsBoard允许供应设备属性，并从设备应用程序中获取其中的一些属性。
您可以将此视为设备的远程配置。您的设备能够从ThingsBoard请求共享属性。
有关更多详细信息，请参见[用户指南](/docs/user-guide/attributes/)。

**attributeRequests**”配置允许配置相应的属性请求和响应消息的格式。

| **参数**                 | **默认值**                                     | **描述**                                                                                    |
|:-|:-|-
| deviceNameFilter              | **SmartMeter.\***                                     | Regular expression device name filter, uses to determine, which function to execute.               |
| attributeFilter               | **uploadFrequency**                                   | Regular expression attribute name filter, uses to determine, which function to execute.            |
| topicExpression               | **sensor/${deviceName}/${attributeKey}**              | JSON-path expression uses for creating topic address to send a message.                            |
| valueExpression               | **{\\"${attributeKey}\\":\\"${attributeValue}\\"}**   | JSON-path expression uses for creating the message data that will send to topic.                   |
|---


示例如下：
```json
  "attributeUpdates": [
    {
      "deviceNameFilter": "SmartMeter.*",
      "attributeFilter": "uploadFrequency",
      "topicExpression": "sensor/${deviceName}/${attributeKey}",
      "valueExpression": "{\"${attributeKey}\":\"${attributeValue}\"}"
    }
  ]
```

##### 服务端RPC命令

ThingsBoard允许将[RPC命令](/docs/user-guide/rpc/)发送到直接或通过网关连接到ThingsBoard的设备。
 
本节提供的配置用于从ThingsBoard向设备发送RPC请求。

| **参数**                 | **默认值**                                                 | **描述**                                                                                                           |
|:-|:-|-
| deviceNameFilter              | **SmartMeter.\***                                                 | Regular expression device name filter, uses to determine, which function to execute.                                      |
| methodFilter                  | **echo**                                                          | Regular expression method name filter, uses to determine, which function to execute.                                      |
| requestTopicExpression        | **sensor/${deviceName}/request/${methodName}/${requestId}**       | JSON-path expression, uses for creating topic address to send RPC request.                                                |
| responseTopicExpression       | **sensor/${deviceName}/response/${methodName}/${requestId}**      | JSON-path expression, uses for creating topic address to subscribe for response message.                                  |
| responseTimeout               | **10000**                                                         | Value in milliseconds, if no response in this period after sending request, gateway will unsubscribe from response topic. |
| valueExpression               | **${params}**                                                     | JSON-path expression, uses for creating data for sending to broker.                                                       |
|---

{% capture methodFilterOptions %}
<br>
There are 2 options for RPC request:  
RPC请求有2个选项：
1. **With response** -- -如果配置中存在responseTopicExpression，则网关将尝试对其进行订阅并等待响应
2. **Without response** -- 如果在配置中不存在responseTopicExpression，则网关仅发送消息，而不会等待响应。
{% endcapture %}
{% include templates/info-banner.md content=methodFilterOptions %}

示例如下：

```json
  "serverSideRpc": [
    {
      "deviceNameFilter": ".*",
      "methodFilter": "echo",
      "requestTopicExpression": "sensor/${deviceName}/request/${methodName}/${requestId}",
      "responseTopicExpression": "sensor/${deviceName}/response/${methodName}/${requestId}",
      "responseTimeout": 10000,
      "valueExpression": "${params}"
    },
    {
      "deviceNameFilter": ".*",
      "methodFilter": "no-reply",
      "requestTopicExpression": "sensor/${deviceName}/request/${methodName}/${requestId}",
      "valueExpression": "${params}"
    }
  ]
```

您可以使用**deviceNameFilter**和**methodFilter**为不同的设备/方法应用不同的映射规则。
网关接收到从服务器到设备的RPC请求后，它将基于**requestTopicExpression**和**valueExpression**发布相应的消息。
如果您希望设备回复请求，则还应指定**responseTopicExpression**和**responseTimeout**。
网关将订阅“response”主题，并等待设备回复，直到检测到“responseTimeout”（以毫秒为单位）。

需要从服务器发送的RPC请求（rpc-request.json）的示例：

```json
{
  "method": "echo",
  "params": {
    "message": "Hello!"
  }
}
```

## 下一步

探索与ThingsBoard主要功能相关的指南：

 - [Data Visualization](/docs/user-guide/visualization/) - how to visualize collected data.
 - [Device attributes](/docs/user-guide/attributes/) - how to use device attributes.
 - [Telemetry data collection](/docs/user-guide/telemetry/) - how to collect telemetry data.
 - [Using RPC capabilities](/docs/user-guide/rpc/) - how to send commands to/from devices.
 - [Rule Engine](/docs/user-guide/rule-engine/) - how to use rule engine to analyze data from devices.
