# RDS MySQL {#concept_snj_ss5_hfb .concept}

本章将为您介绍如何配置RDS MySQL云产品的各个配置项。

|配置项|描述|
|---|--|
|RDS名称|设置您的RDS名称|
|版本|目前支持以下三个版本的RDS MySQL-   5.5
-   5.6
-   5.7

|
|DB名称|数据库的database名称|
|用户名|登录数据库的用户名|
|用户密码|登录数据库的密码|
|字符集|数据库存储字符集-   utf-8
-   utf8mb4

|
|RDS实例类型|根据连接数及并发数选择不同RDS实例-   1核1G 连接数300 IOPS600
-   1核2G 连接数600 IOPS1000
-   2核4G 连接数1200 IOPS2000
-   2核8G 连接数2000 IOPS4000

|
|存储大小\(G\)|数据库存储实例大小|

|环境变量key|描述|
|-------|--|
|iot.hosting.rds.$\{rds.mysql.name\}.mysqlUser|用户名|
|iot.hosting.rds.$\{rds.mysql.name\}.mysqlPassword|用户密码|
|iot.hosting.rds.$\{rds.mysql.name\}.mysqlUrl|访问地址|

**说明：** 节点输出的数据都将放在自研节点的环境变量中，`${rds.mysql.name}`为节点配置中的RDS名称。

