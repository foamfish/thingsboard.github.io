mqtt broker上的授权配置参数详见下表。

|**参数**|**默认值**|**描述**|
|:-|:-|-
| caCert                   | **/etc/thingsboard-gateway/ca.pem**          | Path to CA file.                                               |
| privateKey               | **/etc/thingsboard-gateway/privateKey.pem**  | Path to private key file.                                      |
| cert                     | **/etc/thingsboard-gateway/certificate.pem** | Path to certificate file.
|---    

security示例如下：

```json
  "security":{
    "caCert": "/etc/thingsboard-gateway/ca.pem",
    "privateKey": "/etc/thingsboard-gateway/privateKey.pem",
    "cert": "/etc/thingsboard-gateway/certificate.pem"
  }
```
