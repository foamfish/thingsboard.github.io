---
layout: docwithnav
assignees:
- ashvayka
title: 升级说明
description: ThingsBoard升级说明

---

<ul id="markdown-toc">
  <li>
    <a href="#upgrading-to-103" id="markdown-toc-upgrading-to-103">升级到1.0.3</a>
  </li>
  <li>
    <a href="#upgrading-to-110" id="markdown-toc-upgrading-to-110">升级到1.1.0</a>
  </li>
  <li>
    <a href="#upgrading-to-120" id="markdown-toc-upgrading-to-120">升级到1.2.0</a>
    <ul>
        <li>
            <a href="#ubuntucentos" id="markdown-toc-ubuntucentos">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows" id="markdown-toc-windows">Windows</a>        
        </li>
    </ul>
  </li>
  <li>
    <a href="#upgrading-to-121" id="markdown-toc-upgrading-to-121">升级到1.2.1</a>
    <ul>
        <li>
            <a href="#ubuntucentos-1" id="markdown-toc-ubuntucentos-1">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-1" id="markdown-toc-windows-1">Windows</a>        
        </li>
    </ul>
  </li>
  <li>
    <a href="#upgrading-to-122" id="markdown-toc-upgrading-to-122">升级到1.2.2</a>
    <ul>
        <li>
            <a href="#ubuntucentos-2" id="markdown-toc-ubuntucentos-2">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-2" id="markdown-toc-windows-2">Windows</a>        
        </li>
    </ul>
  </li>
  <li>
    <a href="#upgrading-to-123" id="markdown-toc-upgrading-to-123">升级到1.2.3</a>
    <ul>
        <li>
            <a href="#ubuntucentos-3" id="markdown-toc-ubuntucentos-3">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-3" id="markdown-toc-windows-3">Windows</a>        
        </li>
    </ul>
  </li>
  <li>
    <a href="#upgrading-to-130" id="markdown-toc-upgrading-to-130">升级到1.3.0</a>
    <ul>
        <li>
            <a href="#ubuntucentos-4" id="markdown-toc-ubuntucentos-4">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-4" id="markdown-toc-windows-4">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-131" id="markdown-toc-upgrading-to-131">升级到1.3.1</a>
    <ul>
        <li>
            <a href="#ubuntucentos-5" id="markdown-toc-ubuntucentos-5">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-5" id="markdown-toc-windows-5">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-140" id="markdown-toc-upgrading-to-140">升级到1.4.0</a>
    <ul>
        <li>
            <a href="#ubuntucentos-6" id="markdown-toc-ubuntucentos-6">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-6" id="markdown-toc-windows-6">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-200" id="markdown-toc-upgrading-to-200">升级到2.0.0</a>
    <ul>
        <li>
            <a href="#ubuntucentos-7" id="markdown-toc-ubuntucentos-7">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-7" id="markdown-toc-windows-7">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-201" id="markdown-toc-upgrading-to-201">升级到2.0.1</a>
    <ul>
        <li>
            <a href="#ubuntucentos-8" id="markdown-toc-ubuntucentos-8">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-8" id="markdown-toc-windows-8">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-202" id="markdown-toc-upgrading-to-202">升级到2.0.2</a>
    <ul>
        <li>
            <a href="#ubuntucentos-9" id="markdown-toc-ubuntucentos-9">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-9" id="markdown-toc-windows-9">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-203" id="markdown-toc-upgrading-to-203">升级到2.0.3</a>
    <ul>
        <li>
            <a href="#ubuntucentos-10" id="markdown-toc-ubuntucentos-10">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-10" id="markdown-toc-windows-10">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-210" id="markdown-toc-upgrading-to-210">升级到2.1.0</a>
    <ul>
        <li>
            <a href="#ubuntucentos-11" id="markdown-toc-ubuntucentos-11">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-11" id="markdown-toc-windows-11">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-220" id="markdown-toc-upgrading-to-220">升级到2.2.0</a>
    <ul>
        <li>
            <a href="#ubuntucentos-12" id="markdown-toc-ubuntucentos-12">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-12" id="markdown-toc-windows-12">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-230" id="markdown-toc-upgrading-to-230">升级到2.3.0</a>
    <ul>
        <li>
            <a href="#ubuntucentos-13" id="markdown-toc-ubuntucentos-13">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-13" id="markdown-toc-windows-13">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-231" id="markdown-toc-upgrading-to-231">升级到2.3.1</a>
    <ul>
        <li>
            <a href="#ubuntucentos-14" id="markdown-toc-ubuntucentos-14">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-14" id="markdown-toc-windows-14">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-240" id="markdown-toc-upgrading-to-240">升级到2.4.0</a>
    <ul>
        <li>
            <a href="#ubuntucentos-15" id="markdown-toc-ubuntucentos-15">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-15" id="markdown-toc-windows-15">Windows</a>        
        </li>
    </ul>
  </li>  
  <li>
    <a href="#upgrading-to-241" id="markdown-toc-upgrading-to-241">升级到2.4.1</a>
    <ul>
        <li>
            <a href="#ubuntucentos-16" id="markdown-toc-ubuntucentos-16">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-16" id="markdown-toc-windows-16">Windows</a>        
        </li>
    </ul>
  </li>  
</ul>

## 升级到1.0.3

这些步骤适用于1.0、1.0.1和1.0.2 ThingsBoard版本。

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-1-0-3
thingsboard-download-1-0-3-ubuntu,Ubuntu,shell,resources/1.0.3/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/1.0.3/thingsboard-ubuntu-download.sh
thingsboard-download-1-0-3-centos,CentOS,shell,resources/1.0.3/thingsboard-centos-download.sh,/docs/user-guide/install/resources/1.0.3/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-1-0-3
thingsboard-installation-1-0-3-ubuntu,Ubuntu,shell,resources/1.0.3/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/1.0.3/thingsboard-ubuntu-installation.sh
thingsboard-installation-1-0-3-centos,CentOS,shell,resources/1.0.3/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/1.0.3/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

#### 数据库升级

仅当从1.0或1.0.1版本升级时，才需要执行此步骤。
请按照以下说明更新您的单节点实例：

```bash
# Download upgrade scripts
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.0.3/upgrade_1.0_1.0.2.sh
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.0.3/system_widgets_1.0_1.0.2.cql

# Launch main script
$ chmod +x upgrade_1.0_1.0.2.sh
$ ./upgrade_1.0_1.0.2.sh

``` 
  
#### 启动服务

```bash
$ sudo service thingsboard start
```

## 升级到1.1.0

这些步骤适用于1.0.3 ThingsBoard版本。

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-1-1-0
thingsboard-download-1-1-0-ubuntu,Ubuntu,shell,resources/1.1.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/1.1.0/thingsboard-ubuntu-download.sh
thingsboard-download-1-1-0-centos,CentOS,shell,resources/1.1.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/1.1.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-1-1-0
thingsboard-installation-1-1-0-ubuntu,Ubuntu,shell,resources/1.1.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/1.1.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-1-1-0-centos,CentOS,shell,resources/1.1.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/1.1.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

#### 数据库升级

请按照以下说明更新您的单节点实例：

```bash
# Download upgrade scripts
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.1.0/upgrade_1.0.3_1.1.0.sh
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.1.0/system_widgets_1.0.3_1.1.0.cql

# Launch main script
$ chmod +x upgrade_1.0.3_1.1.0.sh
$ ./upgrade_1.0.3_1.1.0.sh

``` 
  
#### 启动服务

```bash
$ sudo service thingsboard start
```

## 升级到1.2.0

这些步骤适用于1.1.0 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-1-2-0
thingsboard-download-1-2-0-ubuntu,Ubuntu,shell,resources/1.2.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/1.2.0/thingsboard-ubuntu-download.sh
thingsboard-download-1-2-0-centos,CentOS,shell,resources/1.2.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/1.2.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-1-2-0
thingsboard-installation-1-2-0-ubuntu,Ubuntu,shell,resources/1.2.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/1.2.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-1-2-0-centos,CentOS,shell,resources/1.2.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/1.2.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

#### 数据库升级

```bash
# Download upgrade scripts
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.0/upgrade_1.1.0_1.2.0.sh
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.0/system_widgets.cql

# Launch main script
$ chmod +x upgrade_1.1.0_1.2.0.sh
$ ./upgrade_1.1.0_1.2.0.sh

```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-1.2.zip](https://github.com/thingsboard/thingsboard/releases/download/v1.2/thingsboard-windows-1.2.zip).

#### ThingsBoard服务升级

* 对ThingsBoard安装目录中的旧版配置进行备份
* 通过运行ThingsBoard安装目录中的**uninstall.bat**来卸载ThingsBoard服务的先前版本。

**注意** 上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>uninstall.bat
```
* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 运行** install.bat **脚本将新版本的ThingsBoard安装为Windows服务。

```text
C:\thingsboard>install.bat
```

#### 数据库升级
 
* 将升级脚本下载到某些文件夹：
  * [upgrade_1.1.0_1.2.0.bat](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.0/upgrade_1.1.0_1.2.0.bat)
  * [system_widgets.cql](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.0/system_widgets.cql)
* 执行**upgrade_1.1.0_1.2.0.bat **（**注意**此脚本应使用管理角色执行）

```text
upgrade_1.1.0_1.2.0.bat
```
  
#### 启动服务

```text
net start thingsboard
```

## 升级到1.2.1

这些步骤适用于1.2.0 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-1-2-1
thingsboard-download-1-2-1-ubuntu,Ubuntu,shell,resources/1.2.1/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/1.2.1/thingsboard-ubuntu-download.sh
thingsboard-download-1-2-1-centos,CentOS,shell,resources/1.2.1/thingsboard-centos-download.sh,/docs/user-guide/install/resources/1.2.1/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-1-2-1
thingsboard-installation-1-2-1-ubuntu,Ubuntu,shell,resources/1.2.1/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/1.2.1/thingsboard-ubuntu-installation.sh
thingsboard-installation-1-2-1-centos,CentOS,shell,resources/1.2.1/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/1.2.1/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

#### 数据库升级

```bash
# Download upgrade scripts
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.1/upgrade_1.2.0_1.2.1.sh
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.1/schema_update.cql
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.1/system_widgets.cql

# Launch main script
$ chmod +x upgrade_1.2.0_1.2.1.sh
$ ./upgrade_1.2.0_1.2.1.sh

```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-1.2.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v1.2.1/thingsboard-windows-1.2.1.zip).

#### ThingsBoard服务升级

* 对ThingsBoard安装目录中的旧版配置进行备份
* 通过运行ThingsBoard安装目录中的**uninstall.bat**来卸载ThingsBoard服务的先前版本。

**注意** 上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>uninstall.bat
```
* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 运行** install.bat **脚本将新版本的ThingsBoard安装为Windows服务。

```text
C:\thingsboard>install.bat
```

#### 数据库升级
 
* 将升级脚本下载到某些文件夹：
  * [upgrade_1.2.0_1.2.1.bat](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.1/upgrade_1.2.0_1.2.1.bat)
  * [schema_update.cql](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.1/schema_update.cql)
  * [system_widgets.cql](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.1/system_widgets.cql)
* Execute **upgrade_1.2.0_1.2.1.bat** (**NOTE** This script should be executed using Administrative Role)

```text
upgrade_1.2.0_1.2.1.bat
```
  
#### 启动服务

```text
net start thingsboard
```

## 升级到1.2.2

这些步骤适用于1.2.1 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-1-2-2
thingsboard-download-1-2-2-ubuntu,Ubuntu,shell,resources/1.2.2/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/1.2.2/thingsboard-ubuntu-download.sh
thingsboard-download-1-2-2-centos,CentOS,shell,resources/1.2.2/thingsboard-centos-download.sh,/docs/user-guide/install/resources/1.2.2/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-1-2-2
thingsboard-installation-1-2-2-ubuntu,Ubuntu,shell,resources/1.2.2/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/1.2.2/thingsboard-ubuntu-installation.sh
thingsboard-installation-1-2-2-centos,CentOS,shell,resources/1.2.2/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/1.2.2/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

#### 数据库升级

```bash
# Download upgrade scripts
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.2/upgrade_1.2.1_1.2.2.sh
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.2/system_widgets.cql

# Launch main script
$ chmod +x upgrade_1.2.1_1.2.2.sh
$ ./upgrade_1.2.1_1.2.2.sh

```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-1.2.2.zip](https://github.com/thingsboard/thingsboard/releases/download/v1.2.2/thingsboard-windows-1.2.2.zip).

#### ThingsBoard服务升级

* 对ThingsBoard安装目录中的旧版配置进行备份
* 通过运行ThingsBoard安装目录中的**uninstall.bat**来卸载ThingsBoard服务的先前版本。

**注意** 上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>uninstall.bat
```
* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 运行** install.bat **脚本将新版本的ThingsBoard安装为Windows服务。

```text
C:\thingsboard>install.bat
```

#### 数据库升级
 
* 将升级脚本下载到某些文件夹：
  * [upgrade_1.2.1_1.2.2.bat](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.2/upgrade_1.2.1_1.2.2.bat)
  * [system_widgets.cql](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.2/system_widgets.cql)
* Execute **upgrade_1.2.1_1.2.2.bat** (**NOTE** This script should be executed using Administrative Role)

```text
upgrade_1.2.1_1.2.2.bat
```
  
#### 启动服务

```text
net start thingsboard
```

## 升级到1.2.3

These steps are applicable for 1.2.2 ThingsBoard version.

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-1-2-3
thingsboard-download-1-2-3-ubuntu,Ubuntu,shell,resources/1.2.3/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/1.2.3/thingsboard-ubuntu-download.sh
thingsboard-download-1-2-3-centos,CentOS,shell,resources/1.2.3/thingsboard-centos-download.sh,/docs/user-guide/install/resources/1.2.3/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-1-2-3
thingsboard-installation-1-2-3-ubuntu,Ubuntu,shell,resources/1.2.3/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/1.2.3/thingsboard-ubuntu-installation.sh
thingsboard-installation-1-2-3-centos,CentOS,shell,resources/1.2.3/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/1.2.3/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

#### 数据库升级

```bash
# Download upgrade scripts
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.3/upgrade_1.2.2_1.2.3.sh
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.3/schema_update.cql
$ wget https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.3/system_widgets.cql

# Launch main script
$ chmod +x upgrade_1.2.2_1.2.3.sh
$ ./upgrade_1.2.2_1.2.3.sh

```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-1.2.3.zip](https://github.com/thingsboard/thingsboard/releases/download/v1.2.3/thingsboard-windows-1.2.3.zip).

#### ThingsBoard服务升级

* 对ThingsBoard安装目录中的旧版配置进行备份
* 通过运行ThingsBoard安装目录中的**uninstall.bat**来卸载ThingsBoard服务的先前版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>uninstall.bat
```
* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 运行**install.bat**脚本将新版本的ThingsBoard安装为Windows服务。

```text
C:\thingsboard>install.bat
```

#### 数据库升级
 
* 将升级脚本下载到某些文件夹：
  * [upgrade_1.2.2_1.2.3.bat](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.3/upgrade_1.2.2_1.2.3.bat)
  * [schema_update.cql](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.3/schema_update.cql)
  * [system_widgets.cql](https://raw.githubusercontent.com/thingsboard/thingsboard.github.io/master/docs/user-guide/install/resources/1.2.3/system_widgets.cql)
* 执行**upgrade_1.2.2_1.2.3.bat**（**注意**此脚本应使用管理角色执行）

```text
upgrade_1.2.2_1.2.3.bat
```
  
#### 启动服务

```text
net start thingsboard
```

## 升级到1.3.0

这些步骤适用于1.2.3 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-1-3-0
thingsboard-download-1-3-0-ubuntu,Ubuntu,shell,resources/1.3.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/1.3.0/thingsboard-ubuntu-download.sh
thingsboard-download-1-3-0-centos,CentOS,shell,resources/1.3.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/1.3.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-1-3-0
thingsboard-installation-1-3-0-ubuntu,Ubuntu,shell,resources/1.3.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/1.3.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-1-3-0-centos,CentOS,shell,resources/1.3.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/1.3.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值(在文件 **/etc/thingsboard/conf/thingsboard.yml** )中设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
```
database:
    type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
```       

```bash
# Execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=1.2.3 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-1.3.zip](https://github.com/thingsboard/thingsboard/releases/download/v1.3/thingsboard-windows-1.3.zip).

#### ThingsBoard服务升级

* 如果正在运行，则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
  
  ```
  database:
      type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
  ```       
* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=1.2.3
```
  
#### 启动服务

```text
net start thingsboard
```

## 升级到1.3.1

这些步骤适用于1.3.0 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-1-3-1
thingsboard-download-1-3-1-ubuntu,Ubuntu,shell,resources/1.3.1/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/1.3.1/thingsboard-ubuntu-download.sh
thingsboard-download-1-3-1-centos,CentOS,shell,resources/1.3.1/thingsboard-centos-download.sh,/docs/user-guide/install/resources/1.3.1/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-1-3-1
thingsboard-installation-1-3-1-ubuntu,Ubuntu,shell,resources/1.3.1/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/1.3.1/thingsboard-ubuntu-installation.sh
thingsboard-installation-1-3-1-centos,CentOS,shell,resources/1.3.1/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/1.3.1/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序可能会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值（在文件**/etc/thingsboard/conf/thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
```
database:
    type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
```       

```bash
# Execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=1.3.0 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-1.3.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v1.3.1/thingsboard-windows-1.3.1.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
      type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
  ```       
* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=1.3.0
```
  
#### 启动服务

```text
net start thingsboard
```

## 升级到1.4.0

这些步骤适用于1.3.1 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-1-4-0
thingsboard-download-1-4-0-ubuntu,Ubuntu,shell,resources/1.4.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/1.4.0/thingsboard-ubuntu-download.sh
thingsboard-download-1-4-0-centos,CentOS,shell,resources/1.4.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/1.4.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-1-4-0
thingsboard-installation-1-4-0-ubuntu,Ubuntu,shell,resources/1.4.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/1.4.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-1-4-0-centos,CentOS,shell,resources/1.4.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/1.4.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值(在文件 **/etc/thingsboard/conf/thingsboard.yml** )中设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
```
database:
    type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
```       

```bash
# Execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=1.3.1 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-1.4.zip](https://github.com/thingsboard/thingsboard/releases/download/v1.4/thingsboard-windows-1.4.zip).

#### ThingsBoard服务升级

* 如果正在运行，则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
      type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
  ```       
* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=1.3.1
```
  
#### 启动服务

```text
net start thingsboard
```

## 升级到2.0.0

这些步骤适用于1.4.0 ThingsBoard版本。

### Ubuntu/CentOS

{% include templates/upgrade-to-20-notice.md %}

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-0-0
thingsboard-download-2-0-0-ubuntu,Ubuntu,shell,resources/2.0.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.0.0/thingsboard-ubuntu-download.sh
thingsboard-download-2-0-0-centos,CentOS,shell,resources/2.0.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.0.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-0-0
thingsboard-installation-2-0-0-ubuntu,Ubuntu,shell,resources/2.0.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.0.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-0-0-centos,CentOS,shell,resources/2.0.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.0.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值(在文件 **/etc/thingsboard/conf/thingsboard.yml** )中设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
```
database:
    type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
```       

```bash
# Execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=1.4.0 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

{% include templates/upgrade-to-20-notice.md %}

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.0.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.0/thingsboard-windows-2.0.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
      type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
  ```       
* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=1.4.0
```
  
#### 启动服务

```text
net start thingsboard
```

## 升级到2.0.1

这些步骤适用于2.0.0 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-0-1
thingsboard-download-2-0-1-ubuntu,Ubuntu,shell,resources/2.0.1/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.0.1/thingsboard-ubuntu-download.sh
thingsboard-download-2-0-1-centos,CentOS,shell,resources/2.0.1/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.0.1/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-0-1
thingsboard-installation-2-0-1-ubuntu,Ubuntu,shell,resources/2.0.1/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.0.1/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-0-1-centos,CentOS,shell,resources/2.0.1/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.0.1/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值(在文件 **/etc/thingsboard/conf/thingsboard.yml** )中设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
```
database:
    type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
```       

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.0.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.0.1/thingsboard-windows-2.0.1.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
      type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
  ```       
  
#### 启动服务

```text
net start thingsboard
```

## 升级到2.0.2

这些步骤适用于2.0.1 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-0-2
thingsboard-download-2-0-2-ubuntu,Ubuntu,shell,resources/2.0.2/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.0.2/thingsboard-ubuntu-download.sh
thingsboard-download-2-0-2-centos,CentOS,shell,resources/2.0.2/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.0.2/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-0-2
thingsboard-installation-2-0-2-ubuntu,Ubuntu,shell,resources/2.0.2/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.0.2/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-0-2-centos,CentOS,shell,resources/2.0.2/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.0.2/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值(在文件 **/etc/thingsboard/conf/thingsboard.yml** )中设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
```
database:
    type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
```       

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.0.2.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.0.2/thingsboard-windows-2.0.2.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
      type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
  ```       
  
#### 启动服务

```text
net start thingsboard
```

## 升级到2.0.3

这些步骤适用于2.0.2 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-0-3
thingsboard-download-2-0-3-ubuntu,Ubuntu,shell,resources/2.0.3/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.0.3/thingsboard-ubuntu-download.sh
thingsboard-download-2-0-3-centos,CentOS,shell,resources/2.0.3/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.0.3/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-0-3
thingsboard-installation-2-0-3-ubuntu,Ubuntu,shell,resources/2.0.3/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.0.3/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-0-3-centos,CentOS,shell,resources/2.0.3/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.0.3/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值(在文件 **/etc/thingsboard/conf/thingsboard.yml** )中设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
```
database:
    type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
```       

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.0.3.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.0.3/thingsboard-windows-2.0.3.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
      type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
  ```       
  
#### 启动服务

```text
net start thingsboard
```

## 升级到2.1.0

这些步骤适用于2.0.3 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-1-0
thingsboard-download-2-1-0-ubuntu,Ubuntu,shell,resources/2.1.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.1.0/thingsboard-ubuntu-download.sh
thingsboard-download-2-1-0-centos,CentOS,shell,resources/2.1.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.1.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-1-0
thingsboard-installation-2-1-0-ubuntu,Ubuntu,shell,resources/2.1.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.1.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-1-0-centos,CentOS,shell,resources/2.1.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.1.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值(在文件 **/etc/thingsboard/conf/thingsboard.yml** )中设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
```
database:
    type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
```       

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.1/thingsboard-windows-2.1.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
      type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
  ```       

#### 启动服务

```text
net start thingsboard
```

## 升级到2.2.0

这些步骤适用于2.1.0、2.1.1、2.1.2和2.1.3ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-2-0
thingsboard-download-2-2-0-ubuntu,Ubuntu,shell,resources/2.2.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.2.0/thingsboard-ubuntu-download.sh
thingsboard-download-2-2-0-centos,CentOS,shell,resources/2.2.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.2.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-2-0
thingsboard-installation-2-2-0-ubuntu,Ubuntu,shell,resources/2.2.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.2.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-2-0-centos,CentOS,shell,resources/2.2.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.2.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将**database.entities.type**和**database.ts.type**参数值（在文件 **/etc/thingsboard/conf/thingsboard.yml** 中）设置为“cassandra”而不是“sql”来升级您的cassandra数据库：
 
```
    database:
      entities:
        type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
      ts:
        type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```

```bash
# Execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.0.0 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.2.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.2/thingsboard-windows-2.2.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
    entities:
      type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
    ts:
      type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
  ```       

* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=2.0.0
```

#### 启动服务

```text
net start thingsboard
```

## 升级到2.3.0

这些步骤适用于2.2.0 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-3-0
thingsboard-download-2-3-0-ubuntu,Ubuntu,shell,resources/2.3.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.3.0/thingsboard-ubuntu-download.sh
thingsboard-download-2-3-0-centos,CentOS,shell,resources/2.3.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.3.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-3-0
thingsboard-installation-2-3-0-ubuntu,Ubuntu,shell,resources/2.3.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.3.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-3-0-centos,CentOS,shell,resources/2.3.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.3.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将**database.entities.type**和**database.ts.type**参数值（在文件 **/etc/thingsboard/conf/thingsboard.yml** 中）设置为“cassandra”而不是“sql”来升级您的cassandra数据库：
 
```
    database:
      entities:
        type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
      ts:
        type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```

```bash
# Execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.2.0 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.3.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.3/thingsboard-windows-2.3.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
    entities:
      type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
    ts:
      type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
  ```       

* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=2.2.0
```

#### 启动服务

```text
net start thingsboard
```

## 升级到2.3.1

这些步骤适用于2.3.0 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-3-1
thingsboard-download-2-3-1-ubuntu,Ubuntu,shell,resources/2.3.1/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.3.1/thingsboard-ubuntu-download.sh
thingsboard-download-2-3-1-centos,CentOS,shell,resources/2.3.1/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.3.1/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-3-1
thingsboard-installation-2-3-1-ubuntu,Ubuntu,shell,resources/2.3.1/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.3.1/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-3-1-centos,CentOS,shell,resources/2.3.1/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.3.1/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将**database.entities.type**和**database.ts.type**参数值（在文件 **/etc/thingsboard/conf/thingsboard.yml** 中）设置为“cassandra”而不是“sql”来升级您的cassandra数据库：

 
```
    database:
      entities:
        type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
      ts:
        type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```

```bash
# Execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.3.0 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.3.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.3.1/thingsboard-windows-2.3.1.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
    entities:
      type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
    ts:
      type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
  ```       

* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=2.3.0
```

#### 启动服务

```text
net start thingsboard
```


## 升级到2.4.0

这些步骤适用于2.3.1 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-4-0
thingsboard-download-2-4-0-ubuntu,Ubuntu,shell,resources/2.4.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.4.0/thingsboard-ubuntu-download.sh
thingsboard-download-2-4-0-centos,CentOS,shell,resources/2.4.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.4.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-4-0
thingsboard-installation-2-4-0-ubuntu,Ubuntu,shell,resources/2.4.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.4.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-4-0-centos,CentOS,shell,resources/2.4.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.4.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将**database.entities.type**和**database.ts.type**参数值（在文件 **/etc/thingsboard/conf/thingsboard.yml** 中）设置为“cassandra”而不是“sql”来升级您的cassandra数据库：
 
```
    database:
      entities:
        type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
      ts:
        type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```

```bash
# Execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.3.1 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.4.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.4/thingsboard-windows-2.4.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
    entities:
      type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
    ts:
      type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
  ```       

* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=2.3.1
```

#### 启动服务

```text
net start thingsboard
```

## 升级到2.4.1

这些步骤适用于2.4.0 ThingsBoard版本。

### Ubuntu/CentOS

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-4-1
thingsboard-download-2-4-1-ubuntu,Ubuntu,shell,resources/2.4.1/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.4.1/thingsboard-ubuntu-download.sh
thingsboard-download-2-4-1-centos,CentOS,shell,resources/2.4.1/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.4.1/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-4-1
thingsboard-installation-2-4-1-ubuntu,Ubuntu,shell,resources/2.4.1/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.4.1/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-4-1-centos,CentOS,shell,resources/2.4.1/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.4.1/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将**database.entities.type**和**database.ts.type**参数值（在文件 **/etc/thingsboard/conf/thingsboard.yml** 中）设置为“cassandra”而不是“sql”来升级您的cassandra数据库：
 
```
    database:
      entities:
        type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
      ts:
        type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```

```bash
# Execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.4.0 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows

#### ThingsBoard软件包下载

下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.4.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.4.1/thingsboard-windows-2.4.1.zip).

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
    entities:
      type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
    ts:
      type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
  ```       

* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=2.4.0
```

#### 启动服务

```text
net start thingsboard
```

## 下一步

{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}
