---
layout: docwithnav
assignees:
- ashvayka
title: 在Docker(Windows)上安装ThingsBoard
description: 在Docker(Windows)上安装ThingsBoard

---

{% include templates/live-demo-banner.md %}

* TOC
{:toc}

本指南将帮助您在Windows上使用Docker安装和启动ThingsBoard。


## 先决条件

- [安装Docker Toolbox的Windows版本](https://docs.docker.com/toolbox/toolbox_install_windows/)

## 运行

根据所使用的数据库有三种类型的ThingsBoard单实例docker映像：

* [thingsboard/tb-postgres](https://hub.docker.com/r/thingsboard/tb-postgres/) - ThingsBoard与PostgreSQL数据库的单实例
    
    对于具有至少1GB内存的小型服务器的推荐选项。建议使用2-4GB。
* [thingsboard/tb-cassandra](https://hub.docker.com/r/thingsboard/tb-cassandra/) - 具有Cassandra数据库的ThingsBoard的单个实例。
    
    最高性能和推荐的选项，但至少需要4GB的RAM。建议使用8GB。
* [thingsboard/tb](https://hub.docker.com/r/thingsboard/tb/) - 具有嵌入式HSQLDB数据库的ThingsBoard的单个实例。
    
    **注意：** 不建议用于任何评估或生产用途，仅用于开发目的和自动测试。

在此说明中，将使用`thingsboard/tb-postgres`镜像。您可以选择其他具有不同数据库的镜像（请参见上文）。

Windows用户应将Docker托管卷用于ThingsBoard数据库。

在执行docker run命令之前创建docker卷(例如`mytb-data`):

打开"Docker Quickstart Terminal"。执行以下命令以创建Docker卷：

``` 
$ docker volume create mytb-data
$ docker volume create mytb-logs
```

执行以下命令以直接运行docker：
                                   
``` 
$ docker run -it -p 9090:9090 -p 1883:1883 -p 5683:5683/udp -v mytb-data:/data -v ~/mytb-logs:/var/log/thingsboard --name mytb --restart always thingsboard/tb-postgres
```

说明: 
    
- `docker run`              - 运行容器
- `-it`                     - 将终端会话与当前ThingsBoard进程输出连接
- `-p 9090:9090`            - 将本地端口9090映射至HTTP端口9090
- `-p 1883:1883`            - 将本地端口1883映射至MQTT端口1883    
- `-p 5683:5683`            - 将本地端口5683映射至COAP端口5683 
- `-v ~/.mytb-data:/data`   - 挂载`~/.mytb-data`目录为ThingsBoard数据目录
- `-v ~/.mytb-logs:/var/log/thingsboard`   - 挂载`~/.mytb-logs`目录为ThingsBoard日志目录
- `--name mytb`             - 本地别名
- `--restart always`        - 系统重启或出现故障后自动启动ThingsBoard
- `thingsboard/tb-postgres`          - docker镜象`thingsboard/tb-cassandra`或`thingsboard/tb`

从Windows计算机上的外部IP主机访问资源，请执行以下命令：

``` 
$ VBoxManage controlvm "default" natpf1 "tcp-port9090,tcp,,9090,,9090"  
$ VBoxManage controlvm "default" natpf1 "tcp-port1883,tcp,,1883,,1883"
$ VBoxManage controlvm "default" natpf1 "tcp-port5683,tcp,,5683,,5683"
```

执行完命令后您可以`http://{your-host-ip}:9090`在浏览器中打开(例如`http://localhost:9090`)。您应该看到ThingsBoard登录页面。

使用以下默认凭据：

- **系统管理员**: sysadmin@thingsboard.org / sysadmin
- **租户管理员**: tenant@thingsboard.org / tenant
- **客户**: customer@thingsboard.org / customer

您始终可以在帐户详情页面中更改每个帐户的密码。

## 分离、停止和启动

您可以使用`Ctrl-p` `Ctrl-q` - 与会话终端分离-容器将继续在后台运行.

要重新连接到终端（查看ThingsBoard日志），请运行:

分离容器：

```
$ docker attach mytb
```

停止容器：

```
$ docker stop mytb
```

启动容器：

```
$ docker start mytb
```

## 升级

为了更新到最新的镜像，请执行以下命令：:

```
$ docker pull thingsboard/tb-postgres
$ docker stop mytb
$ docker run -it -v mytb-data:/data --rm thingsboard/tb-postgres upgrade-tb.sh
$ docker rm mytb
$ docker run -it -p 9090:9090 -p 1883:1883 -p 5683:5683/udp -v ~/mytb-data:/data -v ~/mytb-logs:/var/log/thingsboard --name mytb --restart always thingsboard/tb-postgres
```

**注意**: 如果您使用不同的数据库，则在所有命令中将映像名称从更改为`thingsboard/tb-postgres` 至 `thingsboard/tb-cassandra` 或 `thingsboard/tb` correspondingly.
 
**注意**: 将主机的目录替换为`~/.mytb-data`容器创建期间使用的目录. 

## 故障排除

### DNS问题

**注意** 如果您发现与DNS问题相关的错误，例如

```bash
127.0.1.1:53: cannot unmarshal DNS message
```

您可以配置系统以使用[Google公用DNS服务](https://developers.google.com/speed/public-dns/docs/using#windows)


## 下一步

{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}
