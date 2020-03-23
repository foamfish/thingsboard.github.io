下表描述了在ThingsBoard平台上配置网关授权的相关参数。

|**参数**|**默认值**|**描述**|
|:-|:-|-
| caCert                   | **/etc/thingsboard-gateway/ca.pem**          | Path to CA file.                                               |
| privateKey               | **/etc/thingsboard-gateway/privateKey.pem**  | Path to private key file.                                      |
| cert                     | **/etc/thingsboard-gateway/certificate.pem** | Path to certificate file.
|---    

配置文件中的安全节点如下所示：

```yaml
  security:
    privateKey: /etc/thingsboard-gateway/privateKey.pem
    caCert: /etc/thingsboard-gateway/ca.pem
    cert: /etc/thingsboard-gateway/certificate.pem
```
