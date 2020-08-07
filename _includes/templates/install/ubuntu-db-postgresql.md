{% capture postgresql-info %}
将PostgreSQL用于开发和生产环境ThingsBoard团队建议load(< 5000 msg/sec)。

许多云服务商都支持托管的PostgreSQL服务器这对于ThingsBoard实例而言都是一种经济高效的解决方案。
{% endcapture %}
{% include templates/info-banner.md content=postgresql-info %}

##### PostgreSQL安装

{% include templates/install/postgres-install-ubuntu.md %}

{% include templates/install/create-tb-db.md %}

##### ThingsBoard配置

编辑ThingsBoard配置文件

```bash 
sudo nano /etc/thingsboard/conf/thingsboard.conf
``` 
{: .copy-code}

将“PUT_YOUR_POSTGRESQL_PASSWORD_HERE”替换postgres用户真实密码

```bash
# DB Configuration 
export DATABASE_ENTITIES_TYPE=sql
export DATABASE_TS_TYPE=sql
export SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.PostgreSQLDialect
export SPRING_DRIVER_CLASS_NAME=org.postgresql.Driver
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/thingsboard
export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=PUT_YOUR_POSTGRESQL_PASSWORD_HERE
export SPRING_DATASOURCE_MAXIMUM_POOL_SIZE=5
# Specify partitioning size for timestamp key-value storage. Allowed values: DAYS, MONTHS, YEARS, INDEFINITE.
export SQL_POSTGRES_TS_KV_PARTITIONING=MONTHS
```
{: .copy-code}