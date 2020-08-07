{% capture postgresql-info %}
将PostgreSQL用于开发和生产环境ThingsBoard团队建议load(< 5000 msg/sec)。

许多云服务商都支持托管的PostgreSQL服务器这对于ThingsBoard实例而言都是一种经济高效的解决方案。
{% endcapture %}
{% include templates/info-banner.md content=postgresql-info %}

##### PostgreSQL安装

下载安装文件(PostgreSQL 9.6+或更高版本[此处](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads#windows)并按照安装说明进行操作。

在PostgreSQL安装时系统将提示您输入超级用户(postgres)密码。

请一定牢记此密码，为了方便记忆我们将密码替换为postgres。

<<<<<<< HEAD
##### 创建ThingsBoard数据库
=======
Download the installation file (PostgreSQL 11.7 or newer releases) [here](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads#windows) and follow the installation instructions.
>>>>>>> master

安装成功后启动“pgAdmin”并使用超级用户(postgres)身份登录。

打开服务器并用“postgres”用户创建数据库“thingsboard”。

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

locate "SQL_POSTGRES_TS_KV_PARTITIONING" parameter in order to override the default value for timestamp key-value storage partitioning size:

```yml
    sql:
      postgres:
        # Specify partitioning size for timestamp key-value storage. Example: DAYS, MONTHS, YEARS, INDEFINITE.
        ts_key_value_partitioning: "${SQL_POSTGRES_TS_KV_PARTITIONING:MONTHS}"
```