# Architecture {#concept_txj_mnp_tdb .concept}

Devices connect to IoT Platform, and communicate with IoT Platform. IoT Platform can forward device data to other Alibaba Cloud services for storage and processing. This is the basis to build IoT applications.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7451/155797773142095_en-US.png)

## IoT SDK {#section_dsy_qcr_pgb .section}

IoT Platform provides multiple device SDKs to help you develop your devices and connect them to IoT Platform. After a device is integrated with a device SDK, you can securely connect the device to IoT Platform and use features such as device management, data analytics, and data forwarding.

Only devices that support the TCP/IP protocol can integrate with the provided SDKs.

For more information, see [Device SDK Developer Guide](../../../../reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#).

## IoT Hub {#section_um1_pcr_pgb .section}

IoT Hub helps devices connect to Alibaba Cloud IoT Platform. IoT Hub acts as a data channel for secure communications between devices and IoT Platform. IoT Hub supports both the Pub/Sub and RRPC communication modes. The Pub/Sub mode is a topic-based message routing mode.

IoT Hub has the following features:

-   High scalability: Supports linear dynamic scaling, and allows up to one billion devices to connect to IoT Platform simultaneously.
-   End-to-end encryption: The entire communication link is encrypted with RSA or AES to ensure secure data transmission.
-   Real-time messages: After a data channel is established between a device and IoT Hub, the channel becomes a persistent connection that can minimize the handshake time and ensure real-time arrival of messages.
-   Support for data passthrough: IoT Hub supports binary data passthrough to the IoT Platform server. To keep data manageable and secure, IoT Hub does not store device data.
-   Support for multiple communication modes: IoT Hub supports both the Pub/Sub and RRPC communication modes to meet your communication needs in various scenarios.
-   Support for multiple protocols: Allows you to use the CoAP, MQTT, or HTTPS protocol to connect devices to IoT Platform.

## Device management {#section_h1b_4cr_pgb .section}

IoT Platform provides you with multiple device management services, including: lifecycle, [device grouping](../../../../reseller.en-US/User Guide/Create products and devices/Device group.md#), [device shadow](../../../../reseller.en-US/User Guide/Device shadows/Device Shadow overview.md#), [TSL models](../../../../reseller.en-US/User Guide/Create products and devices/TSL/Overview.md#), [data parsing](../../../../reseller.en-US/User Guide/Create products and devices/Data parsing.md#), [data storage](../../../../reseller.en-US/User Guide/Create products and devices/Manage files.md#), [online debugging](../../../../reseller.en-US/User Guide/Monitoring and Maintenance/Online debug/Debug applications using virtual devices.md#), [firmware upgrade](../../../../reseller.en-US/User Guide/Monitoring and Maintenance/Firmware update.md#), [remote configuration](../../../../reseller.en-US/User Guide/Monitoring and Maintenance/Remote configuration.md#), and real-time monitoring. For more information, see related documentation.

## Data forwarding {#section_ikb_ncr_pgb .section}

When a device communicates with IoT Platform by using a topic, you can write an SQL expression to process the data in the topic. You can then configure rules to forward data to other topics or other Alibaba Cloud services for processing and storage. Examples:

-   Forward data to ApsaraDB for RDS and Table Store for storage.
-   Forward data to Function Compute for event computing.
-   Forward data to Message Service for consumption of highly available data.
-   Forward data to another topic to implement M2M communication.

## Security authentication and authorization policies {#section_wvv_lcr_pgb .section}

Security is very important to IoT. Alibaba Cloud IoT Platform provides multiple security policies to ensure secure communications between devices and IoT Platform.

-   IoT Platform issues a unique certificate to each device. Each device uses its unique certificate for authentication when they try to connect to IoT Platform.
-   IoT Platform provides multiple device authentication methods for developers to address different security needs and production requirements.
-   IoT Platform grants permissions on a device basis. A device can publish messages and subscribe only to its own topic. IoT Platform identifies user permissions according to AccessKey information and allows users to operate only on authorized topics.

