---
layout: docwithnav
title: Linux Docker下安装ThingsBoard网关

---

* TOC
{:toc}

本指南将帮助您在Linux或Mac OS上使用Docker安装和启动ThingsBoard网关。


## 先决条件

- [安装Docker CE](https://docs.docker.com/engine/installation/)

## 运行

**执行以下命令以直接运行此docker：**

```
docker run -it -v ~/.tb-gateway/logs:/var/log/thingsboard-gateway -v ~/.tb-gateway/extensions:/var/lib/thingsboard_gateway/extensions -v ~/.tb-gateway/config:/etc/thingsboard-gateway/config --name tb-gateway --restart always thingsboard/tb-gateway
```
{: .copy-code}

说明: 
    
- `docker run`              - 运行容器
- `-it`                     - 将终端会话与网关进程输出连接
- `-v ~/.tb-gateway/config:/etc/thingsboard-gateway/config`   - 挂载主机目录`~/.tb-gateway/config`至网关配置目录
- `-v ~/.tb-gateway/extensions:/var/lib/thingsboard_gateway/extensions`   - 挂载主机目录`~/.tb-gateway/extensions`至网关扩展目录
- `-v ~/.tb-gateway/logs:/var/log/thingsboard-gateway`   - 挂载主机目录`~/.tb-gateway/logs`至网关日志目录
- `--name tb-gateway`             - 网关在本机的别名
- `--restart always`        - 系统重启或出现故障后自动启动ThingsBoard。
- `thingsboard/tb-gateway`          - docker镜像

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

**使用[本指南](/docs/iot-gateway/configuration/)将网关配置为与ThingsBoard实例一起使用：**

进行更改后启动容器：

```
docker start tb-gateway
```
{: .copy-code}

## 升级

为了更新到最新的镜像，请执行以下命令：

```
$ docker pull thingsboard/tb-gateway
$ docker stop tb-gateway
$ docker rm tb-gateway
$ docker run -it -v ~/.tb-gateway/logs:/var/log/thingsboard-gateway -v ~/.tb-gateway/extensions:/var/lib/thingsboard_gateway/extensions -v ~/.tb-gateway/config:/etc/thingsboard-gateway/config --name tb-gateway --restart always thingsboard/tb-gateway
```
