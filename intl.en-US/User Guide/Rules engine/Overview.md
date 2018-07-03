# Overview {#concept_g11_dcl_vdb .concept}

When your devices communicate using [topics](intl.en-US/User Guide/Create products and devices/Topics/System-defined topics.md#), you can use the rule engine and write SQL expressions to process data in topics. You can also configure forwarding rules to send the processed data to other Alibaba Cloud services. For example:

-   You can forward the processed data to [RDS](https://www.aliyun.com/product/rds/mysql?spm=a2c0j.8235941.765261.321.12721a22vfGB5L), [Table Store](https://www.aliyun.com/product/ots?spm=5176.7920929.765261.290.5c0b41d6FIk9JI), and [HiTSDB](https://www.aliyun.com/product/hitsdb?spm=5176.54465.765261.334.5d1461844Iybud) for storage.
-   You can forward the processed data to [DataHub](https://data.aliyun.com/product/datahub?spm=a2c0j.117599.588239.33333.7ac44d52aRzL8f) and use [StreamCompute](https://data.aliyun.com/product/sc?spm=5176.8142029.388261.375.39126d3ea17nR9) for stream computing and [MaxCompute](https://www.aliyun.com/product/odps?spm=5176.149792.765261.372.7f197e91pYwLJL) for large-scale offline calculations.
-   You can forward the processed data to [Function Compute](https://www.aliyun.com/product/fc?spm=5176.7944453.765261.265.2db352dfFQwGIn) for event-driven computing.
-   You can forward the processed data to another topic to achieve M2M communication.
-   You can forward the processed data to [MQ](https://www.aliyun.com/product/ons?spm=5176.137990.765261.383.6d95224eWTU5bG) to ensure reliable use of data.

By using the rule engine, you will be provided with a complete range of services including data collection, computing, and storage without purchasing a distributed server deployment architecture.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7486/2243_en-US.png)

**Note:** When using the rule engine, you need to pay attention to the following points:

-   The rule engine processes data based on topics. You can use the rule engine to process device data only when devices are communicating with each other by using topics.
-   The rule engine processes the data in topics using SQL.
-   SQL subqueries and the use of the LIKE operator are currently not supported.
-   Some functions are supported. For example, you can use `deviceName()` to obtain the name of the current device. For more information about the supported functions, see Function list.

