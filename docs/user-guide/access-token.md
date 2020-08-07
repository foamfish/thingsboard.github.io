---
layout: docwithnav
assignees:
- vsosliuk
title: 令牌认证
description: ThingsBoard令牌认证。

---

设备默认的身份验证是访问令牌的方式。
在ThingsBoard中创建设备后将生成默认访问令牌。
为了使用基于访问令牌的身份验证将设备连接到服务器客户端必须在请求URL（用于HTTP和CoAP）或MQTT connect消息中的用户名中指定访问令牌。有关更多详细信息，请参见[支持的协议 ](/docs/reference/protocols/)API。

### 单向的MQTT SSL
 
单向SSL身份验证是一种标准的身份验证模式，你的客户端设备使用服务器证书来验证服务器的身份。为了运行单向MQTT SSL，服务器证书链应由授权的CA签名，否则客户端必须将自签名的服务器证书（.cer或.pem）导入其信任库。否则，连接将失败，并显示“未知CA”错误。

#### 在Python客户端运行单向 MQTT SSL

下面的示例演示如何连接到使用自签名证书的ThingsBoard MQTT服务器。您将需要具有PEM格式的服务器证书的公钥。有关服务器端配置的更多详细信息，请[参见](/docs/user-guide/mqtt-over-ssl/#self-signed-certificate-generation)以下说明。

<<<<<<< HEAD
下载Python客户端示例[**one-way-ssl-mqtt-client.py**](https://raw.githubusercontent.com/thingsboard/thingsboard/master/tools/src/main/python/one-way-ssl-mqtt-client.py)。指定访问令牌和服务器证书公钥的路径
=======
Download Python client example [**one-way-ssl-mqtt-client.py**](/docs/user-guide/resources/mqtt-over-ssl/one-way-ssl-mqtt-client.py).
Specify your access token and path to the public key of the server certificate.
>>>>>>> master

```python
# Some code omitted

client.tls_set(ca_certs="mqttserver.pub.pem", certfile=None, keyfile=None, cert_reqs=ssl.CERT_REQUIRED, tls_version=ssl.PROTOCOL_TLSv1, ciphers=None);

client.username_pw_set("accessToken")

# Some code omitted
```

<<<<<<< HEAD
**注意** 脚本使用 **8883** mqtt端口和需要paho mqtt库你可以使用以下命令进行安装：**pip install paho-mqtt**
=======
**Note** Script uses **8883** mqtt port and requires paho mqtt library that you can install using the following command: **pip3 install paho-mqtt**
>>>>>>> master
 
运行脚本:

{% capture tabspec %}mqtt-ssl-configuration-keygen
A,python one-way-ssl-mqtt-client.py,shell,resources/mqtt-ssl-configuration-run-onewaysslmqttclient.sh,/docs/user-guide/resources/mqtt-ssl-configuration-run-onewaysslmqttclient.sh{% endcapture %}
{% include tabs.html %}         

如果一切配置正确则输出应为:

{% capture tabspec %}mqtt-ssl-configuration-output
A,onewaysslmqttclient.py output,shell,resources/mqtt-ssl-configuration-onewaysslmqttclient-output.txt,/docs/user-guide/resources/mqtt-ssl-configuration-onewaysslmqttclient-output.txt{% endcapture %}
{% include tabs.html %}