
accessToken是一种安全配置要获得它，您应该登录ThingsBoard平台并转到DEVICE标签，按加号图标填写值并选中"Is gateway"选项打开此设备，然后按“COPY ACCESS TOKEN”按钮，然后将默认值替换为您的值。


|**参数**|**默认值**|**描述**|
|:-|:-|-
| accessToken              | **FUH2Fonov6eajSHi0Zyw**                     | Access token for the gateway from ThingsBoard server.|
|---

配置文件中的“安全性”节点如下所示：

```yaml
...
  security:
    accessToken: FUH2Fonov6eajSHi0Zyw
...
```
