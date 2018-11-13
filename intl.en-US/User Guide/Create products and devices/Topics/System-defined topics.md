# System-defined topics {#concept_ogz_vnl_vdb .concept}

This document lists the system-defined topics.

## **Topic categories in IoT Platform Basic** {#section_c1y_pb1_wdb .section}

IoT Platform Basic supports customized topic categories. IoT Platform automatically creates three customized topic categories by default when you create a product. You can also create additional customized topic categories.

-   `/${productKey}/${deviceName}/update`: for devices to upload data. Permission operation: Publish.
-   `/${productKey}/${deviceName}/update/error`: for devices to report errors. Permission operation: Publish.
-   `/${productKey}/${deviceName}/get`: for the cloud to get device data. Permission operation: Subscribe.

## Topic categories in IoT Platform Pro {#section_cf2_l21_wdb .section}

IoT Platform Pro provides default system-defined topics. Topic categories are not customizeable.  You can use the SDK, or use the system-defined topics associated with the Pub or Sub operations to enable two-way communication for device data between devices and the cloud.

The default topic category in IoT Pro has two categories: Alink and passthrough. The Alink topic category is used for device communication in the Alink JSON  format. The passthrough topic category is used for device communication in a pass-through/custom format.

Alink topic categories:

-   `/sys/${productKey}/${deviceName}/thing/event/property/post`: for devices to report properties. Permission operation: Publish.
-   `/sys/${productKey}/${deviceName}/thing/service/property/set`: for devices to set properties. Permission operation: Subscribe.
-   `/sys/${productKey}/${deviceName}/thing/event/{tsl.event.identifer}/post`: for devices to report events. Permission operation: Publish.
-   `/sys/${productKey}/${deviceName}/thing/service/{tsl.service.identifer}`: for devices to call services. Permission operation: Subscribe.
-   `/sys/${productKey}/${deviceName}/thing/deviceinfo/update`: for devices to report tags. Permission operation: Publish.

Passthrough topic categories:

-   `/sys/{ProductKey}/{deviceName}/thing/model/up_raw`: uploaded passthrough data for devices to report device properties, events, and other device data. Permission operation: Publish.
-   `/sys/{ProductKey}/{deviceName}/thing/model/down_raw`: downloaded passthrough data for devices to get properties, set properties, and call services. Permission operation: Subscribe.

## **Topic categories for device shadows** {#section_ofn_t31_wdb .section}

A device shadow provides the system-defined topic categories, which are mainly used by IoT Platform Basic products to update shadows.

-   `/shadow/update/${productKey}/${deviceName}`: for updating the device shadows. Permission operation: Publish.
-   `/shadow/get/${productKey}/${deviceName}`: for getting the device shadows. Permission operation: Subscribe.

## Topic categories for remote configuration {#section_blq_v1b_wdb .section}

Remote configuration provides the system-defined topic categories, which are used for sending configuration files to devices.

-   /`sys/${productKey}/${deviceName}/thing/config/push`: for devices to receive the configuration files from the cloud. Permission operation: Subscribe.
-   `/sys/${productKey}/${deviceName}/thing/config/get`: for devices to request a configuration update. Permission operation: Publish.
-   `/sys/${productKey}/${deviceName}/thing/config/get_reply`: for devices to request a configuration update, and receive the configuration files from the cloud. Permission operation: Subscribe.

## Topic categories for firmware upgrade {#section_rcj_3bb_wdb .section}

The firmware upgrade provides system-defined topic categories for devices to report firmware versions and receive upgrade notifications.

-   `/ota/device/inform/${productKey}/${deviceName}`: for devices to report firmware versions to the cloud. Permission operation: Publish.
-   `/ota/device/upgrade/${productKey}/${deviceName}`: for devices to receive firmware upgrade notifications from the cloud. Permission operation: Subscribe.
-   `/ota/device/progress/${productKey}/${deviceName}`: for devices to report the progress of a firmware upgrade. Permission operation: Publish.
-   `/ota/device/request/${productKey}/${deviceName}`: for devices to request a firmware upgrade. Permission operation: Publish.

## Topic categories for device broadcasts {#section_gx2_3cb_wdb .section}

System-defined topic categories are available for you to broadcast to devices. You can define recipient devices.

`/broadcast/${productKey}/+`: for devices to receive broadcasts. Permission operation: Subscribe. The wildcard character \(+\) can be used to define the recipient devices of the broadcast.

## **Topic categories for Revert-RPC communication** {#section_o43_pkc_b2b .section}

The IoT Hub has integrated the synchronous communication mode based on the open-source MQTT protocol. The server sends commands to devices to receive responses in real time. These topic categories are as follows:

-   `/sys/${YourProductKey}/${YourDeviceName}/rrpc/response/${messageId}`: for devices to respond to Revert-RPC requests. Permission operation: Publish.
-   `/sys/${YourProductKey}/${YourDeviceName}/rrpc/request/+`: for devices to subscribe to Revert-RPC requests. Permission operation: Subscribe.

