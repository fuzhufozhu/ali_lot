# Gateways and sub-devices {#concept_ngv_knl_vdb .concept}

IoT Platform allows devices to connect to it directly. Devices can also be mounted as sub-devices to a gateway that connects to IoT Platform.

## Gateways and devices {#section_xwh_fbc_wdb .section}

When creating products and devices, you need to select a node type. IoT Platform currently supports two node types, device and gateway.

-   Device: refers to a device to which sub-devices cannot be mounted. Devices can connect directly to the IoT Hub. Alternatively, devices can connect as sub-devices mounted to gateways that are connected to the IoT Hub.
-   Gateway: refers to a device to which sub-devices can be mounted. A gateway connects sub-devices to IoT Platform. Gateways can manage sub-devices, maintain their topological relationships with sub-devices, and synchronize these topological relationships to the cloud.

## Topological relationship between a gateway and its sub-devices {#section_yv4_1fc_wdb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12824/2876_en-US.png "Device topological relationship")

After you have created products and devices, if the topology contains gateways, the gateways need to synchronize the topological relationships to the cloud during the device development phase. The gateways will represent sub-devices to complete device authentication, message uploading, instruction receiving, and other communication with IoT Platform.  Sub-devices are centrally managed by the gateways. At this time:

1.  The gateway first connects to the cloud. Refer to [Develop devices](intl.en-US/User Guide/Develop devices.md#) for more information.
2.  The gateway then communicates with IoT Platform on behalf of its sub-devices. Refer to [Connect sub-devices to IoT Platform](intl.en-US/User Guide/Develop devices/Connect sub-devices to the cloud/Connect sub-devices to IoT Platform.md#) for more information.

