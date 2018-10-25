# Overview {#concept_g11_dcl_vdb .concept}

When your devices communicate using [topics](reseller.en-US/User Guide/Create products and devices/Topics/System-defined topics.md#), you can use the rule engine and write SQL expressions to process data in topics. You can also configure forwarding rules to send the processed data to other Alibaba Cloud services. For example:

-   You can forward the processed data to [RDS](https://partners-intl.aliyun.com/help/doc-detail/26092.htm), and [Table Store](https://partners-intl.aliyun.com/help/product/27278.htm) for storage.
-   You can forward the processed data to [Function Compute](https://partners-intl.aliyun.com/help/product/50980.htm)for event-driven computing.
-   You can forward the processed data to another topic to achieve M2M communication.
-   You can forward the processed data to [Message Service](https://partners-intl.aliyun.com/help/product/27412.htm)to ensure reliable use of data.

By using the rule engine, you will be provided with a complete range of services including data collection, computing, and storage without purchasing a distributed server deployment architecture.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7486/15404607342243_en-US.png)

**Note:** When using the rule engine, you need to pay attention to the following points:

-   The rule engine processes data based on topics. You can use the rule engine to process device data only when devices are communicating with each other by using topics.
-   The rule engine processes the data in topics using SQL.
-   SQL subqueries and the use of the LIKE operator are currently not supported.
-   Some functions are supported. For example, you can use `deviceName()` to obtain the name of the current device. For more information about the supported functions, see Function list.

