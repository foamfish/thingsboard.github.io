---
layout: docwithnav
assignees:
- ashvayka
title: 在Docker(Linux或Mac OS)上安装ThingsBoard
description: 在Docker(Linux或Mac OS)上安装ThingsBoard

---

{% include templates/live-demo-banner.md %}

* TOC
{:toc}

本指南将帮助您在Linux或Mac OS上使用Docker安装和启动ThingsBoard。


## 先决条件

- [Install Docker CE](https://docs.docker.com/engine/installation/)

<<<<<<< HEAD
## 运行
=======
- [Install Docker Compose](https://docs.docker.com/compose/install/)

## Running
>>>>>>> master

根据所使用的数据库有三种类型的ThingsBoard单实例docker映像：

* [thingsboard/tb-postgres](https://hub.docker.com/r/thingsboard/tb-postgres/) - ThingsBoard与PostgreSQL数据库的单实例
    
    对于具有至少1GB内存的小型服务器的推荐选项。建议使用2-4GB。
* [thingsboard/tb-cassandra](https://hub.docker.com/r/thingsboard/tb-cassandra/) - 具有Cassandra数据库的ThingsBoard的单个实例。
    
    最高性能和推荐的选项，但至少需要4GB的RAM。建议使用8GB。
* [thingsboard/tb](https://hub.docker.com/r/thingsboard/tb/) - 具有嵌入式HSQLDB数据库的ThingsBoard的单个实例。
    
<<<<<<< HEAD
    **注意：** 不建议用于任何评估或生产用途，仅用于开发目的和自动测试。

在此说明中，将使用`thingsboard/tb-postgres`镜像。您可以选择其他具有不同数据库的镜像（请参见上文）。

执行以下命令以直接运行docker：
=======
    **Note:** Not recommended for any evaluation or production usage and is used only for development purposes and automatic tests. 
    
In this instruction `thingsboard/tb-postgres` image will be used. You can choose any other images with different databases (see above).
>>>>>>> master

## Choose ThingsBoard queue service

{% include templates/install/install-queue.md %}

{% capture contenttogglespecqueue %}
In Memory <small>(built-in and default)</small>%,%inmemory%,%templates/install/docker-queue-in-memory.md%br%
Kafka <small>(recommended for on-prem, production installations)</small>%,%kafka%,%templates/install/docker-queue-kafka.md%br%
AWS SQS <small>(managed service from AWS)</small>%,%aws-sqs%,%templates/install/docker-queue-aws-sqs.md%br%
Google Pub/Sub <small>(managed service from Google)</small>%,%pubsub%,%templates/install/docker-queue-pub-sub.md%br%
Azure Service Bus <small>(managed service from Azure)</small>%,%service-bus%,%templates/install/docker-queue-service-bus.md%br%
RabbitMQ <small>(for small on-prem installations)</small>%,%rabbitmq%,%templates/install/docker-queue-rabbitmq.md{% endcapture %}

{% include content-toggle.html content-toggle-id="ubuntuThingsboardQueue" toggle-spec=contenttogglespecqueue %} 

说明: 
    
<<<<<<< HEAD
- `docker run`              - 运行容器
- `-it`                     - 将终端会话与当前ThingsBoard进程输出连接
- `-p 9090:9090`            - 将本地端口9090映射至HTTP端口9090
- `-p 1883:1883`            - 将本地端口1883映射至MQTT端口1883    
- `-p 5683:5683`            - 将本地端口5683映射至COAP端口5683 
- `-v ~/.mytb-data:/data`   - 挂载`~/.mytb-data`目录为ThingsBoard数据目录
- `-v ~/.mytb-logs:/var/log/thingsboard`   - 挂载`~/.mytb-logs`目录为ThingsBoard日志目录
- `--name mytb`             - 本地别名
- `--restart always`        - 系统重启或出现故障后自动启动ThingsBoard
- `thingsboard/tb-postgres`          - docker镜像`thingsboard/tb-cassandra`或`thingsboard/tb`
    
执行完命令后您可以`http://{your-host-ip}:9090`在浏览器中打开(例如`http://localhost:9090`)。您应该看到ThingsBoard登录页面。
=======
- `8080:9090`            - connect local port 8080 to exposed internal HTTP port 9090
- `1883:1883`            - connect local port 1883 to exposed internal MQTT port 1883    
- `5683:5683`            - connect local port 5683 to exposed internal COAP port 5683 
- `~/.mytb-data:/data`   - mounts the host's dir `~/.mytb-data` to ThingsBoard DataBase data directory
- `~/.mytb-logs:/var/log/thingsboard`   - mounts the host's dir `~/.mytb-logs` to ThingsBoard logs directory
- `mytb`             - friendly local name of this machine
- `restart: always`        - automatically start ThingsBoard in case of system reboot and restart in case of failure.
- `image: thingsboard/tb-postgres`          - docker image, can be also `thingsboard/tb-cassandra` or `thingsboard/tb`


Before starting Docker container run following commands to create a directory for storing data and logs and then change its owner to docker container user,
to be able to change user, **chown** command is used, which requires sudo permissions (command will request password for a sudo access):

```
$ mkdir -p ~/.mytb-data && sudo chown -R 799:799 ~/.mytb-data
$ mkdir -p ~/.mytb-logs && sudo chown -R 799:799 ~/.mytb-logs
```

**NOTE**: replace directory `~/.mytb-data` and `~/.mytb-logs` with directories you're planning to used in `docker-compose.yml`. 

Execute the following command to up this docker compose directly:

**NOTE**: For running docker compose commands you have to be in a directory with docker-compose.yml file. 

```
docker-compose pull
docker-compose up
```
{: .copy-code}

    
After executing this command you can open `http://{your-host-ip}:8080` in you browser (for ex. `http://localhost:8080`). You should see ThingsBoard login page.
Use the following default credentials:
>>>>>>> master

使用以下默认凭据：

- **系统管理员**: sysadmin@thingsboard.org / sysadmin
- **租户管理员**: tenant@thingsboard.org / tenant
- **客户**: customer@thingsboard.org / customer

您始终可以在帐户详情页面中更改每个帐户的密码。

## 分离、停止和启动

您可以使用`Ctrl-p` `Ctrl-q` - 与会话终端分离-容器将继续在后台运行.

要重新连接到终端（查看ThingsBoard日志），请运行:

<<<<<<< HEAD
分离容器：
=======
In case of any issues you can examine service logs for errors.
For example to see ThingsBoard node logs execute the following command:
>>>>>>> master

```
docker-compose logs -f mytb
```
{: .copy-code}

停止容器：

```
docker-compose stop
```
{: .copy-code}

启动容器：

```
docker-compose start
```
{: .copy-code}

## 升级

为了更新到最新的镜像，请执行以下命令：:

```
docker pull thingsboard/tb-postgres
docker-compose stop
docker run -it -v ~/.mytb-data:/data --rm thingsboard/tb-postgres upgrade-tb.sh
docker-compose rm mytb
docker-compose up
```
{: .copy-code}

**注意**: 如果您使用不同的数据库，则在所有命令中将映像名称从更改为`thingsboard/tb-postgres` 至 `thingsboard/tb-cassandra` 或 `thingsboard/tb` correspondingly.
 
**注意**: 将主机的目录替换为`~/.mytb-data`容器创建期间使用的目录. 

<<<<<<< HEAD
## 故障排除
=======
**NOTE**: if you have used one database and want to try another one, then remove the current docker container using `docker-compose rm` command and use different directory for `~/.mytb-data` in `docker-compose.yml`.
 

## Troubleshooting
>>>>>>> master

### DNS问题

**注意** 如果您发现与DNS问题相关的错误，例如

```bash
127.0.1.1:53: cannot unmarshal DNS message
```

您可以将系统配置为使用Google公共DNS服务器。请参阅相应的[Linux](https://developers.google.com/speed/public-dns/docs/using#linux)和[Mac OS](https://developers.google.com/speed/public-dns/docs/using#mac_os)说明。


## 下一步

{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}
