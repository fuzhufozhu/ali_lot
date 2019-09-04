# Overview {#concept_d4s_jcv_42b .concept}

Alibaba Cloud IoT Platform supports communication over MQTT, CoAP, or HTTP. Other types of protocols, such as the fire protection agreement GB/T 26875.3-2011, Modbus, and JT808, are not supported. In specific scenarios, some devices may not be able to directly connect to IoT Platform. You must use the generic protocol SDK to build a bridge for your devices or platforms with IoT Platform, so that they can communicate with each other.

## Architecture {#section_daj_tjk_l9l .section}

The generic protocol SDK is a self-adaptive protocol framework. This SDK is used to provide a bridge service for the bi-directional communication between IoT Platform and your devices or platforms.

The following figure shows the architecture.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16467/15675828698136_en-US.png)

## Scenarios {#section_ogk_ehn_tyh .section}

The generic protocol SDK can be applied to the following scenarios:

-   Your device cannot be directly connected to IoT Platform because of the network or hardware restrictions.
-   Your device supports only protocols that are not supported by IoT Platform.
-   A connection is already established between the device and your server. You want to connect the device to IoT Platform without modifying the device and protocol.
-   The device is directly connected to your server. Additional logic processing is required.

## Features {#section_tfj_mgf_4qk .section}

The generic protocol SDK enables the bridge server to communicate with IoT Platform.

Basic features:

-   Allows you to manage configurations based on a configuration file.
-   Allows you to manage device connections.
-   Provides upstream communication capabilities.
-   Provides downstream communication capabilities.

Advanced features:

-   Allows you to manage configurations based on interfaces.
-   Provides interfaces that can be called to report properties, events, and tags.

## Terms {#section_g9v_mtu_tuw .section}

|Term|Description|
|:---|:----------|
|device|The device in a real IoT scenario that cannot directly communicate with IoT Platform by using the protocols supported by IoT Platform.|
|bridge server|The server to which the device is connected. This server uses a specific protocol to communicate with the device and uses the generic protocol SDK to communicate with IoT Platform.|
|original protocol|The specific protocol used between the device and the bridge server. The generic protocol SDK does not involve the definition and implementation of the original protocol.|
|original device identifier|The unique identifier used by the device to communicate with the bridge server over the original protocol. Among the generic protocol SDK interface parameters, the originalIdentity parameter specifies the identifier of the device's original identity.|
|device certificate|The device certificate information obtained after you register the device with IoT Platform. The information includes ProductKey, DeviceName, and DeviceSecret. In a scenario that uses the generic protocol, you do not need to install the device certificate on the device. Instead, you must configure the generic protocol SDK file: devices.conf. The bridge maps the originalIdentity of the device to the device certificate.|
|bridge certificate|The device certificate information returned after you register the bridge device with IoT Platform. The information includes ProductKey, DeviceName, and DeviceSecret. The bridge certificate uniquely identifies the bridge in IoT Platform.|

## Development and deployment {#section_g1g_w2w_42b .section}

1.  Create products and devices.

    Log on to the IoT Platform console and create products and devices. For more information, see [Create a product](intl.en-US/User Guide/Create products and devices/Create a product.md#) and [Create a device](intl.en-US/User Guide/Create products and devices/Create devices/Create a device.md#) or [Create multiple devices at a time](intl.en-US/User Guide/Create products and devices/Create devices/Create multiple devices at a time.md#).

    Obtain the device certificate of the bridge. This certificate must be provided when you configure the generic protocol SDK.

    **Note:** Bridge is a virtual concept. You can use any device certificate as the certificate information of the bridge.

2.  Configure the generic protocol SDK.

    The generic protocol SDK supports only the Java language. Only JDK 1.8 and later versions are supported.

    For more information about how to configure the generic protocol SDK, see [Use the basic features](intl.en-US/User Guide/Generic protocol SDK/Use the basic features.md#).

3.  Deploy the bridge service.

    You can deploy a developed bridge service on Alibaba Cloud in a scalable manner by using Alibaba Cloud services such as [ECS](../../../../intl.en-US/Product Introduction/What is ECS?.md#) and [SLB](../../../../intl.en-US/Product Introduction/What is Server Load Balancer?.md#). You can also deploy the bridge service in local environment to guarantee secure communication.

    The following figure shows the procedures of using ECS to deploy the bridge service:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16467/15675828698073_en-US.png)


