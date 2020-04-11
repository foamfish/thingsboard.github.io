---
layout: docwithnav
assignees:
- vsosliuk
title: X.509凭据认证
description: ThingsBoard基于X.509凭据认证IoT设备和项目。

---
    
X.509基于证书的身份验证用于双向SSL连接。在这种情况下证书本身就是客户端的ID，因此不再需要访问令牌。

以下说明将描述如何生成客户端证书以及如何通过SSL连接到运行MQTT的服务器。您将需要具有PEM格式的服务器证书的公钥。有关服务器端配置的更多详细信息请[参见](/docs/user-guide/mqtt-over-ssl/#self-signed-certificate-generation)以下说明。

#### 更新keygen.properties文件
 
打 keygen.properties文件并根据需要更新对应内容:

```bash
DOMAIN_SUFFIX="$(hostname)"
ORGANIZATIONAL_UNIT=ThingsBoard
ORGANIZATION=ThingsBoard
CITY=San Francisco
STATE_OR_PROVINCE=CA
TWO_LETTER_COUNTRY_CODE=US

SERVER_KEYSTORE_PASSWORD=server_ks_password
SERVER_KEY_PASSWORD=server_key_password

SERVER_KEY_ALIAS="serveralias"
SERVER_FILE_PREFIX="mqttserver"
SERVER_KEYSTORE_DIR="/etc/thingsboard/conf/"

CLIENT_KEYSTORE_PASSWORD=password
CLIENT_KEY_PASSWORD=password

CLIENT_KEY_ALIAS="clientalias"
CLIENT_FILE_PREFIX="mqttclient"
```

#### 运行客户端密钥生成脚本

下载并启动[**client.keygen.sh**](https://raw.githubusercontent.com/thingsboard/thingsboard/master/tools/src/main/shell/client.keygen.sh)脚本。

```bash
chmod +x client.keygen.sh
./client.keygen.sh
```

该脚本输出以下文件:

 - **CLIENT_FILE_PREFIX.jks** - 导入了服务器证书的Java密钥库文件
 - **CLIENT_FILE_PREFIX.nopass.pem** - 非Java客户端使用的PEM格式的客户端证书文件
 - **CLIENT_FILE_PREFIX.pub.pem** - 客户端公共钥

#### 将客户端公钥设置为设备凭据

**ThingsBoard Web UI -> Devices -> Your Device -> Device Credentials**. 选择**X.509凭据**, 插入**CLIENT_FILE_PREFIX.pub.pem** 文件内容并保存或者通过REST API进行相同操作。

#### 运行双向MQTT SSL Python客户端

下载Python客户端示例[**two-way-ssl-mqtt-client.py**](https://raw.githubusercontent.com/thingsboard/thingsboard/master/tools/src/main/python/two-way-ssl-mqtt-client.py)。
指定你的客户端证书和服务器证书公钥的路径。

```python
# Some code omitted

client.tls_set(ca_certs="mqttserver.pub.pem", certfile="mqttclient.nopass.pem", keyfile=None, cert_reqs=ssl.CERT_REQUIRED,
                       tls_version=ssl.PROTOCOL_TLSv1, ciphers=None);

# Some code omitted
```

**注意** 脚本使用 **8883** mqtt端口和需要paho mqtt库你可以使用以下命令进行安装：**pip install paho-mqtt**

运行脚本:

{% capture tabspec %}mqtt-ssl-configuration-twoway
A,python twowaysslmqttclient.py,shell,resources/mqtt-ssl-configuration-run-twowaysslmqttclient.sh,/docs/user-guide/resources/mqtt-ssl-configuration-run-twowaysslmqttclient.sh{% endcapture %}
{% include tabs.html %}  

如果一切配置正确，则输出应为：

{% capture tabspec %}mqtt-ssl-configuration-output-twoway
A,twowaysslmqttclient.py output,shell,resources/mqtt-ssl-configuration-twowaysslmqttclient-output.txt,/docs/user-guide/resources/mqtt-ssl-configuration-twowaysslmqttclient-output.txt{% endcapture %}
{% include tabs.html %}


To run Java client, import **CLIENT_FILE_PREFIX.jks** file as follows:

{% capture tabspec %}mqtt-ssl-java-twoway-client
A,MqttSslClient.java,java,resources/MqttSslClient.java,/docs/user-guide/resources/MqttSslClient.java{% endcapture %}
{% include tabs.html %}