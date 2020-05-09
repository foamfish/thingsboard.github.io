---
layout: docwithnav
assignees:
- ashvayka
title: 入门
description: 开始使用ThingsBoard平台

---

* TOC
{:toc}


## 介绍

本教程演示的ThingsBoard的基本用法，你将从中学到如下功能：

 - 创建资产和设备;
 - 定义资产和设备间的关系;
 - 发送设备数据至ThingsBoard;
 - 创建实时仪表板;
 - 定义阀值并报警;
 - 将警报通过邮件发送.
 
我们将展示如何监视物体不同部分的温度，如何在温度超过特定阈值时发出警报以及如何可视化收集的数据和警报。
 
## 视频教程
 
我们建议您观看以下视频教程。为了方便起见，下面列出了本教程中使用的所有资源。
 
&nbsp; 
  
<div id="video">  
    <div id="video_wrapper">
        <iframe src="http://124.161.226.23:8081/Content/tb/helloworld/5.mp4" frameborder="0" allowfullscreen></iframe>
    </div>
</div>

## 教程资源

#### 在线实例和安装指南

如果你无法运行实例请通过[在线实例](https://demo.thingsboard.io/signup) 或者
[安装指南](/docs/user-guide/install/installation-options/)
进行学习。 

如果你安装在本地计算机上并使用 **--loadDemo** 进行初始化则使用默认的用户名及密码，你可以通过[此连接](/docs/samples/demo-account/)进行查看。

#### 发送设备数据

我们使用HTTP，MQTT或CoAP协议从你的本地PC发送数据。请查看[设备连接](/docs/guides#AnchorIDConnectYourDevice)指南以获取所有的连接解决方​​案和选项以及[硬件示例](/docs/guides#AnchorIDHardwareSamples)，以了解如何将各种硬件平台连接到ThingsBoard

#### 客户端安装

使用如下命令安装 HTTP (cURL), MQTT (Mosquitto 或者 MQTT.js) 或者 CoAP (CoAP.js) 。

{% capture tabspec %}mqtt-client
ClientA,cURL (Windows),shell,resources/curl-win.sh,/docs/getting-started-guides/resources/curl-win.sh
ClientB,cURL (macOS),shell,resources/curl-macos.sh,/docs/getting-started-guides/resources/curl-macos.sh
ClientC,cURL (Ubuntu),shell,resources/curl-ubuntu.sh,/docs/getting-started-guides/resources/curl-ubuntu.sh
ClientD,MQTT.js,shell,resources/node-mqtt.sh,/docs/getting-started-guides/resources/node-mqtt.sh
ClientE,Mosquitto (Ubuntu),shell,resources/mosquitto-ubuntu.sh,/docs/getting-started-guides/resources/mosquitto-ubuntu.sh
ClientF,Mosquitto (macOS),shell,resources/mosquitto-macos.sh,/docs/getting-started-guides/resources/mosquitto-macos.sh
ClientG,CoAP.js,shell,resources/node-coap.sh,/docs/getting-started-guides/resources/node-coap.sh{% endcapture %}
{% include tabs.html %}

#### 视频教程中使用cURL

如果你已经安装cURL工具则此命令适用于Windows, Ubuntu 和 macOS。 

```bash
# Please replace $HOST_NAME and $ACCESS_TOKEN with corresponding values.
curl -v -X POST -d "{\"temperature\": 25}" $HOST_NAME/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"

# For example, $HOST_NAME in case of live demo server:
curl -v -X POST -d "{\"temperature\": 25}" https://demo.thingsboard.io/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"

# For example, $HOST_NAME in case of local installation:
curl -v -X POST -d "{\"temperature\": 25}" http://localhost:8080/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
```

#### 生成脚本

```javascript
var msg = { temperature: +(Math.random()*5 + 25).toFixed(1)};
var metadata = {};
var msgType = "POST_TELEMETRY_REQUEST";

return { msg: msg, metadata: metadata, msgType: msgType };
```

#### 规则引擎指南

[规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/) - 了解规则引擎的基础知识和架构。

[规则引擎指南](/docs/guides#AnchorIDDataProcessing) - 解如何使用ThingsBoard规则引擎。

#### 邮件设置

使用本[指南](/docs/user-guide/ui/mail-settings/#step-31-sendgrid-configuration-example)来配置SendGrid或使用任何其他可用的SMTP服务器。

#### 其他测试数据文件

**创建一个文件夹** 来存储本教程的所有必需文件。下载到此文件夹或创建以下数据文件:

 - {% include ghlink.html content='**attributes-data.json**' ghlink='/docs/getting-started-guides/resources/attributes-data.json' %} - 包含两个设备属性值: 固件版本和序列号。
 - {% include ghlink.html content='**telemetry-data.json**' ghlink='/docs/getting-started-guides/resources/telemetry-data.json' %} - 包含三个时间序列值: 温度，湿度和活动标记。
 
请注意，此文件中的数据基本上是键值格式。您可以使用自己的键和值。有关更多详细信息，请参见[MQTT](/docs/reference/mqtt-api/#key-value-format)，[CoAP](/docs/reference/coap-api/#key-value-format) 或[HTTP](/docs/reference/http-api/#key-value-format)协议参考。

{% capture tabspec %}data
A,attributes-data.json,json,resources/attributes-data.json,/docs/getting-started-guides/resources/attributes-data.json
B,telemetry-data.json,json,resources/telemetry-data.json,/docs/getting-started-guides/resources/telemetry-data.json{% endcapture %}
{% include tabs.html %}

#### 使用MQTT，CoAP或HTTP推送数据

根据使用的客户端下载对应的文件至文件夹中
 - **MQTT.js (MQTT)**
   - {% include ghlink.html content='mqtt-js.sh' ghlink='/docs/getting-started-guides/resources/mqtt-js.sh' %} (Ubuntu & MacOS) or {% include ghlink.html content='mqtt-js.bat' ghlink='/docs/getting-started-guides/resources/mqtt-js.bat' %} (Windows)
   - {% include ghlink.html content='publish.js' ghlink='/docs/getting-started-guides/resources/publish.js' %}
 - **Mosquitto (MQTT)**
   - {% include ghlink.html content='mosquitto.sh' ghlink='/docs/getting-started-guides/resources/mosquitto.sh' %}
 - **CoAP.js (CoAP)**
   - {% include ghlink.html content='coap-js.sh' ghlink='/docs/getting-started-guides/resources/coap-js.sh' %}
 - **cURL (HTTP)**
   - {% include ghlink.html content='curl.sh' ghlink='/docs/getting-started-guides/resources/curl.sh' %}

如果您使用的是Shell脚本（* .sh），请确保该脚本是可执行的:

```shell
chmod +x *.sh
```

在执行脚本之前，请不要忘记执行以下操作: 

 - 替换 **$ACCESS_TOKEN** 成 **设备凭证**。
 - 替换 **$THINGSBOARD_HOST** 成 **127.0.0.1** (本地电脑) or **demo.thingsboard.io** (在线实例)。

最后，执行相应的* .sh或* .bat脚本将数据推送到服务器。

以下是包含所提供脚本内容的选项卡。
 
{% capture tabspec %}mqtt-telemetry-upload
A,MQTT.js (Ubuntu & MacOS),shell,resources/mqtt-js.sh,/docs/getting-started-guides/resources/mqtt-js.sh
B,MQTT.js (Windows),shell,resources/mqtt-js.bat,/docs/getting-started-guides/resources/mqtt-js.bat
C,publish.js,shell,resources/publish.js,/docs/getting-started-guides/resources/publish.js
D,Mosquitto (MQTT),shell,resources/mosquitto.sh,/docs/getting-started-guides/resources/mosquitto.sh
E,CoAP.js (CoAP),shell,resources/coap-js.sh,/docs/getting-started-guides/resources/coap-js.sh
F,cURL (HTTP),shell,resources/curl.sh,/docs/getting-started-guides/resources/curl.sh{% endcapture %}
{% include tabs.html %}

## ThingsBoard社区版教程
 
 <div id="video">  
     <div id="video_wrapper">
         <iframe src="http://124.161.226.23:8081/Content/tb/helloworld/6.mp4" frameborder="0" allowfullscreen></iframe>
     </div>
 </div>
 <p></p>
 
## 下一步

{% assign currentGuide = "GettingStartedGuides" %}{% include templates/guides-banner.md %}

## 意见


通过在 **[github](https://github.com/thingsboard/thingsboard)** 上关注ThingsBoard来帮助我们推广它。如果您对此样本有任何疑问，请将其发布在 **[论坛](https://groups.google.com/forum/#!forum/thingsboard)**。