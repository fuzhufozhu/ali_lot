# Compare data forwarding solutions {#concept_abq_hf1_kgb .concept}

In many scenarios, you must process the data that is reported by devices or use the data for business applications. You can forward device data by either using the IoT Platform service subscription or the rules engine data forwarding function. This topic compares the various data forwarding solutions and application scenarios that are supported by IoT Platform to help you select a forwarding solution that best suits your needs.

## Data forwarding solutions {#section_phg_l3c_kgb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/92223/155712256036895_en-US.png)

IoT Platform supports the following functions for data forwarding:

-   Rules engine data forwarding: Provides basic data filtering and processing capabilities. You can configure forwarding rules to filter and process device data and then forward the data to other Alibaba Cloud services.
-   Service subscription: Obtains device data directly from HTTP/2 clients. You can quickly obtain device data without being filtered and processed. This function is simple, easy to use, and efficient.

## Compare rules engine data forwarding and service subscription {#section_yzk_zqh_kgb .section}

|Forwarding function|Scenarios|Advantages and disadvantages|Restrictions|
|:------------------|:--------|:---------------------------|:-----------|
|Rules engine data forwarding| -   Complex scenarios
-   High-throughput scenarios

 | Advantages:

 -   Full fledged.
-   Allows you to modify forwarding rules while the rules are running.
-   Supports data filtering and processing.
-   Allows you to forward data to other Alibaba Cloud services.

The [Rules engine-based solutions](#) table briefly compares solutions that use the rules engine for forwarding data to different Alibaba Cloud services.


 Disadvantages:

 -   Complex to use. Users must write SQL expressions and configure forwarding rules.

 |See [Limits for data forwarding](../../../../reseller.en-US/Product Introduction/Limits.md#).|
|Service subscription| -   Scenarios that simply involves data receiving.
-   Scenarios that meet the following requirements:
    -   IoT Platform receives all device data.
    -   The device SDK is developed based on the Java or .NET language.
    -   The devices forwards data at a maximum rate of 5,000 queries per second \(QPS\).

 | Advantages:

 -   Easy to use.

 Disadvantages:

 -   Lack of the filtering capability.
-   Limited language support for the SDK.

 |See [Limits for service subscription](reseller.en-US/User Guide/Create products and devices/Service Subscription/Limits.md#).|

|Forwarding destination|Scenarios|Advantages|Disadvantages|
|:---------------------|:--------|:---------|:------------|
|Message Service \(MNS\)|Device data requires complex or refined processing. Scenarios where the transmit rate is slower than 1,000 QPS.

 | -   Uses the HTTPS protocol.
-   Allows IoT Platform to forward data on the Internet with high performance.

 |Provides performance slightly lower than MQ for RocketMQ.|
|ApsaraDB for RDS|Data storage scenarios.|Writes data directly to databases.|N/A|
|Table Store|Data storage scenarios.|Writes data directly into Table Store instances.|N/A|
|Function Compute|Scenarios where the device development process must be simplified and device data must be processed in a flexible way.| -   Great flexibility in data processing.
-   Multiple functions.
-   Do not require deployment.

 |Higher costs.|

## Service subscription {#section_kt2_mlc_kgb .section}

Business servers can subscribe to all types of messages by using the SDK.

|Restrictions|Guidelines|References|
|:-----------|:---------|:---------|
| -   Only the SDK in Java 8 or later and the .NET SDK are supported.
-   Service subscription does not support filtering messages. It receives all subscribed messages from the devices.
-   The transmit rate is up to 1,000 QPS. If your business requires a higher QPS, open a ticket and describe your requirements.

 For more information about service subscription restrictions, see [Limits](reseller.en-US/User Guide/Create products and devices/Service Subscription/Limits.md#).

 | -   Scenarios where the maximum transmit rate is no higher than 5,000 QPS.
-   Make sure that you are fully aware of any impacts of data loss and data delay on your business. Protect important data in the business layer.
-   Service subscription does not apply to scenarios where data filtering and fine-grained processing are required. We recommend that you use the rules engine data forwarding for these scenarios.

 | -   [What is service subscription](reseller.en-US/User Guide/Create products and devices/Service Subscription/What is Service Subscription?.md#)
-   [Development guide for the Java SDK](reseller.en-US/User Guide/Create products and devices/Service Subscription/Development guide for Java HTTP__2 SDK.md#)
-   [Development guide for the .NET SDK](reseller.en-US/User Guide/Create products and devices/Service Subscription/Development guide for .NET HTTP__2 SDK.md#)
-   [Best practices](../../../../reseller.en-US/Best Practices/Configure service subscription.md#)

 |

## Forward data to Message Service {#section_k2v_p5c_kgb .section}

The rules engine enables IoT Platform to forward messages in specific topics to the topics in Message Service. Message Service can then receive these messages by using the Message Service SDK. Message Service allows access from the public network but it provides a lower performance than RocketMQ. We recommend that you use Message Service for scenarios where the transmit rate is lower than 1,000 QPS.

|Restrictions|Guidelines|References|
|:-----------|:---------|:---------|
| |When a message fails to be forwarded by using the rules engine after making the maximum retries, the message will be dropped. Message-oriented services may have delay issues. Make sure that you are fully aware of the impacts of data loss or delay on your business.| -   [Create and configure a rule](reseller.en-US/User Guide/Rules/Data Forwarding/Create and configure a rule.md#)
-   [Forward data to Message Service](reseller.en-US/User Guide/Rules/Data Forwarding Examples/Forward data to Message Service.md#)
-   
 |

## Forward data to Function Compute {#section_wcc_4vc_kgb .section}

The rules engine enables IoT Platform to forward messages in specific topics to Function Compute. Developers can then further process the messages. Function Compute does not require deployment, which simplifies business development.

|Restrictions|Guidelines|References|
|:-----------|:---------|:---------|
|See [Function Compute limits](https://partners-intl.aliyun.com/help/doc-detail/51907.htm).| -   Applicable to scenarios where users can customize data processing or are required to simplify the development and operation processes.
-   When a message fails to be forwarded by using the rules engine after making the maximum retries, the message will be dropped. Make sure that you are fully aware of any impacts of data loss or delay on your business.

 | -   [Create and configure a rule](reseller.en-US/User Guide/Rules/Data Forwarding/Create and configure a rule.md#)
-   [Forward data to Function Compute](reseller.en-US/User Guide/Rules/Data Forwarding Examples/Forward data to Function Compute.md#)
-   
 |

