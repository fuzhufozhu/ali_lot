# What is service subscription? {#concept_d2m_g2v_y2b .concept}

A server can directly subscribe to messages under a product: device upstream notifications, device status change notifications, notifications of sub-devices reported by gateway devices, device lifecycle change notifications, and topological relationship change notifications. After you configure the Service Subscription function, IoT Platform forwards the subscribed messages from all devices under the product to your server. Two subscription methods are supported. One is to forward data through HTTP/2 channels to your servers and the other is to push message to your Message Service instances.

## Scenarios {#section_r1m_kcf_2hb .section}

Service Subscription is applicable to scenarios where only data receiving is involved. The following conditions must also be met:

-   The server must receive subscribed data from all devices under the product.
-   Device data is transmitted at a rate of up to 5,000 messages per second.

## HTTP/2-based message subscription {#section_mpx_l4f_2hb .section}

The new version of IoT Platform can push messages over HTTP/2 channels. After you configure HTTP/2-based message subscription for a product, IoT Platform will push the subscribed messages of all devices under the product to your server through the HTTP/2 channel.

Data forwarding workflow for HTTP/2-based subscription:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18849/155797769112665_en-US.png)

The server can receive messages directly from IoT Platform by connecting the HTTP/2 SDK to IoT Platform. The HTTP/2 SDK provides identity authentication, topic subscription, and message sending and message receiving capabilities.

-   The HTTP/2 SDK on the server is used to transfer a large number of messages between IoT Platform and the server.

-   The HTTP/2 SDK on the device is used to transfer messages between devices and IoT Platform.


**Note:** Only Java and .NET SDKs are supported.

For information about how to configure HTTP/2 channels and configuration examples, see:

-   [Limits](reseller.en-US/User Guide/Create products and devices/Service Subscription/Limits.md#)
-   [Development guide for Java HTTP/2 SDK](reseller.en-US/User Guide/Create products and devices/Service Subscription/Development guide for Java HTTP__2 SDK.md#)
-   [Development guide for .NET HTTP/2 SDK](reseller.en-US/User Guide/Create products and devices/Service Subscription/Development guide for .NET HTTP__2 SDK.md#)

For information about comparisons between service subscription-based and rules engine-based data forwarding, see [Compare data forwarding solutions](reseller.en-US/User Guide/Rules/Data Forwarding/Compare data forwarding solutions.md#).

## Push messages to Message Service {#section_w1q_jxy_3hb .section}

IoT Platform pushes subscribed messages to Message Service. Your server applications listen to queues in Message Service to receive device messages.

For more information about how to use Message Service to subscribe to device messages, see [Use Message Service to subscribe to device messages](reseller.en-US/User Guide/Create products and devices/Service Subscription/Subscribe to device messages by using Message Service.md#).

**Note:** Message Service charges fees for receiving messages pushed by IoT Platform. For more information about the billing and usage of Message Service, see [Message Service documentation](https://partners-intl.aliyun.com/help/product/27412.htm).

