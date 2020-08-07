---
layout: docwithnav
assignees:
- ashvayka
title: 升级说明
description: ThingsBoard升级说明

---

<ul id="markdown-toc">
<<<<<<< HEAD
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
=======
    <li>
      <a href="#upgrading-to-301" id="markdown-toc-upgrading-to-30">Upgrading to 3.0.1</a>
      <ul>
          <li>
              <a href="#ubuntucentos-301" id="markdown-toc-ubuntucentos-301">Ubuntu/CentOS</a>        
          </li>
          <li>
              <a href="#windows-301" id="markdown-toc-windows-301">Windows</a>        
          </li>
      </ul>
    </li>
    <li>
      <a href="#upgrading-to-30" id="markdown-toc-upgrading-to-30">Upgrading to 3.0</a>
      <ul>
          <li>
              <a href="#ubuntucentos-30" id="markdown-toc-ubuntucentos-30">Ubuntu/CentOS</a>        
          </li>
          <li>
              <a href="#windows-30" id="markdown-toc-windows-30">Windows</a>        
          </li>
      </ul>
    </li>
    <li>
      <a href="#upgrading-to-252" id="markdown-toc-upgrading-to-252">Upgrading to 2.5.2</a>
      <ul>
          <li>
              <a href="#ubuntucentos-252" id="markdown-toc-ubuntucentos-252">Ubuntu/CentOS</a>        
          </li>
          <li>
              <a href="#windows-252" id="markdown-toc-windows-252">Windows</a>        
          </li>
      </ul>
    </li>      
    <li>
      <a href="#upgrading-to-251" id="markdown-toc-upgrading-to-251">Upgrading to 2.5.1</a>
      <ul>
          <li>
              <a href="#ubuntucentos-251" id="markdown-toc-ubuntucentos-20">Ubuntu/CentOS</a>        
          </li>
          <li>
              <a href="#windows-251" id="markdown-toc-windows-20">Windows</a>        
          </li>
      </ul>
    </li>    
    <li>
      <a href="#upgrading-to-25" id="markdown-toc-upgrading-to-25">Upgrading to 2.5</a>
      <ul>
          <li>
              <a href="#ubuntucentos-19" id="markdown-toc-ubuntucentos-19">Ubuntu/CentOS</a>        
          </li>
          <li>
              <a href="#windows-19" id="markdown-toc-windows-19">Windows</a>        
          </li>
      </ul>
    </li>
    <li>
    <a href="#upgrading-to-243" id="markdown-toc-upgrading-to-243">Upgrading to 2.4.3</a>
>>>>>>> master
    <ul>
        <li>
            <a href="#ubuntucentos-18" id="markdown-toc-ubuntucentos-18">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-18" id="markdown-toc-windows-18">Windows</a>        
        </li>
    </ul>
<<<<<<< HEAD
  </li>  
  <li>
    <a href="#upgrading-to-240" id="markdown-toc-upgrading-to-240">升级到2.4.0</a>
=======
    </li>
    <li>
    <a href="#upgrading-to-2421" id="markdown-toc-upgrading-to-2421">Upgrading to 2.4.2.1</a>
>>>>>>> master
    <ul>
        <li>
            <a href="#ubuntucentos-17" id="markdown-toc-ubuntucentos-17">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-17" id="markdown-toc-windows-17">Windows</a>        
        </li>
    </ul>
<<<<<<< HEAD
  </li>  
  <li>
    <a href="#upgrading-to-241" id="markdown-toc-upgrading-to-241">升级到2.4.1</a>
=======
    </li> 
    <li>
    <a href="#upgrading-to-241" id="markdown-toc-upgrading-to-241">Upgrading to 2.4.1</a>
>>>>>>> master
    <ul>
        <li>
            <a href="#ubuntucentos-16" id="markdown-toc-ubuntucentos-16">Ubuntu/CentOS</a>        
        </li>
        <li>
            <a href="#windows-16" id="markdown-toc-windows-16">Windows</a>        
        </li>
    </ul>
<<<<<<< HEAD
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
=======
    </li> 
    <li>
    <a href="#upgrading-to-240" id="markdown-toc-upgrading-to-240">Upgrading to 2.4.0</a>
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
    <a href="/docs/user-guide/install/old-upgrade-instructions/" id="markdown-toc-upgrading-to-240">Older versions</a>
    </li>       
</ul>

## Upgrading to 3.0.1 {#upgrading-to-301} 

### Ubuntu/CentOS {#ubuntucentos-301}

**NOTE**: These upgrade steps are applicable for ThingsBoard version 3.0. In order to upgrade to 3.0.1 you need to [**upgrade to 3.0 first**](/docs/user-guide/install/upgrade-instructions/#ubuntucentos-30).

<br>

{% include templates/install/tb-30-update.md %}

<br>

{% capture tb_3_0_1_postgreSQL_linux %}
**Since ThingsBoard 3.0 only PostgreSQL database is supported for entities data**  
 - If you are using **Cassandra** database for entities data please install PostgreSQL database before proceeding upgrade procedure using the following guide:
   - [PostgreSQL Installation on Ubuntu](/docs/user-guide/install/ubuntu/?ubuntuThingsboardDatabase=postgresql#step-3-configure-thingsboard-database)
   - [PostgreSQL Installation on CentOS/RHEL](/docs/user-guide/install/rhel/?rhelThingsboardDatabase=postgresql#step-3-configure-thingsboard-database)
>>>>>>> master

{% endcapture %}
{% include templates/info-banner.md content=tb_3_0_1_postgreSQL_linux %}

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-3-0-1
thingsboard-download-3-0-1-ubuntu,Ubuntu,shell,resources/3.0.1/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/3.0.1/thingsboard-ubuntu-download.sh
thingsboard-download-3-0-1-centos,CentOS,shell,resources/3.0.1/thingsboard-centos-download.sh,/docs/user-guide/install/resources/3.0.1/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-3-0-1
thingsboard-installation-3-0-1-ubuntu,Ubuntu,shell,resources/3.0.1/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/3.0.1/thingsboard-ubuntu-installation.sh
thingsboard-installation-3-0-1-centos,CentOS,shell,resources/3.0.1/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/3.0.1/thingsboard-centos-installation.sh{% endcapture %} 
{% include tabs.html %}

<<<<<<< HEAD
**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值(在文件 **/etc/thingsboard/conf/thingsboard.yml** )中设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
=======
**NOTE:** Package installer will ask you to merge your thingsboard configuration. It is preferred to use **merge option** to make sure that all your previous parameters will not be overwritten.  
Please make sure that you set **database.ts.type** parameter value (in the file **/etc/thingsboard/conf/thingsboard.yml**) to "cassandra" instead of "sql" if you are using Cassandra database for timeseries data:

>>>>>>> master
```
database:
  ts_max_intervals: "${DATABASE_TS_MAX_INTERVALS:700}" # Max number of DB queries generated by single API call to fetch telemetry records
  ts:
    type: "${DATABASE_TS_TYPE:sql}" # cassandra, sql, or timescale (for hybrid mode, DATABASE_TS_TYPE value should be cassandra, or timescale)
```

**NOTE**: If you were using **Cassandra** database for entities data execute the following migration script: 

```bash
# Execute migration script from Cassandra to PostgreSQL
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.5.0-cassandra
```

Otherwise execute regular upgrade script:

```bash
# Execute regular upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.5.0
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows {#windows-301}

**NOTE**: These upgrade steps are applicable for ThingsBoard version 3.0. In order to upgrade to 3.0.1 you need to [**upgrade to 3.0 first**](/docs/user-guide/install/upgrade-instructions/#windows-30).

<br>

{% include templates/install/tb-30-update.md %}

{% capture tb_3_0_1_postgreSQL_windows %}
**Since ThingsBoard 3.0 only PostgreSQL database is supported for entities data**  
 - If you are using **Cassandra** database for entities data please install PostgreSQL database before proceeding upgrade procedure using the following guide:
   - [PostgreSQL Installation on Windows](/docs/user-guide/install/windows/?ubuntuThingsboardDatabase=postgresql#step-3-configure-thingsboard-database)

{% endcapture %}
{% include templates/info-banner.md content=tb_3_0_1_postgreSQL_windows %}

#### ThingsBoard软件包下载

<<<<<<< HEAD
下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.0.3.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.0.3/thingsboard-windows-2.0.3.zip).
=======
Download ThingsBoard installation archive for Windows: [thingsboard-windows-3.0.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v3.0.1/thingsboard-windows-3.0.1.zip).
>>>>>>> master

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

<<<<<<< HEAD
* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
  
  ```
  database:
      type: "${DATABASE_TYPE:cassandra}" # cassandra OR sql
  ```       
  
#### 启动服务
=======
* Remove ThingsBoard install dir.
* Unzip installation archive to ThingsBoard install dir.
* Compare your old ThingsBoard configuration files (from the backup you made in the first step) with new ones.
* Please make sure that you set **database.ts.type** parameter value (in the file **\<ThingsBoard install dir\>\conf\thingsboard.yml**) to "cassandra" instead of "sql" if you are using Cassandra database for timeseries data:
  
```
database:
  ts_max_intervals: "${DATABASE_TS_MAX_INTERVALS:700}" # Max number of DB queries generated by single API call to fetch telemetry records
  ts:
    type: "${DATABASE_TS_TYPE:sql}" # cassandra, sql, or timescale (for hybrid mode, DATABASE_TS_TYPE value should be cassandra, or timescale)
```       

* Finally, run **upgrade.bat** script to upgrade ThingsBoard to the new version.

**NOTE** Scripts listed above should be executed using Administrator Role.

**NOTE**: If you were using **Cassandra** database for entities data execute the following migration script: 

```text
C:\thingsboard>upgrade.bat --fromVersion=2.5.0-cassandra
```

Otherwise execute regular upgrade script:

```text
C:\thingsboard>upgrade.bat --fromVersion=2.5.0
```

#### Start the service
>>>>>>> master

```text
net start thingsboard
```

<<<<<<< HEAD
## 升级到2.1.0

这些步骤适用于2.0.3 ThingsBoard版本。
=======

## Upgrading to 3.0 {#upgrading-to-30}


**NOTE**: These upgrade steps are applicable for ThingsBoard version 2.5.2. In order to upgrade to 3.0 you need to [**upgrade to 2.5.2 first**](/docs/user-guide/install/upgrade-instructions/#ubuntucentos-252).

### Ubuntu/CentOS {#ubuntucentos-30}

{% include templates/install/tb-30-update.md %}
>>>>>>> master

<br>

{% capture tb_3_0_postgreSQL_linux %}
**Since ThingsBoard 3.0 only PostgreSQL database is supported for entities data**  
 - If you are using **Cassandra** database for entities data please install PostgreSQL database before proceeding upgrade procedure using the following guide:
   - [PostgreSQL Installation on Ubuntu](/docs/user-guide/install/ubuntu/?ubuntuThingsboardDatabase=postgresql#step-3-configure-thingsboard-database)
   - [PostgreSQL Installation on CentOS/RHEL](/docs/user-guide/install/rhel/?rhelThingsboardDatabase=postgresql#step-3-configure-thingsboard-database)

{% endcapture %}
{% include templates/info-banner.md content=tb_3_0_postgreSQL_linux %}

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-3-0
thingsboard-download-3-0-ubuntu,Ubuntu,shell,resources/3.0.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/3.0.0/thingsboard-ubuntu-download.sh
thingsboard-download-3-0-centos,CentOS,shell,resources/3.0.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/3.0.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-3-0
thingsboard-installation-3-0-ubuntu,Ubuntu,shell,resources/3.0.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/3.0.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-3-0-centos,CentOS,shell,resources/3.0.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/3.0.0/thingsboard-centos-installation.sh{% endcapture %} 
{% include tabs.html %}

<<<<<<< HEAD
**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将database.type参数值(在文件 **/etc/thingsboard/conf/thingsboard.yml** )中设置为“cassandra”而不是“sql”，以便升级cassandra数据库：
 
=======
**NOTE:** Package installer will ask you to merge your thingsboard configuration. It is preferred to use **merge option** to make sure that all your previous parameters will not be overwritten.  
Please make sure that you set **database.ts.type** parameter value (in the file **/etc/thingsboard/conf/thingsboard.yml**) to "cassandra" instead of "sql" if you are using Cassandra database for timeseries data:

>>>>>>> master
```
database:
  ts_max_intervals: "${DATABASE_TS_MAX_INTERVALS:700}" # Max number of DB queries generated by single API call to fetch telemetry records
  ts:
    type: "${DATABASE_TS_TYPE:sql}" # cassandra, sql, or timescale (for hybrid mode, DATABASE_TS_TYPE value should be cassandra, or timescale)
```

**NOTE**: If you were using **Cassandra** database for entities data execute the following migration script: 

```bash
# Execute migration script from Cassandra to PostgreSQL
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.5.0-cassandra
```

Otherwise execute regular upgrade script:

```bash
# Execute regular upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.5.0
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows {#windows-30}

**NOTE**: These upgrade steps are applicable for ThingsBoard version 2.5.2. In order to upgrade to 3.0 you need to [**upgrade to 2.5.2 first**](/docs/user-guide/install/upgrade-instructions/#windows-252).

{% include templates/install/tb-30-update.md %}

{% capture tb_3_0_postgreSQL_windows %}
**Since ThingsBoard 3.0 only PostgreSQL database is supported for entities data**  
 - If you are using **Cassandra** database for entities data please install PostgreSQL database before proceeding upgrade procedure using the following guide:
   - [PostgreSQL Installation on Windows](/docs/user-guide/install/windows/?ubuntuThingsboardDatabase=postgresql#step-3-configure-thingsboard-database)

{% endcapture %}
{% include templates/info-banner.md content=tb_3_0_postgreSQL_windows %}

#### ThingsBoard软件包下载

<<<<<<< HEAD
下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.1/thingsboard-windows-2.1.zip).
=======
Download ThingsBoard installation archive for Windows: [thingsboard-windows-3.0.zip](https://github.com/thingsboard/thingsboard/releases/download/v3.0/thingsboard-windows-3.0.zip).
>>>>>>> master

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

<<<<<<< HEAD
* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
=======
* Remove ThingsBoard install dir.
* Unzip installation archive to ThingsBoard install dir.
* Compare your old ThingsBoard configuration files (from the backup you made in the first step) with new ones.
* Please make sure that you set **database.ts.type** parameter value (in the file **\<ThingsBoard install dir\>\conf\thingsboard.yml**) to "cassandra" instead of "sql" if you are using Cassandra database for timeseries data:
>>>>>>> master
  
```
database:
  ts_max_intervals: "${DATABASE_TS_MAX_INTERVALS:700}" # Max number of DB queries generated by single API call to fetch telemetry records
  ts:
    type: "${DATABASE_TS_TYPE:sql}" # cassandra, sql, or timescale (for hybrid mode, DATABASE_TS_TYPE value should be cassandra, or timescale)
```       

* Finally, run **upgrade.bat** script to upgrade ThingsBoard to the new version.

**NOTE** Scripts listed above should be executed using Administrator Role.

**NOTE**: If you were using **Cassandra** database for entities data execute the following migration script: 

```text
C:\thingsboard>upgrade.bat --fromVersion=2.5.0-cassandra
```

Otherwise execute regular upgrade script:

```text
C:\thingsboard>upgrade.bat --fromVersion=2.5.0
```

#### 启动服务

```text
net start thingsboard
```

<<<<<<< HEAD
## 升级到2.2.0

这些步骤适用于2.1.0、2.1.1、2.1.2和2.1.3ThingsBoard版本。
=======
## Upgrading to 2.5.2 {#upgrading-to-252}

### Ubuntu/CentOS {#ubuntucentos-252}
>>>>>>> master

**NOTE**: These upgrade steps are applicable for ThingsBoard version 2.5.1. In order to upgrade to 2.5.2 you need to [**upgrade to 2.5.1 first**](/docs/user-guide/install/upgrade-instructions/#ubuntucentos-251).

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-5-2
thingsboard-download-2-5-2-ubuntu,Ubuntu,shell,resources/2.5.2/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.5.2/thingsboard-ubuntu-download.sh
thingsboard-download-2-5-2-centos,CentOS,shell,resources/2.5.2/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.5.2/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-5-2
thingsboard-installation-2-5-2-ubuntu,Ubuntu,shell,resources/2.5.2/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.5.2/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-5-2-centos,CentOS,shell,resources/2.5.2/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.5.2/thingsboard-centos-installation.sh{% endcapture %} 
{% include tabs.html %}

<<<<<<< HEAD
**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将**database.entities.type**和**database.ts.type**参数值（在文件 **/etc/thingsboard/conf/thingsboard.yml** 中）设置为“cassandra”而不是“sql”来升级您的cassandra数据库：
 
```
    database:
      entities:
        type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
      ts:
        type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```
=======
**NOTE:** Upgrading ThingsBoard to 2.5.2 version in case of using PostgreSQL database require to upgrade the PostgreSQL service to 11.x version.

Please refer to the guides below that will describe how to upgrade your PostgreSQL service on:

 - [Ubuntu](https://gist.github.com/ShvaykaD/1f0e6c1321a0a2b4b9f3b9ea9ab3e8d3)
 - [CentOS](https://gist.github.com/ShvaykaD/313745d31a9af6db3d6a01ec9f16aac8)

**NOTE:** Package installer will ask you to merge your thingsboard configuration. It is preferred to use **merge option** to make sure that all your previous parameters will not be overwritten.  

>>>>>>> master

```bash
# Finally, execute upgrade script and specify your previous ThingsBoard version. 
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.4.3
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows {#windows-252}

**NOTE**: These upgrade steps are applicable for ThingsBoard version 2.5.1. In order to upgrade to 2.5.2 you need to [**upgrade to 2.5.1 first**](/docs/user-guide/install/upgrade-instructions/#windows-251).

#### ThingsBoard软件包下载

<<<<<<< HEAD
下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.2.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.2/thingsboard-windows-2.2.zip).
=======
Download ThingsBoard installation archive for Windows: [thingsboard-windows-2.5.2.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.5.2/thingsboard-windows-2.5.2.zip).
>>>>>>> master

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

<<<<<<< HEAD
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
=======
* Remove ThingsBoard install dir.
* Unzip installation archive to ThingsBoard install dir.
* Compare your old ThingsBoard configuration files (from the backup you made in the first step) with new ones.
* Please note that upgrading ThingsBoard from 2.4.3 to 2.5 version in case of using PostgreSQL database require to upgrade the PostgreSQL service to 11.x version.

* Finally, run **upgrade.bat** script to upgrade ThingsBoard to the new version.
>>>>>>> master

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=2.4.3
```

#### 启动服务

```text
net start thingsboard
```

<<<<<<< HEAD
## 升级到2.3.0

这些步骤适用于2.2.0 ThingsBoard版本。
=======
## Upgrading to 2.5.1 {#upgrading-to-251}

These steps are applicable for 2.4.3+ ThingsBoard version.
>>>>>>> master

### Ubuntu/CentOS {#ubuntucentos-251}

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-5-1
thingsboard-download-2-5-1-ubuntu,Ubuntu,shell,resources/2.5.1/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.5.1/thingsboard-ubuntu-download.sh
thingsboard-download-2-5-1-centos,CentOS,shell,resources/2.5.1/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.5.1/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-5-1
thingsboard-installation-2-5-1-ubuntu,Ubuntu,shell,resources/2.5.1/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.5.1/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-5-1-centos,CentOS,shell,resources/2.5.1/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.5.1/thingsboard-centos-installation.sh{% endcapture %} 
{% include tabs.html %}

<<<<<<< HEAD
**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将**database.entities.type**和**database.ts.type**参数值（在文件 **/etc/thingsboard/conf/thingsboard.yml** 中）设置为“cassandra”而不是“sql”来升级您的cassandra数据库：
 
```
    database:
      entities:
        type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
      ts:
        type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```
=======
**NOTE:** Upgrading ThingsBoard to 2.5.1 version in case of using PostgreSQL database require to upgrade the PostgreSQL service to 11.x version.

Please refer to the guides below that will describe how to upgrade your PostgreSQL service on:

 - [Ubuntu](https://gist.github.com/ShvaykaD/1f0e6c1321a0a2b4b9f3b9ea9ab3e8d3)
 - [CentOS](https://gist.github.com/ShvaykaD/313745d31a9af6db3d6a01ec9f16aac8)

**NOTE:** Package installer will ask you to merge your thingsboard configuration. It is preferred to use **merge option** to make sure that all your previous parameters will not be overwritten.  

>>>>>>> master

```bash
# Finally, execute upgrade script and specify your previous ThingsBoard version. 
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.4.3
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows {#windows-251}

#### ThingsBoard软件包下载

<<<<<<< HEAD
下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.3.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.3/thingsboard-windows-2.3.zip).
=======
Download ThingsBoard installation archive for Windows: [thingsboard-windows-2.5.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.5.1/thingsboard-windows-2.5.1.zip).
>>>>>>> master

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

<<<<<<< HEAD
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
=======
* Remove ThingsBoard install dir.
* Unzip installation archive to ThingsBoard install dir.
* Compare your old ThingsBoard configuration files (from the backup you made in the first step) with new ones.
* Please note that upgrading ThingsBoard from 2.4.3 to 2.5 version in case of using PostgreSQL database require to upgrade the PostgreSQL service to 11.x version.

* Finally, run **upgrade.bat** script to upgrade ThingsBoard to the new version.
>>>>>>> master

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=2.4.3
```

#### 启动服务

```text
net start thingsboard
```

<<<<<<< HEAD
## 升级到2.3.1

这些步骤适用于2.3.0 ThingsBoard版本。
=======

## Upgrading to 2.5
>>>>>>> master

These steps are applicable for 2.4.3 ThingsBoard version.

### Ubuntu/CentOS {#ubuntucentos-19}

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-5
thingsboard-download-2-5-ubuntu,Ubuntu,shell,resources/2.5/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.5/thingsboard-ubuntu-download.sh
thingsboard-download-2-5-centos,CentOS,shell,resources/2.5/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.5/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-5
thingsboard-installation-2-5-ubuntu,Ubuntu,shell,resources/2.5/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.5/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-5-centos,CentOS,shell,resources/2.5/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.5/thingsboard-centos-installation.sh{% endcapture %} 
{% include tabs.html %}

<<<<<<< HEAD
**注意：**软件包安装程序会要求您合并您的Thingsboard配置。最好使用**合并选项**以确保您之前的所有参数都不会被覆盖。
请确保将**database.entities.type**和**database.ts.type**参数值（在文件 **/etc/thingsboard/conf/thingsboard.yml** 中）设置为“cassandra”而不是“sql”来升级您的cassandra数据库：

 
=======
**NOTE:** Upgrading ThingsBoard from 2.4.3 to 2.5 version in case of using PostgreSQL database require to upgrade the PostgreSQL service to 11.x version.

Please refer to the guides below that will describe how to upgrade your PostgreSQL service on:

 - [Ubuntu](https://gist.github.com/ShvaykaD/1f0e6c1321a0a2b4b9f3b9ea9ab3e8d3)
 - [CentOS](https://gist.github.com/ShvaykaD/313745d31a9af6db3d6a01ec9f16aac8)

**NOTE:** Package installer will ask you to merge your thingsboard configuration. It is preferred to use **merge option** to make sure that all your previous parameters will not be overwritten.  
Please make sure that you set **database.entities.type** and **database.ts.type** parameters values (in the file **/etc/thingsboard/conf/thingsboard.yml**) to "cassandra" instead of "sql" in order to upgrade your cassandra database:

>>>>>>> master
```
database:
  ts_max_intervals: "${DATABASE_TS_MAX_INTERVALS:700}" # Max number of DB queries generated by single API call to fetch telemetry records
  entities:
    type: "${DATABASE_ENTITIES_TYPE:sql}" # cassandra OR sql
  ts:
    type: "${DATABASE_TS_TYPE:sql}" # cassandra, sql, or timescale (for hybrid mode, DATABASE_TS_TYPE value should be cassandra, or timescale)

# note: timescale works only with postgreSQL database for DATABASE_ENTITIES_TYPE.
```

**NOTE:** If you are using **PostgreSql(Sql)** for time-series data storage before executing the upgrade script, go to the PostgreSQL terminal(psql) and follow the instructions below: 

```bash
# Connect to thingsboard database:
\c thingsboard

# Execute the next commands:

# Update ts_kv table constraints:
ALTER TABLE ts_kv DROP CONSTRAINT IF EXISTS ts_kv_unq_key;
ALTER TABLE ts_kv DROP CONSTRAINT IF EXISTS ts_kv_pkey;
ALTER TABLE ts_kv ADD CONSTRAINT ts_kv_pkey PRIMARY KEY (entity_type, entity_id, key, ts);

# Update ts_kv_latest table constraints:
ALTER TABLE ts_kv_latest DROP CONSTRAINT IF EXISTS ts_kv_latest_unq_key;
ALTER TABLE ts_kv_latest DROP CONSTRAINT IF EXISTS ts_kv_latest_pkey;
ALTER TABLE ts_kv_latest ADD CONSTRAINT ts_kv_latest_pkey PRIMARY KEY (entity_type, entity_id, key);

# exit psql terminal 
\q
```

```bash
# Finally, execute upgrade script
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.4.3
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows {#windows-19}

#### ThingsBoard软件包下载

<<<<<<< HEAD
下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.3.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.3.1/thingsboard-windows-2.3.1.zip).
=======
Download ThingsBoard installation archive for Windows: [thingsboard-windows-2.5.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.5/thingsboard-windows-2.5.zip).
>>>>>>> master

#### ThingsBoard服务升级

* 如果正在运行则停止ThingsBoard服务。
 
```text
net stop thingsboard
```

* 对ThingsBoard安装目录中的旧版配置进行备份

<<<<<<< HEAD
* 删除ThingsBoard安装目录。
* 将安装档案解压缩到ThingsBoard安装目录。
* 将旧的ThingsBoard配置文件（来自第一步中的备份）与新的进行比较。
* 请确保将database.type参数值（在文件**\<ThingsBoard install dir\>\conf\thingsboard.yml**中）设置为“cassandra”而不是“sql”，以便升级cassandra数据库
=======
* Remove ThingsBoard install dir.
* Unzip installation archive to ThingsBoard install dir.
* Compare your old ThingsBoard configuration files (from the backup you made in the first step) with new ones.
* Please note that upgrading ThingsBoard from 2.4.3 to 2.5 version in case of using PostgreSQL database require to upgrade the PostgreSQL service to 11.x version.
* Please make sure that you set **database.entities.type** and **database.ts.type** parameters values (in the file **\<ThingsBoard install dir\>\conf\thingsboard.yml**) to "cassandra" instead of "sql" in order to upgrade your cassandra database:
>>>>>>> master
  
```
database:
  ts_max_intervals: "${DATABASE_TS_MAX_INTERVALS:700}" # Max number of DB queries generated by single API call to fetch telemetry records
  entities:
    type: "${DATABASE_ENTITIES_TYPE:sql}" # cassandra OR sql
  ts:
    type: "${DATABASE_TS_TYPE:sql}" # cassandra, sql, or timescale (for hybrid mode, DATABASE_TS_TYPE value should be cassandra, or timescale)

<<<<<<< HEAD
* 运行**upgrade.bat**脚本将ThingsBoard升级到新版本。
=======
# note: timescale works only with postgreSQL database for DATABASE_ENTITIES_TYPE.
```       

**NOTE:** If you are using **PostgreSql(Sql)** for time-series data storage before executing the upgrade script, you need to access the psql terminal. Once you will be logged to the psql terminal, please follow the instructions below:

```bash
# Connect to thingsboard database:
\c thingsboard

# Execute the next commands:

# Update ts_kv table constraints:
ALTER TABLE ts_kv DROP CONSTRAINT IF EXISTS ts_kv_unq_key;
ALTER TABLE ts_kv DROP CONSTRAINT IF EXISTS ts_kv_pkey;
ALTER TABLE ts_kv ADD CONSTRAINT ts_kv_pkey PRIMARY KEY (entity_type, entity_id, key, ts);

# Update ts_kv_latest table constraints:
ALTER TABLE ts_kv_latest DROP CONSTRAINT IF EXISTS ts_kv_latest_unq_key;
ALTER TABLE ts_kv_latest DROP CONSTRAINT IF EXISTS ts_kv_latest_pkey;
ALTER TABLE ts_kv_latest ADD CONSTRAINT ts_kv_latest_pkey PRIMARY KEY (entity_type, entity_id, key);

# exit psql terminal 
\q
```

* Finally, run **upgrade.bat** script to upgrade ThingsBoard to the new version.
>>>>>>> master

**注意**上面列出的脚本应使用管理员角色执行。

```text
C:\thingsboard>upgrade.bat --fromVersion=2.4.3
```

#### 启动服务

```text
net start thingsboard
```

## Upgrading to 2.4.3

<<<<<<< HEAD
## 升级到2.4.0

这些步骤适用于2.3.1 ThingsBoard版本。
=======
These steps are applicable for 2.4.2 and 2.4.2.1 ThingsBoard versions.
>>>>>>> master

### Ubuntu/CentOS {#ubuntucentos-18}

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-4-3
thingsboard-download-2-4-3-ubuntu,Ubuntu,shell,resources/2.4.3/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.4.3/thingsboard-ubuntu-download.sh
thingsboard-download-2-4-3-centos,CentOS,shell,resources/2.4.3/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.4.3/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-4-3
thingsboard-installation-2-4-3-ubuntu,Ubuntu,shell,resources/2.4.3/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.4.3/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-4-3-centos,CentOS,shell,resources/2.4.3/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.4.3/thingsboard-centos-installation.sh{% endcapture %}  
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
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.4.2
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows {#windows-18}

#### ThingsBoard软件包下载

<<<<<<< HEAD
下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.4.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.4/thingsboard-windows-2.4.zip).
=======
Download ThingsBoard installation archive for Windows: [thingsboard-windows-2.4.3.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.4.3/thingsboard-windows-2.4.3.zip).
>>>>>>> master

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
C:\thingsboard>upgrade.bat --fromVersion=2.4.2
```

#### 启动服务

```text
net start thingsboard
```

<<<<<<< HEAD
## 升级到2.4.1

这些步骤适用于2.4.0 ThingsBoard版本。
=======
## Upgrading to 2.4.2.1

These steps are applicable for 2.4.1 and 2.4.2 ThingsBoard versions.
>>>>>>> master

### Ubuntu/CentOS {#ubuntucentos-17}

#### ThingsBoard软件包下载

{% capture tabspec %}thingsboard-download-2-4-2
thingsboard-download-2-4-2-ubuntu,Ubuntu,shell,resources/2.4.2.1/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.4.2.1/thingsboard-ubuntu-download.sh
thingsboard-download-2-4-2-centos,CentOS,shell,resources/2.4.2.1/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.4.2.1/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard服务升级

{% capture tabspec %}thingsboard-installation-2-4-2
thingsboard-installation-2-4-2-ubuntu,Ubuntu,shell,resources/2.4.2.1/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.4.2.1/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-4-2-centos,CentOS,shell,resources/2.4.2.1/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.4.2.1/thingsboard-centos-installation.sh{% endcapture %}  
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
$ sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=2.4.1 
```

#### 启动服务

```bash
$ sudo service thingsboard start
```

### Windows {#windows-17}

#### ThingsBoard软件包下载

<<<<<<< HEAD
下载适用于Windows的ThingsBoard安装包: [thingsboard-windows-2.4.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.4.1/thingsboard-windows-2.4.1.zip).
=======
Download ThingsBoard installation archive for Windows: [thingsboard-windows-2.4.2.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.4.2.1/thingsboard-windows-2.4.2.1.zip).
>>>>>>> master

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
C:\thingsboard>upgrade.bat --fromVersion=2.4.1
```

<<<<<<< HEAD
#### 启动服务
=======
#### Start the service

```text
net start thingsboard
```


## Upgrading to 2.4.1

These steps are applicable for 2.4.0 ThingsBoard version.

### Ubuntu/CentOS {#ubuntucentos-16}

#### ThingsBoard package download

{% capture tabspec %}thingsboard-download-2-4-1
thingsboard-download-2-4-1-ubuntu,Ubuntu,shell,resources/2.4.1/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.4.1/thingsboard-ubuntu-download.sh
thingsboard-download-2-4-1-centos,CentOS,shell,resources/2.4.1/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.4.1/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard service upgrade

{% capture tabspec %}thingsboard-installation-2-4-1
thingsboard-installation-2-4-1-ubuntu,Ubuntu,shell,resources/2.4.1/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.4.1/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-4-1-centos,CentOS,shell,resources/2.4.1/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.4.1/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**NOTE:** Package installer will ask you to merge your thingsboard configuration. It is preferred to use **merge option** to make sure that all your previous parameters will not be overwritten.  
Please make sure that you set **database.entities.type** and **database.ts.type** parameters values (in the file **/etc/thingsboard/conf/thingsboard.yml**) to "cassandra" instead of "sql" in order to upgrade your cassandra database:
 
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

#### Start the service

```bash
$ sudo service thingsboard start
```

### Windows {#windows-16}

#### ThingsBoard package download

Download ThingsBoard installation archive for Windows: [thingsboard-windows-2.4.1.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.4.1/thingsboard-windows-2.4.1.zip).

#### ThingsBoard service upgrade

* Stop ThingsBoard service if it is running.
 
```text
net stop thingsboard
```

* Make a backup of previous ThingsBoard configuration located in \<ThingsBoard install dir\>\conf (for ex. C:\thingsboard\conf).

* Remove ThingsBoard install dir.
* Unzip installation archive to ThingsBoard install dir.
* Compare your old ThingsBoard configuration files (from the backup you made in the first step) with new ones.
* Please make sure that you set **database.entities.type** and **database.ts.type** parameters values (in the file **\<ThingsBoard install dir\>\conf\thingsboard.yml**) to "cassandra" instead of "sql" in order to upgrade your cassandra database:
  
  ```
  database:
    entities:
      type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
    ts:
      type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
  ```       

* Run **upgrade.bat** script to upgrade ThingsBoard to the new version.

**NOTE** Scripts listed above should be executed using Administrator Role.

```text
C:\thingsboard>upgrade.bat --fromVersion=2.4.0
```

#### Start the service

```text
net start thingsboard
```

## Upgrading to 2.4.0 

These steps are applicable for 2.3.1 ThingsBoard version.

### Ubuntu/CentOS {#ubuntucentos-15}

#### ThingsBoard package download

{% capture tabspec %}thingsboard-download-2-4-0
thingsboard-download-2-4-0-ubuntu,Ubuntu,shell,resources/2.4.0/thingsboard-ubuntu-download.sh,/docs/user-guide/install/resources/2.4.0/thingsboard-ubuntu-download.sh
thingsboard-download-2-4-0-centos,CentOS,shell,resources/2.4.0/thingsboard-centos-download.sh,/docs/user-guide/install/resources/2.4.0/thingsboard-centos-download.sh{% endcapture %}  
{% include tabs.html %}

#### ThingsBoard service upgrade

{% capture tabspec %}thingsboard-installation-2-4-0
thingsboard-installation-2-4-0-ubuntu,Ubuntu,shell,resources/2.4.0/thingsboard-ubuntu-installation.sh,/docs/user-guide/install/resources/2.4.0/thingsboard-ubuntu-installation.sh
thingsboard-installation-2-4-0-centos,CentOS,shell,resources/2.4.0/thingsboard-centos-installation.sh,/docs/user-guide/install/resources/2.4.0/thingsboard-centos-installation.sh{% endcapture %}  
{% include tabs.html %}

**NOTE:** Package installer will ask you to merge your thingsboard configuration. It is preferred to use **merge option** to make sure that all your previous parameters will not be overwritten.  
Please make sure that you set **database.entities.type** and **database.ts.type** parameters values (in the file **/etc/thingsboard/conf/thingsboard.yml**) to "cassandra" instead of "sql" in order to upgrade your cassandra database:
 
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

#### Start the service

```bash
$ sudo service thingsboard start
```

### Windows {#windows-15}

#### ThingsBoard package download

Download ThingsBoard installation archive for Windows: [thingsboard-windows-2.4.zip](https://github.com/thingsboard/thingsboard/releases/download/v2.4/thingsboard-windows-2.4.zip).

#### ThingsBoard service upgrade

* Stop ThingsBoard service if it is running.
 
```text
net stop thingsboard
```

* Make a backup of previous ThingsBoard configuration located in \<ThingsBoard install dir\>\conf (for ex. C:\thingsboard\conf).

* Remove ThingsBoard install dir.
* Unzip installation archive to ThingsBoard install dir.
* Compare your old ThingsBoard configuration files (from the backup you made in the first step) with new ones.
* Please make sure that you set **database.entities.type** and **database.ts.type** parameters values (in the file **\<ThingsBoard install dir\>\conf\thingsboard.yml**) to "cassandra" instead of "sql" in order to upgrade your cassandra database:
  
  ```
  database:
    entities:
      type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
    ts:
      type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
  ```       

* Run **upgrade.bat** script to upgrade ThingsBoard to the new version.

**NOTE** Scripts listed above should be executed using Administrator Role.

```text
C:\thingsboard>upgrade.bat --fromVersion=2.3.1
```

#### Start the service
>>>>>>> master

```text
net start thingsboard
```

<<<<<<< HEAD
## 下一步
=======


## Next steps
>>>>>>> master

{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}
