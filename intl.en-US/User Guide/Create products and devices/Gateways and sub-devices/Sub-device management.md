# Sub-device management {#task_z1r_q1p_y2b .task}

You can add sub-devices to a gateway device, and send the TSL and the extended service information of the sub-devices to the gateway.

-   If the gateway connection protocol of a device is Modbus or OPC UA, before you connect the device to a gateway, you must create a corresponding sub-device channel for the gateway. For information about how to create sub-device channels, see the documentation about sub-device channels.
-   Products and devices created before September 4, 2018 can be added to gateways as sub-devices. You can then build their topological relationships, but you cannot use sub-device channels or other custom configurations.

1.  In the left-side navigation pane, click **Devices** \> **Device** .
2.  On the Devices page, find the gateway device for which you want to add sub-devices and click **View** corresponding to it. You are directed to the Device Details page.
3.  Click **Sub-device Management** \> **Add Sub-device**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18811/155505337510897_en-US.png)

4.  Enter the information of the sub-device in the dialog box. 

    |Parameter|Description|
    |---------|-----------|
    |Product|Select the name of the product for which the sub-device belongs.|
    |Device|Select the name of the device that you want to add as a sub-device.|
    |If the gateway connection protocol of the sub-device is Modbus, the following parameters are required.|
    |Associated Channel|Select a channel for the sub-device from the sub-device channels that you have created.|
    |Slave Station Number|Enter an integer in the range of 1 - 247.|
    |If the gateway connection protocol of the sub-device is OPC UA, the following parameters are required.|
    |Associated Channel|Select a channel for the sub-device from the sub-device channels that you have created.|
    |Node Path|Enter a node path. For example, Objects/Device1. In this example, Objects is a fixed root node, and Device1 is the name of the device node path. Use `/` to separate node names.|
    |If the gateway connection protocol of the sub-device is a custom protocol, you can set the following parameters.|
    |Associated Channel|Optional. Select a channel for the sub-device from the sub-device channels that you have created.|
    |Custom Configuration|If you have selected an associated channel, you must customize the configuration. The custom configuration must be in JSON format.|

5.  After you have added sub-devices to a gateway, go back to the details page of the gateway device and click **Send Configuration Data** to assign the TSLs and extended service information of the products \(to which the sub-devices belong\) and the gateway connection configurations to the gateway. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18811/155505337511200_en-US.png)

6.  On the details page of the sub-device, you can view the gateway device information. Click **Edit** to modify the configuration information.

-   If you want to develop your own device SDK and assign the configurations of sub-devices to the gateway device, see [Alink protocol](../../../../../reseller.en-US/Developer Guide (Devices)/Connect sub-devices to the cloud/Alink Protocol.md#).

