---
layout: docwithnav
assignees:
- ashvayka
title: 设备连接状态
description: IoT设备状态和连接检查
redirect_from: "/docs/user-guide/rule-engine-2-0/tutorials/device-online-offline/"

---

* TOC
{:toc}

## 功能概述

ThingsBoard设备状态服务负责监视设备连接状态并触发推送到[**规则引擎的**](/docs/user-guide/rule-engine-2-0/re-getting-started/)设备连接事件。平台开发者可以对相关事件出相关处理。

支持事件如下:
 
 - **Connect event** - 设备连接到ThingsBoard时触发。基于MQTT的会话传输和HTTP请求传输同时连接事件将在每一个HTTP请求上触发。
 - **Disconnect event** - 设备与ThingsBoard断开连接时触发。基于MQTT的会话传输和HTTP请求传输同时连接事件将在每一个HTTP请求上触发。
 - **Activity event** - 通过属性(attribute update)或者rpc命令推送遥测数据。
 - **Inactivity event** - 当设备指定时间内不活动时触发。请注意，即使没有从设备断开连接事件，也可能触发此事件。通常表示一段时间没有触发任何活动事件。
 - 设备状态服务负责维护以下[服务端属性](/docs/user-guide/attributes/#attribute-types)属性:

 - **active** - 表示当前设备状态为true或false;
 - **lastConnectTime** - 表示设备最后一次连接到ThingsBoard的时间，自1970年1月1日格林尼治标准时间00:00:00以来的毫秒数
 - **lastDisconnectTime** - 表示设备与ThingsBoard断开连接的最后时间，自1970年1月1日格林威治标准时间00:00:00以来的毫秒数
 - **lastActivityTime** - 表示设备上次推送遥测，属性更新或rpc命令的时间，自1970年1月1日格林威治标准时间00:00:00以来的毫秒数
 - **inactivityAlarmTime** - 表示上一次触发不活动事件的时间，自1970年1月1日格林尼治标准时间00:00:00以来的毫秒数
 
## 配置

设备状态服务将全局配置参数用于不活动超时，参数(state.defaultInactivityTimeoutInSec)在**thingsboard.yml**中定义默认为10秒。

用户可以通过设置服务器端属性"inactivityTimeout"来覆盖单个设备的此参数（值以毫秒为单位）。

设备状态服务使用全局配置参数来检测不活动事件,参数(state.defaultStateCheckIntervalInSec)在**thingsboard.yml**中定义默认为10秒。

## 下一步

{% assign currentGuide = "AdvancedFeatures" %}{% include templates/guides-banner.md %}



 


 
    
