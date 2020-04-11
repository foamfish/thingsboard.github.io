---
layout: docwithnav
assignees:
- vsosliuk
title: 基于SSL的MQTT认证
description: 使用基于SSL的的MQTT认证协议启动ThingsBoard以连接你的IoT设备和项目。


---

* TOC
{:toc}

ThingsBoard提供了基于SSL认证运行MQTT服务的功能，同时支持单向和双向SSL。你可以使用有效的证书或生成自签名的SSL证书并将其添加到密钥库来启用SSL功能。你需要在　**thingsboard.yml**　文件中指定密钥库信息。请参阅下面的有关如何生成SSL证书并在你的ThingsBoard安装并使用。

### 生成自签名证书

**注意** 此步骤必须在基于Linux的操作中安装java.

从官方ThingsBoard仓库中下载[**server.keygen.sh**](https://raw.githubusercontent.com/thingsboard/thingsboard/master/tools/src/main/shell/server.keygen.sh)到你的工作目录。

将[**keygen.properties**](https://raw.githubusercontent.com/thingsboard/thingsboard/master/tools/src/main/shell/keygen.properties)文件下载到你的工作目录，并填充所需的值。

例如：

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

CLIENT_TRUSTSTORE="client_truststore"
CLIENT_KEY_ALIAS="clientalias"
CLIENT_FILE_PREFIX="mqttclient"
```

where 

 - **DOMAIN_SUFFIX** - 对应于证书的**CN**值。必须与目标服务器域相对应（允许使用通配符）。默认为当前主机名。
 - **ORGANIZATIONAL_UNIT** - 对应于证书的 **OU** 值。
 - **ORGANIZATION** -  -对应于证书的 **O** 值。
 - **CITY** -  -对应于证书的 **L** 值。
 - **STATE_OR_PROVINCE** - 对应于证书的 **ST** 值。
 - **TWO_LETTER_COUNTRY_CODE** - 对应于证书的 **C** 值。
 - **SERVER_KEYSTORE_PASSWORD** - 服务器密钥库密码
 - **SERVER_KEY_PASSWORD** - 服务器密钥密码。可能与SERVER_KEYSTORE_PASSWORD不相同。
 - **SERVER_KEY_ALIAS** - 服务器密钥别名。在密钥库中必须唯一
 - **SERVER_FILE_PREFIX** - 所有与服务器密钥生成有关的输出文件的前缀
 - **SERVER_KEYSTORE_DIR** - 可以选择复制密钥的默认位置。 可以由**server.keygen.sh** 脚本中的-d选项覆盖，或在脚本运行时手动输入

其余值对于服务器密钥库的生成并不重要

要运行服务器密钥库生成，请使用以下命令。
 
```bash
chmod +x server.keygen.sh
sudo ./server.keygen.sh
```

您可以不带任何参数运行此脚本或者可以指定以下可选参数:

 - **-c \| --copy** - 指定是否将密钥库复制到服务器目录。默认为 **true**
 - **-d \| --dir** - 服务器密钥库目录，将在其中复制生成的**SERVER_FILE_PREFIX**.jks密钥库文件。如果指定，则覆盖属性文件中的值
 - **-p \| --props \| --properties** - 指定属性文件的相对路径。默认为 **./keygen.properties** 

该脚本将使用指定的配置运行keytool。它将生成以下输出文件：

 - **SERVER_FILE_PREFIX.jks** - Java密钥库文件。这是ThingsBoard MQTT服务将使用的文件
 - **SERVER_FILE_PREFIX.cer** - 服务器公用密钥文件。然后将其导入到客户端的.jks密钥库文件中。
 - **SERVER_FILE_PREFIX.pub.pem** - **PEM** 格式的服务器公共密钥，然后可以用作密钥存储或由非Java客户端导入。

如果您指定不复制密钥库文件，则将其手动上传到服务器的类路径中的目录。您可能要修改密钥库文件的所有者和权限：

```bash
sudo chmod 400 /etc/thingsboard/conf/mqttserver.jks
sudo chown thingsboard:thingsboard /etc/thingsboard/conf/mqttserver.jks
```

### 服务器配置

找到您的 **thingsboard.yml** 文件，并取消注释"＃取消注释以下行以为MQTT启用ssl之后的行"：

```bash
# MQTT server parameters
mqtt:
  bind_address: "${MQTT_BIND_ADDRESS:0.0.0.0}"
  bind_port: "${MQTT_BIND_PORT:8883}"
  adaptor: "${MQTT_ADAPTOR_NAME:JsonMqttAdaptor}"
  timeout: "${MQTT_TIMEOUT:10000}"
# Uncomment the following lines to enable ssl for MQTT
  ssl:
    key_store: mqttserver.jks
    key_store_password: server_ks_password
    key_password: server_key_password
    key_store_type: JKS
```

您可能还希望将 **mqtt.bind_port** 更改为8883，基于SSL认证的MQTT推荐使用。

此 **key_store** 属性必须指向.jks文件位置。**key_store_password**和**key_password**必须与生成密钥库时使用的相同。

**注意:** ThingsBoard也支持 **.p12** 密钥库。如果是这种情况，请将**key_store_type**e值设置为 **'PKCS12'**

设置这些值之后，启动或重新启动Thingsboard服务器。

## 客户实例

请参阅以下资源:
 - [设备身份认证](/docs/user-guide/device-credentials/)
 - [单向SSL令牌身份认证](/docs/user-guide/access-token/)
 - [X.509身份双向认证](/docs/user-guide/certificates/)
