编辑ThingsBoard配置文件

```bash 
sudo nano /etc/thingsboard/conf/thingsboard.conf
``` 
{: .copy-code}

将以下行添加到配置文件。

```bash
# Update ThingsBoard memory usage and restrict it to 256MB in /etc/thingsboard/conf/thingsboard.conf
export JAVA_OPTS="$JAVA_OPTS -Xms256M -Xmx256M"
```
{: .copy-code}