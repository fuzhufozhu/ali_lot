# RDS SQL Server {#concept_u21_5s5_hfb .concept}

本章将为您介绍如何配置RDS SQL Server云产品的各个配置项。

|配置项|描述|
|---|--|
|RDS名称|设置您的RDS名称|
|版本|目前支持以下三个版本的RDS MySQL-   2012
-   2008r2

|
|DB名称|数据库的database名称|
|用户名|登录数据库的用户名|
|用户密码|登录数据库的密码|
|字符集|数据库存储字符集-   Chinese\_PRC\_CI\_AS

|
|RDS实例类型|根据连接数及并发数选择不同的RDS实例：-   2核4G
-   2核8G
-   4核8G
-   4核16G

|
|存储大小\(G\)|数据库存储实例大小|

|环境变量key|描述|
|-------|--|
|iot.hosting.rds.$\{rds.mssql.name\}.mssqlUser|用户名|
|iot.hosting.rds.$\{rds.mssql.name\}.mssqlPassword|用户密码|
|iot.hosting.rds.$\{rds.mssql.name\}.mssqlUrl|访问地址|

**说明：** 节点输出的数据都将放在自研节点的环境变量中，`${rds.mssql.name}`为节点配置中的RDS名称。

