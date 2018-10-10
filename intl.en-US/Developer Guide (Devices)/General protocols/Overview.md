# Overview {#concept_d4s_jcv_42b .concept}

The Alibaba Cloud IoT Platform already supports MQTT, CoAP, HTTP and other common protocols, yet fire protection agreement GB/T 26875.3-2011, Modbus and JT808 is not supported, and in some specialized cases, devices may not be able to connect to IoT Platform. At this point, you need to use general protocol SDK to quickly build a bridge between your devices and platform to Alibaba Cloud IoT Platform, allowing two-way data communication.

## General protocol SDK {#section_f1g_w2w_42b .section}

The general protocol SDK is a protocol self-adaptive framework, using for high-efficiency bi-directional communication between your devices or platform to IoT Platform.  The SDK architecture is shown below:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16467/15391536518136_en-US.png)

General protocol provides two SDKs: Core SDK and Server SDK.

-   **General protocol core SDK**

    Core SDK abstracts abilities like session and configuration management. It acts like a net bridge between devices and IoT Platform and communicates with the Platform in representation of devices. This greatly simplifies the development of IoT Platform. Its main features include:

    -   provides non-persistent session management capabilities.
    -   provides configuration management capabilities based on configuration files.
    -   provides connection management capabilities.
    -   provides upstream communication capability.
    -   provides downstream communication capabilities.
    -   supports device authentication.
    If your devices are already connected to the internet and you want to build a bridge between IoT Platform and your devices or existing platform, choose core SDK.

-   **General protocol server SDK**

    Server SDK provides device connection service on the basis of core SDK function. Its main features include:

    -   supports any protocol that is based on TCP/UDP.
    -   supports TLS/SSL encryption for transmission.
    -   supports horizontal expansion of the capacity of device connection.
    -   provides Netty-based communication service.
    -   provides automated and customizable device connection and management capability.
    If you want to build the connection service from scratch, choose server SDK which provides socket for communication.


## Development and deployment {#section_g1g_w2w_42b .section}

**Create products and devices in IoT console**

Create products and devices in console. See [Create a product \(Pro Edition\)](../../../../reseller.en-US/User Guide/Create products and devices/Create a product (Pro Edition).md#) for more information. Acquire the ProductKey, DeviceName and DeviceSecret of the net bridge device you've just created.

**Note:** Net bridge is a virtual concept, and you can use the information of any device as device information of the net bridge.

**SDK dependency**

General protocol SDKs are currently only available in Java, and supports JDK 1.8 and later versions. Maven dependencies:

```
<! -- Core SDK -->
<dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>iot-as-bridge-sdk-core</artifactId>
    <version>1.0.0</version>
</dependency>

<! -- Server SDK -->
<dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>iot-as-bridge-sdk-server</artifactId>
    <version>1.0.0</version>
</dependency>

```

**Develop SDK**

[Core SDK](reseller.en-US/Developer Guide (Devices)/General protocols/Core SDK.md#)and [Server SDK](reseller.en-US/Developer Guide (Devices)/General protocols/Server SDK/Server SDK.md#) briefly introduces the development process. For detailed implementation, refer to javadoc.

**Deployment**

The completed bridge connection service can be deployed on Alibaba Cloud using services like [ECS](../../../../reseller.en-US/Product Introduction/What is ECS?.md#) and [SLB](../../../../reseller.en-US/Product Introduction/What is Server Load Balancer?.md#), or deployed in local environment to guarantee communication security.

The whole process \(if using Alibaba Cloud ECS to deploy\) is shown below:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16467/15391536518073_en-US.png)

