---
layout: docwithnav
assignees:
- vbabak
title: 贡献指南

---

* TOC
{:toc}

我们一直在寻找社区中有关如何改进ThingsBoard的反馈。
如果您有想法或要考虑一些新功能，请在ThingsBoard [**GitHub问题页面**](https://github.com/thingsboard/thingsboard/issues)新建一个问题。
请确保问题列表中没有相同的问题（或类似问题）。

在开始任何实施之前，请等待ThingsBoard团队对您的票进行评论。我们将尽快与您联系。

#### 必备工具

要构建和运行ThingsBoard实例，请确保已在系统上安装了**Java**和**Maven**。

请参考[**从源代码构建**](/docs/user-guide/install/building-from-source)部分，其中[**Java**](/docs/user-guide/install/building-from-source/#java)和[**Maven**](/docs/user-guide/install/building-from-source/#maven)安装过程进行了描述。

#### 分支并构建ThingsBoard代码库

完成所需工具的安装后，请克隆正式的[**ThingsBoard代码库**](https://github.com/thingsboard/thingsboard)分支。

现在您可以克隆分支项目的源代码。

**注意：** 我们稍后将以 **${TB_WORK_DIR}** 指代您已经克隆的代码库文件夹。

如果是首次在Windows上构建，则可能需要运行以下命令以确保所需的npm依赖项可用：
```bat 
npm install -g cross-env 
npm install -g webpack 
``` 

在将项目导入*IDE*之前请使用**Maven**工具从根文件夹中进行构建：

```bash
cd ${TB_WORK_DIR}
mvn clean install -DskipTests
```

使用*IDE*编译*application*模块时会生成必须的*protobuf*文件。

接下来，将该项目作为**Maven**项目导入到您最喜欢的*IDE*中。  
请参阅[**IDEA**](https://www.jetbrains.com/help/idea/2016.3/importing-project-from-maven-model.html)和[**Eclipse**](http://javapapers.com/java/import-maven-project-into-eclipse/)。

**注意：** 如果您使用的是Eclipse，则在将maven项目导入到IDE之后，建议您在**ui**项目上禁用Maven项目构建器。这将极大地提高Eclipse性能，因为它将避免Eclipse Maven构建生成node_modules目录（这是不必要的，只会导致Eclipse挂起）。为此，右键单击**ui**项目，转到**Properties-> Builders**，然后取消选中**Maven Project Builder**”复选框，然后单击**确定**。

#### 数据库

默认情况下，ThingsBoard使用嵌入式HSQLDB实例，这对于评估或开发目的非常方便。
  
另外，您可以配置平台以使用可伸缩的Cassandra数据库集群或各种SQL数据库。
如果您更喜欢使用SQL数据库，建议使用PostgreSQL。

##### [可选] SQL数据库：PostgreSQL

{% include templates/install/optional-db.md %}

请使用[此链接](https://wiki.postgresql.org/wiki/Detailed_installation_guides)获得PostgreSQL安装说明。

一旦安装了PostgreSQL，您可能想要创建一个新用户或设置主要用户的密码。

{% include templates/install/create-tb-db.md %}


##### [可选] NoSQL数据库：Cassandra

请参考适当的部分，在其中找到有关如何安装cassandra的说明：

 - [Cassandra安装在**Linux**上](/docs/user-guide/install/linux/#cassandra)
 - [Cassandra安状在**Windows**上](/docs/user-guide/install/windows/#cassandra)

##### [可选]配置ThingsBoard以使用外部数据库
 
{% include templates/install/optional-db.md %} 
 
编辑ThingsBoard配置文件：

```text
/application/src/main/resources/thingsboard.yml
```

{% include templates/disable-hsqldb.md %} 

对于**PostgreSQL**：

{% include templates/enable-postgresql.md %} 

对于**Cassandra DB**：

找到并将数据库类型配置参数设置为‘cassandra’。
 
```text
database:
  entities:
    type: "${DATABASE_ENTITIES_TYPE:cassandra}" # cassandra OR sql
  ts:
    type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```

**注意：** 如果您的Cassandra服务器已安装在远程计算机上，或者已绑定到自定义接口/端口，则还需要在Thingsboard.yml中进行指定。
请转至[**配置指南**](/docs/user-guide/install/config/)以获取**thingsboard.yml**文件的详细说明以及用于cassandra连接配置的属性。

Thingsboard.yml文件更新后，请重新编译应用程序模块，以便将更新后的Thingsboard.yml值生效：

```bash
cd ${TB_WORK_DIR}/application
mvn clean install -DskipTests
```

##### 创建数据库结构并生成演示数据

为了创建数据库表请执行以下命令：

在 *Linux*:

```bash
cd ${TB_WORK_DIR}/application/target/bin/install
chmod +x install_dev_db.sh
./install_dev_db.sh
```

在 *Windows*:

```bat
cd %TB_WORK_DIR%\application\target\windows
install_dev_db.bat
```

#### 运行开发环境

##### 在热部署模式下运行UI容器。

默认情况下ThingsBoard UI通过8080使用端口。但是您可能要在热重新部署模式下运行UI。

**注意：** 此步骤是可选的仅当您要更改UI时才需要。
 
要以热部署模式启动UI容器，您需要先安装 **node.js**。安装**node.js**后，您可以通过执行下一个命令来启动容器：

```bash
cd ${TB_WORK_DIR}/ui
mvn clean install -P npm-start
```

这将启动一个特殊的服务器，该服务器将侦听3000端口。所有REST API和websocket请求都将转发到8080端口。

##### 运行服务器端容器

要启动服务器端容器，您可以使用几个选项。

首先，您可以运行*IDE*中*application*模块中的**org.thingsboard.server.ThingsboardServerApplication**类的main方法。

其次是，您可以从命令行使用**Spring boot**启动应用程序服务器：

```bash
cd ${TB_WORK_DIR}
java -jar application/target/thingsboard-${VERSION}-boot.jar
```

##### 运行

导航到http://localhost:3000/或http://localhost:8080/ 并使用演示数据凭据登录ThingsBoard：

 - *用户名* **tenant@thingsboard.org**
 - *密码* **tenant**

确保您能够登录并且一切都已正确启动。

#### 代码更改

现在您准备开始对代码库进行一些更改。
更新服务端或UI代码。
从用户角度验证所做的更改是否满足您的要求和期望。

##### 生成验证

在将更改提交到远程存储库之前，请使用*Maven*运行测试以在本地进行构建：

```bash
mvn clean install
```

确保构建正常并且所有测试均成功。

##### 将更改推送到您的分支

完成代码更改后，提交并使用一些有意义的注释将其推送到您的分支代码库中：

```bash
git commit -m '有意义的说明'
git push origin master
```

##### 创建请求请求

请默认将拉取请求创建到**master**分支中（如果需要，在github问题讨论的初始阶段将提供其他*branch*名称）。

如果由于提交前新事物已进入ThingsBoard master分支而发生冲突，请解决这些冲突以继续。

签署贡献许可协议（CLA），并验证远程构建是否成功。使用github CLA机器人对CLA进行了签名。
 
 ![image](/images/user-guide/pr_cla.png)

请耐心等待，拉取请求可能需要几天的时间进行审核。



#### 了解更多

- [规则节点开发指南](/docs/user-guide/contribution/rule-node-development/) 介绍如何创建自己的规则节点。

- [部件开发指南](/docs/user-guide/contribution/widgets-development/) 介绍如何创建自己的窗口小部件。

## 下一步

{% assign currentGuide = "Contribution" %}{% include templates/guides-banner.md %}