现在开始启动ThingsBoard服务！

以管理员身份打开命令提示符并执行以下命令：

```shell
net start thingsboard
```

执行输出结果:

```text
The ThingsBoard Server Application service is starting.
The ThingsBoard Server Application service was started successfully.
```

您可以执行以下命令重新启动ThingsBoard服务：

```shell
net stop thingsboard
net start thingsboard
```

启动后您将可以使用以下链接打开Web UI：

```bash
http://localhost:8080/
```

如果在安装脚本的执行过程中指定了*-loadDemo*则可以使用以下默认凭据：

- **系统管理员**: sysadmin@thingsboard.org / sysadmin
- **租户管理员**: tenant@thingsboard.org / tenant
- **客户**: customer@thingsboard.org / customer

您始终可以在帐户详情页面中更改每个帐户的密码。