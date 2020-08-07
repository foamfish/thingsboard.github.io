---
layout: docwithnav
title: 网关配置
description: ThingsBoard网关的安装结构和配置 

---


* TOC
{:toc}


## 目录结构

<<<<<<< HEAD
请在下面查看默认目录结构。
=======
Please see default directory structure below for daemon installation.  
>>>>>>> master

```text
/etc/thingsboard-gateway/config                   - Configuration folder.
    tb_gateway.yaml                               - Main configuration file for Gateway.
    logs.conf                                     - Configuration file for logging.
    modbus.json                                   - Modbus connector configuration.
    mqtt.json                                     - MQTT connector configuration.
    ble.json                                      - BLE connector configuration.
    opcua.json                                    - OPC-UA connector configuration.
    request.json                                  - Request connector configuration.
    can.json                                      - CAN connector configuration. 
    ... 

/var/lib/thingsboard_gateway/extensions           - Folder for custom connectors/converters.                      
    modbus                                        - Folder for Modbus custom connectors/converters.
    mqtt                                          - Folder for MQTT custom connectors/converters.
        __init__.py                               - Default python package file, needed for correct imports.
        custom_uplink_mqtt_converter.py           - Custom Mqtt converter example.
    ...
    opcua                                         - Folder for OPC-UA custom connectors/converters.
    ble                                           - Folder for BLE custom connectors/converters.
    request                                       - Folder for Request custom connectors/converters.
    can                                           - Folder for CAN custom connectors/converters.

/var/log/thingsboard-gateway                      - Logs folder
    connector.log                                 - Connector logs.
    service.log                                   - Main gateway service logs.
    storage.log                                   - Storage logs.
    tb_connection.log                             - Logs for connection to the ThingsBoard instance.
```
        
## 常规配置文件

用于连接到ThingsBoard平台实例和启用/禁用连接器的主要配置文件。

该配置指向ThingsBoard实例demo.thingsboard.io并使用

内存文件存储配置为最多存储100,000条记录。有4个不同的活动连接器。

如果您只想使用其中之一-只需移除所有其他连接器即可。

<br>
<details>
<summary>
<b>主要配置文件示例</b>
</summary>

{% highlight yaml %}

thingsboard:
  host: demo.thingsboard.io
  port: 1883
  security:
    accessToken: PUT_YOUR_ACCESS_TOKEN_HERE
storage:
  type: memory
  read_records_count: 100
  max_records_count: 100000
connectors:
  -
    name: MQTT Broker Connector
    type: mqtt
    configuration: mqtt.json

  -
    name: Modbus Connector
    type: modbus
    configuration: modbus.json

  -
    name: Modbus Connector
    type: modbus
    configuration: modbus_serial.json

  -
    name: OPC-UA Connector
    type: opcua
    configuration: opcua.json

  -
    name: BLE Connector
    type: ble
    configuration: ble.json

  -
    name: CAN Connector
    type: can
    configuration: can.json

  -
    name: Custom Serial Connector
    type: serial
    configuration: custom_serial.json
    class: CustomSerialConnector

{% endhighlight %}
<b><i>重要的空间标识</i></b>  
</details>

#### 配置文件节点

+ **thingsboard** -- 用于连接ThingsBoard平台的配置
  - *security* -- 加密和授权类型的配置
+ **storage** -- 配置本地存储来自设备的传入数据。
+ **connectors** -- 连接器阵列及其使用的配置。

#### 连接ThingsBoard

|**参数**             | **默认值**                            |   **描述**                                              |
|---                       |---                                           |---                                                             |
| ***thingsboard***        |                                              | Configuration for connection to server.                        |
| host                     | **127.0.0.1**                                | Hostname or ip address of ThingsBoard server.                  |
| port                     | **1883**                                     | Port of mqtt service on ThingsBoard server.                    |

###### security配置

{% capture securitytogglespec %}
Access Token<small>基本安全</small>%,%accessToken%,%templates/iot-gateway/security-accesstoken-config.md%br%
TLS + Access Token<small>高级安全</small>%,%tlsToken%,%templates/iot-gateway/security-tls-token-config.md%br%
TLS + Private Key<small>高级安全</small>%,%tls%,%templates/iot-gateway/security-tls-config.md{% endcapture %}

支持3种安全类型：

{% include content-toggle.html content-toggle-id="securityConfig" toggle-spec=securitytogglespec %}


#### storage配置

storage节点的配置提供了用于在将传入数据发送到ThingsBoard平台之前保存传入数据的配置。
  
支持两种保存方式：
1. **Memory**配置 - 接收到的数据保存到RAM内存中。
2. **File**配置 - 接收到的数据保存到硬盘驱动器。

{% capture storagetogglespec %}
Memory storage<br/> <small>(磁盘空间容量较低时使用)</small>%,%memory%,%templates/iot-gateway/storage-memory-config.md%br%
File storage<br/> <small>(将文件永久保存建议使用)</small>%,%file%,%templates/iot-gateway/storage-file-config.md{% endcapture %}

{% include content-toggle.html content-toggle-id="storageConfig" toggle-spec=storagetogglespec %}

#### 连接器配置

连接器部分配置中的配置，用于通过已实现的协议连接到设备。

本节中每个连接器的配置必须具有下表中的参数：
 
|**参数**|**默认值**|**描述**|
|:-|:-|- 
| name                     | **MQTT Broker Connector**                    | Name of connector to broker.                                                    |
| type                     | **mqtt**                                     | Type of connector, must be like a name of folder, contained configuration file. |
| configuration            | **mqtt.json**                                | Name of the file with configuration in config folder.*                          |
|---

\* -- 带有此配置文件的文件夹。  

配置文件中的节连接器可能与下面显示的有所不同，但是它们应具有以下结构：

```yaml
connectors:

  -
    name: MQTT Broker Connector
    type: mqtt
    configuration: mqtt.json

  -
    name: Modbus Connector
    type: modbus
    configuration: modbus.json

  -
    name: Modbus Connector
    type: modbus
    configuration: modbus_serial.json

  -
    name: OPC-UA Connector
    type: opcua
    configuration: opcua.json

  -
    name: BLE Connector
    type: ble
    configuration: ble.json

  -
    name: CAN Connector
    type: can
    configuration: can.json

  -
    name: Custom Serial Connector
    type: serial
    configuration: custom_serial.json
    class: CustomSerialConnector
```

**注意：**您可以同时使用多个相同的连接器，但应为其提供不同的名称和配置文件。

如果您需要其他类型的连接器，则可以使用[自定义指南](/docs/iot-gateway/custom/)或通过电子邮件给我们：<info@thingsboard.io>。
