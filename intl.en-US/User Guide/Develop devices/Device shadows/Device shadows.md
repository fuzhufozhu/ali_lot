# Device shadows {#concept_r4r_b1v_wdb .concept}

A device shadow is a JSON file that is used to store the reported status and the desired status of the device.

-   Each device only has one device shadow. The device gets and sets the device shadow based on Message Queuing Telemetry Transport \(MQTT\). Therefore, the device shadow status and the device status can synchronize.
-   The application uses the SDK of IoT Platform to get and set the device shadow. Then, the application can obtain the latest device status from and deliver the desired status to the target device by using the device shadow.

## Scenario 1 {#section_zqs_g1v_wdb .section}

A device frequently disconnects from and reconnects to IoT Platform. This is caused by unstable network conditions. The application cannot obtain the device status when requesting the status from an offline device, and fails to send another device status request when the device is reconnected.

The device shadow can synchronize with the device to update and store the latest device status. Therefore, the application can obtain the current device status from the device shadow of an offline or online device.

## Scenario 2 {#section_dxg_31v_wdb .section}

A device has to respond to each status request when multiple applications request the status of this device in stable network conditions. Even if the responses are the same, the device may be overloaded when processing these requests.

On IoT Platform, the device synchronizes the status to the device shadow only. Applications can request the latest device status from the device shadow, instead of the target device. Therefore, applications are decoupled from the device.

## Scenario 3 {#section_p44_j1v_wdb .section}

-   A device frequently disconnects from and reconnects to IoT Platform. This is caused by unstable network conditions. A device that is in offline status cannot receive application commands.
    -   Quality of Service 1 or 2 \(QoS 1 or 2\) may solve this issue. However, we do not recommend that you use this method. This method increases the workload of the service.
    -   On IoT Platform, the device shadow stores the control commands that contain the timestamps when the application sends these commands. The device obtains these commands and checks their timestamps to determine whether to execute the commands when the device has reconnected to IoT Platform.
-   A device in offline status cannot receive the commands from the application. When the connection has recovered, the device executes valid commands by checking the timestamps of the device shadow commands.

