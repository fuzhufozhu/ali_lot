# MySQL {#concept_cqz_xr5_hfb .concept}

本章将为您介绍如何配置开源MySQL的各个配置项。

|配置项|描述|
|---|--|
|服务名称|容器服务中的servicename，其他节点可通过该名称进行调用|
|root密码|root用户密码，必须包含英文大小写字母和数字|
|用户名|登录数据库的用户名|
|用户密码|登录数据库的密码，必须包含英文大小写字母和数字|
|数据库名称|数据库的database名称|
|memory配额|节点占用的内存配额，单位为Mi或Gi，Mi=1024\*1024，Gi=1024\*Mi|
|cpu配额|节点占用的CPU配额，单位m，m表示千分之一的CPU资源|
|数据存储类型|数据存储的类型-   alicloud-disk-efficiency（阿里云高效磁盘）

|
|数据文件大小|按需选择数据存储的大小-   50G
-   100G
-   200G

|
|初始化sql语句|写入数据库初始化语句，多个语句间用分号隔离|
|配置数据库cnf|写入数据库配置语句|

|环境变量key|描述|
|-------|--|
|iot.hosting.$\{k8s.mysql.name\}.mysqlUser|用户名|
|iot.hosting.$\{k8s.mysql.name\}.mysqlPassword|用户密码|
|iot.hosting.$\{k8s.mysql.name\}.mysqlUrl|访问地址|

**说明：** 节点输出的数据都将放在自研节点的环境变量中，`${k8s.mysql.name}`为节点配置中的服务名称。

