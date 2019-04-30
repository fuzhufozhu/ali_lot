# Device Shadow overview {#concept_r4r_b1v_wdb .concept}

IoT Platform provides the Device Shadow function to cache property information for a device. If the device is online, the device can directly receive commands from IoT Platform. If the device is offline, the device can actively request for cached commands from IoT Platform after it comes online again.

A device shadow is a JSON file that is used to store the reported status and desired status information for a device.

Each device has only one shadow. A device can obtain and set the shadow over MQTT for status synchronization. The synchronization is bi-directional, either from the shadow to the device or from the device to the shadow.

## Scenarios {#section_fdb_ftz_2hb .section}

-   **Scenario 1: In an unstable network, a device frequently disconnects from and reconnects to IoT Platform.**

    The device frequently disconnects from and reconnects to IoT Platform due to network instability. When an application that interacts with the device requests the current device status, the device is offline, which leads to a request failure. When the device is reconnected, the application fails to initiate another device status request.

    The Device Shadow function can synchronize with the device to update and store the latest device status information in the device shadow. The application can obtain the current device status information from the device shadow despite of the connection status.

-   **Scenario 2: Multiple applications simultaneously request the device status information.**

    In a stable network, a device must respond to each status request from multiple applications, even if the responses are the same. The device may be overloaded with the requests.

    By using the Device Shadow function, the device only needs to synchronize status information to the device shadow that is stored in IoT Platform. Applications can request the latest device status information from the device shadow instead of the target device. In this way, applications are decoupled from the device.

-   **Scenario 3: Device disconnection**

    -   In an unstable network, a device frequently disconnects from and reconnects to IoT Platform. When an application sends a control command to the device, the device is offline and the command fails to be dispatched to the device.
        -   Quality of Service 1 or 2 \(QoS 1 or 2\) may solve this issue. However, we recommend that you do not use this method. This method increases the workload of the server.
        -   By using the Device Shadow function, IoT Platform stores the control commands from the application to the device shadow. Each command is stored with the timestamp when the command was received. After the device is reconnected to IoT Platform, the device obtains these commands and checks the timestamp of each command to determine whether to run the command.
    -   A device goes offline and fails to receive commands from the application. When the device is reconnected, the device runs only the valid commands by checking the timestamp of each command that is pulled from the device shadow.

## View and update a device shadow {#section_p44_j1v_wdb .section}

You can view and update the shadow of a device in the IoT Platform console.

Procedure:

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  From the left-side navigation pane, choose **Devices** \> **Device**.
3.  Click **View** next to the corresponding device. The Device Details page appears.
4.  Click the **Device Shadow** tab.

    You can view the shadow that contains the latest information that is reported by the device.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7474/155661758341532_en-US.png)

5.  Click **Update Shadow**, and enter the desired status information in the "desired" section.

    For more information about the shadow file format, see [Device shadow JSON format](reseller.en-US/User Guide/Device shadows/Device shadow JSON format.md#).

    The device obtains the desired status information by subscribing to a specific topic. When the device is online, IoT Platform pushes the desired value to the device in real time.

    When the device is offline, the device's shadow caches the desired status information. After the device comes online again, it actively pulls the latest desired status information from IoT Platform.


## Related API operations {#section_zc4_mhb_fhb .section}

Obtain a device shadow: [GetDeviceShadow](../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Device shadows/GetDeviceShadow.md#)

Update a device shadow: [UpdateDeviceShadow](../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Device shadows/UpdateDeviceShadow.md#)

