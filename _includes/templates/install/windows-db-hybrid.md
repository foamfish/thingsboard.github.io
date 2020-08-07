{% capture hybrid-info %}
如果您计划生产或超过100万个数据采集率(> 5000 msg/sec)ThingsBoard团队建议使用Hybrid数据库方式。

在这种情况下ThingsBoard将在Cassandra中存储时间序列数据，同时继续将PostgreSQL用于主要实体（设备/资产/仪表板/客户）存储。
{% endcapture %}
{% include templates/info-banner.md content=hybrid-info %}

##### PostgreSQL安装

下载安装文件(PostgreSQL 9.6+或更高版本[此处](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads#windows)并按照安装说明进行操作。

在PostgreSQL安装时系统将提示您输入超级用户(postgres)密码。

<<<<<<< HEAD
请一定牢记此密码，为了方便记忆我们将密码替换为postgres。
=======
Download the installation file (PostgreSQL 11.7 or newer releases) [here](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads#windows) and follow the installation instructions.
>>>>>>> master

##### 创建ThingsBoard数据库

安装成功后启动“pgAdmin”并使用超级用户(postgres)身份登录。

打开服务器并用“postgres”用户创建数据库“thingsboard”。

##### Cassandra安装

下面列出的说明将帮助您安装Cassandra。

- 下载DataStax社区版v3.0.9
    - [MSI安装(32位)](http://downloads.datastax.com/community/datastax-community-32bit_3.0.9.msi)
    - [MSI安装(64位)](http://downloads.datastax.com/community/datastax-community-64bit_3.0.9.msi)
- Run downloaded MSI package. You are first presented with an initial welcome panel that identifies your installation package:
- 运行下载的MSI软件包。首先您会看到一个初始的欢迎面板，该面板标识您的安装软件包：

 ![image](/images/user-guide/install/windows/windows-cassandra-1.png)
 
- 单击Next将进入到最终用户许可协议：
 
 ![image](/images/user-guide/install/windows/windows-cassandra-2.png)
 
- 单击Next进入指定软件的安装位置：
   
 ![image](/images/user-guide/install/windows/windows-cassandra-3.png)

- 设置好安装目录后，安装程序将询问您如何处理将要安装的服务：

 ![image](/images/user-guide/install/windows/windows-cassandra-4.png)

- 单击Next开始安装过程：

 ![image](/images/user-guide/install/windows/windows-cassandra-5.png)
 
 ![image](/images/user-guide/install/windows/windows-cassandra-6.png)

- 最后询问您是否希望在该软件的新版本可用时进行更新：

 ![image](/images/user-guide/install/windows/windows-cassandra-7.png)
 
- 您可以在安装程序为您创建的“ DataStax Community Edition”程序组中找到已安装的接口：

 ![image](/images/user-guide/install/windows/windows-cassandra-8.png)
 
- Cassandra的主要接口是CQL（Cassandra查询语言）shell实用程序，可用于为新的Cassandra服务器执行CQL命令。

##### ThingsBoard配置

如果您已将PostgreSQL超级用户密码指定为"postgres"，则可以跳过此步骤。

以管理员用户身份打开记事本或其他编辑器（右键单击应用程序图标，然后选择“以管理员身份运行”）。

打开以下文件进行编辑（在文件选择对话框中选择“所有文件”而不是“文本文档”，编码为UTF-8）：

```text 
C:\Program Files (x86)\thingsboard\conf\thingsboard.yml
``` 
{: .copy-code}


找到"SQL DAO Configuration"代码块将postgres用户密码替换"postgres"：

```yml
# SQL DAO Configuration
spring:
  data:
    jpa:
      repositories:
        enabled: "true"
  jpa:
    open-in-view: "false"
    hibernate:
      ddl-auto: "none"
    database-platform: "${SPRING_JPA_DATABASE_PLATFORM:org.hibernate.dialect.PostgreSQLDialect}"
  datasource:
    driverClassName: "${SPRING_DRIVER_CLASS_NAME:org.postgresql.Driver}"
    url: "${SPRING_DATASOURCE_URL:jdbc:postgresql://localhost:5432/thingsboard}"
    username: "${SPRING_DATASOURCE_USERNAME:postgres}"
    password: "${SPRING_DATASOURCE_PASSWORD:YOUR_POSTGRES_PASSWORD_HERE}"
    hikari:
      maximumPoolSize: "${SPRING_DATASOURCE_MAXIMUM_POOL_SIZE:5}"
``` 
{: .copy-code}

找到“ DATABASE_TS_TYPE”参数。将“sql”替换为“ cassandra”。

```yml
    type: "${DATABASE_TS_TYPE:cassandra}" # cassandra OR sql (for hybrid mode, only this value should be cassandra)
```

您可以选择在“cassandra”代码块中调整参数。

```yml
# Cassandra driver configuration parameters
cassandra:
  # Thingsboard cluster name
  cluster_name: "${CASSANDRA_CLUSTER_NAME:Thingsboard Cluster}"
  # Thingsboard keyspace name
  keyspace_name: "${CASSANDRA_KEYSPACE_NAME:thingsboard}"
  # Specify node list
  url: "${CASSANDRA_URL:127.0.0.1:9042}"
  # Enable/disable secure connection
  ssl: "${CASSANDRA_USE_SSL:false}"
  # Enable/disable JMX
  jmx: "${CASSANDRA_USE_JMX:true}"
  # Enable/disable metrics collection.
  metrics: "${CASSANDRA_DISABLE_METRICS:true}"
  # NONE SNAPPY LZ4
  compression: "${CASSANDRA_COMPRESSION:none}"
  # Specify cassandra cluster initialization timeout in milliseconds (if no hosts available during startup)
  init_timeout_ms: "${CASSANDRA_CLUSTER_INIT_TIMEOUT_MS:300000}"
  # Specify cassandra claster initialization retry interval (if no hosts available during startup)
  init_retry_interval_ms: "${CASSANDRA_CLUSTER_INIT_RETRY_INTERVAL_MS:3000}"
  max_requests_per_connection_local: "${CASSANDRA_MAX_REQUESTS_PER_CONNECTION_LOCAL:32768}"
  max_requests_per_connection_remote: "${CASSANDRA_MAX_REQUESTS_PER_CONNECTION_REMOTE:32768}"
  # Credential parameters #
  credentials: "${CASSANDRA_USE_CREDENTIALS:false}"
  # Specify your username
  username: "${CASSANDRA_USERNAME:}"
  # Specify your password
  password: "${CASSANDRA_PASSWORD:}"
```

