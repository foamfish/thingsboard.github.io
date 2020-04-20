{% capture hybrid-info %}
如果您计划生产或超过100万个数据采集率(> 5000 msg/sec)ThingsBoard团队建议使用Hybrid数据库方式。

在这种情况下ThingsBoard将在Cassandra中存储时间序列数据，同时继续将PostgreSQL用于主要实体（设备/资产/仪表板/客户）存储。
{% endcapture %}
{% include templates/info-banner.md content=hybrid-info %}

##### PostgreSQL安装

{% include templates/install/postgres-install-rhel.md %}

{% include templates/install/create-tb-db-rhel.md %}

##### Cassandra配置

{% include templates/install/cassandra-rhel-install.md %}

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
export DATABASE_TS_TYPE=cassandra
export SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.PostgreSQLDialect
export SPRING_DRIVER_CLASS_NAME=org.postgresql.Driver
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/thingsboard
export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=PUT_YOUR_POSTGRESQL_PASSWORD_HERE
``` 
{: .copy-code}

您可以选择添加以下参数来重新配置ThingsBoard实例以连接到外部Cassandra节点：

```bash
export CASSANDRA_CLUSTER_NAME=Thingsboard Cluster
export CASSANDRA_KEYSPACE_NAME=thingsboard
export CASSANDRA_URL=127.0.0.1:9042
export CASSANDRA_USE_CREDENTIALS=false
export CASSANDRA_USERNAME=
export CASSANDRA_PASSWORD=
```
{: .copy-code}