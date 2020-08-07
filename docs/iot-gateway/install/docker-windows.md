---
layout: docwithnav
title: Windows Docker下安装ThingsBoard网关

---

* TOC
{:toc}

本指南将帮助您在Windows上使用Docker安装和启动ThingsBoard网关


## 先决条件

- [安装 Windows Docker工具箱](https://docs.docker.com/toolbox/toolbox_install_windows/)

## 运行

单击Docker QuickStart图标，以启动预配置的Docker Toolbox终端。

确保您已使用命令行[登录](https://docs.docker.com/engine/reference/commandline/login/)到docker hub。

**执行以下命令以直接运行此docker：**

```
docker run -it -v %HOMEPATH%/tb-gateway/config:/thingsboard_gateway/config -v %HOMEPATH%/tb-gateway/extensions:/thingsboard_gateway/extensions -v %HOMEPATH%/tb-gateway/logs:/thingsboard_gateway/logs --name tb-gateway --restart always thingsboard/tb-gateway
```
{: .copy-code}

说明: 
    
<<<<<<< HEAD
- `docker run`              - 运行容器
- `-it`                     - 将终端会话与网关进程输出连接
- `-v ~/.tb-gateway/config:/etc/thingsboard-gateway/config`   - 挂载主机目录`~/.tb-gateway/config`至网关配置目录
- `-v ~/.tb-gateway/extensions:/var/lib/thingsboard_gateway/extensions`   - 挂载主机目录`~/.tb-gateway/extensions`至网关扩展目录
- `-v ~/.tb-gateway/logs:/var/log/thingsboard-gateway`   - 挂载主机目录`~/.tb-gateway/logs`至网关日志目录
- `--name tb-gateway`             - 网关在本机的别名
- `--restart always`        - 系统重启或出现故障后自动启动ThingsBoard。
- `thingsboard/tb-gateway`          - docker镜像
- `$HOME`   - 当前系统用户目录(`%HomePath%`)
=======
- `docker run`              - run this container
- `-it`                     - attach a terminal session with current Gateway process output
- `-v %HOMEPATH%/tb-gateway/config:/thingsboard_gateway/config`   - mounts the host's dir `%HOMEPATH%\tb-gateway\config` to Gateway config  directory
- `-v %HOMEPATH%/tb-gateway/extensions:/thingsboard_gateway/extensions`   - mounts the host's dir `%HOMEPATH%\tb-gateway\extensions` to Gateway extensions  directory
- `-v %HOMEPATH%/tb-gateway/logs:/thingsboard_gateway/logs`   - mounts the host's dir `%HOMEPATH%\tb-gateway\logs` to Gateway logs  directory
- `--name tb-gateway`             - friendly local name of this machine
- `--restart always`        - automatically start ThingsBoard in case of system reboot and restart in case of failure.
- `thingsboard/tb-gateway`          - docker image
- `$HOME`   - current user's home dir(`%HomePath%`)
>>>>>>> master

## 分离、停止和启动

您可以使用`Ctrl-p` `Ctrl-q` - 与会话终端分离-容器将继续在后台运行.

要重新连接到终端（查看网关日志），请运行:

分离容器：

```
docker attach tb-gateway
```
{: .copy-code}

停止容器：

```
docker stop tb-gateway
```
{: .copy-code}

启动容器：

```
docker start tb-gateway
```
{: .copy-code}

## 网关配置

停止容器：

```
docker stop tb-gateway
```
{: .copy-code}

使用配置文件打开目录：

`%HomePath%\tb-gateway\config`


**使用[本指南](/docs/iot-gateway/configuration/)将网关配置为与ThingsBoard实例一起使用：**


进行更改后启动容器：

```
docker start tb-gateway
```
{: .copy-code}

## Upgrading
## 升级

为了更新到最新的镜像，请执行以下命令：

```
$ docker pull thingsboard/tb-gateway
$ docker stop tb-gateway
$ docker rm tb-gateway
$ docker run -it -v $HOME/tb-gateway/config:/etc/thingsboard-gateway/config -v $HOME/tb-gateway/extensions:/var/lib/thingsboard_gateway/extensions -v $HOME/tb-gateway/logs:/var/log/thingsboard-gateway --name tb-gateway --restart always thingsboard/tb-gateway
```