# Gateways and sub-devices {#concept_ngv_knl_vdb .concept}

IoT Platform allows devices to connect to it directly. Devices can also be mounted as sub-devices to a gateway that connects to IoT Platform.

## Gateways and devices {#section_xwh_fbc_wdb .section}

When creating products and devices, you need to select a node type. IoT Platform currently supports two node types, device and gateway.

-   Device: refers to a device to which sub-devices cannot be mounted. Devices can connect directly to the IoT Hub. Alternatively, devices can connect as sub-devices mounted to gateways that are connected to the IoT Hub.
-   Gateway: refers to a device to which sub-devices can be mounted. A gateway connects sub-devices to IoT Platform. Gateways can manage sub-devices, maintain their topological relationships with sub-devices, and synchronize these topological relationships to the cloud.

## Topological relationship between a gateway and its sub-devices {#section_yv4_1fc_wdb .section}

![](images/2876_en-US.PNG "Device topological relationship")

After creating products and devices, you can:

-   Connect the gateway to IoT Platform and use the gateway to synchronize the topologial relationship with the cloud. The gateway is then responsible for device authentication, message uploading, instruction receiving and other communications with IoT Platform for all sub-devices. Please refer to [设备开发指南](reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#) and [Connect sub-devices to IoT Platform](reseller.en-US/Developer Guide (Devices)/C-SDK/Connect sub-devices to the cloud/Connect sub-devices to IoT Platform.md#) for details.
-   Configure the sub-device communication channels in the console, manage the topological relationships and send the configuration details to the gateway. Please refer to [Sub-device channels](reseller.en-US/User Guide/Create products and devices/Gateways and sub-devices/Sub-device channels.md#) and [Sub-device management](reseller.en-US/User Guide/Create products and devices/Gateways and sub-devices/Sub-device management.md#) for details.

