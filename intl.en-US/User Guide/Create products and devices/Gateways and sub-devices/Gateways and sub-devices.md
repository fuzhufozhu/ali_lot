# Gateways and sub-devices {#concept_ngv_knl_vdb .concept}

IoT Platform allows devices to connect to it directly, or be mounted as sub-devices to gateways that connect to IoT Platform.

## Gateways and devices {#section_xwh_fbc_wdb .section}

When you create a product, you must select a node type for the devices of the product. Currently, IoT Platform supports two node types, Device and Gateway.

-   Device: Devices of this node type cannot be mounted with sub-devices, but can be connected directly to the IoT Platform or be mounted as sub-devices to gateways.
-   Gateway: Devices of this node type can connect to IoT Platform directly and can be mounted with sub-devices. Gateways are then used to manage sub-devices, maintain topological relationships with sub-devices, and synchronize these topological relationships to IoT Platform.

The topological relationship between a gateway and its sub-devices is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12824/15687869642876_en-US.PNG)

## Connect gateways and sub-devices to IoT Platform {#section_yv4_1fc_wdb .section}

Once a gateway has been connected to IoT Platform, the gateway will synchronize its topological relationships with its sub-devices to IoT Platform. A gateway supports device authentication, message reporting, instruction receiving, and other communications with IoT Platform for all its sub-devices. That is, sub-devices are managed by their corresponding gateway.

1.  Develop the gateway and connect the gateway to Iot Platform.

    For more information about how to connect gateways to IoT Platform, see [Link Kit SDK](https://www.alibabacloud.com/help/product/93051.htm).

2.  You can connect sub-devices to IoT Platform using either of the following two methods:
    -   The [Unique-certificate-per-device authentication](../../../../reseller.en-US/Developer Guide (Devices)/Authenticate devices /Unique-certificate-per-device authentication.md#) method. This method requires you to install the device certificates \(namely, the ProductKey, DeviceName, and DeviceSecret\) in the physical sub-devices, and then connect the sub-devices to IoT Platform.
    -   The [Unique-certificate-per-product authentication](../../../../reseller.en-US/Developer Guide (Devices)/Authenticate devices /Unique-certificate-per-product authentication.md#) method. This method requires you to enable Dynamic Registration on the product details page and register devices in the IoT Platform console. Then, when a physical sub-device is being connected, the gateway will initiate a connection request to IoT Platform for the sub-device. IoT Platform then verifies the sub-device information. If the verification passes, IoT Platform will assign the DeviceSecret to the sub-device. The sub-device then receives all the required information \(namely, the ProductKey, DeviceName, and DeviceSecret\) to successfully connect to IoT Platform.

