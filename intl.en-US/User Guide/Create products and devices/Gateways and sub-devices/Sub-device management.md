# Sub-device management {#task_z1r_q1p_y2b .task}

You can add a sub-device to a gateway device and assign the TSL and the extended information of the product \(to which the sub-device belongs\) to the sub-device.

-   If the gateway connection protocol of a sub-device is Modbus or OPC UA, before you connect the sub-device to the gateway, you must create a corresponding sub-device channel for the gateway. For more information about how to create sub-device channels, see the corresponding documentation about sub-device channels.
-   Products and devices created before September 3, 2018, Beijing time, can be added to gateways as sub-devices. However, you can only add their topological relationships,and not channel configurations.

1.  On the Devices page, find the gateway device for which you want to add sub-devices and click **View** next to it. 
2.  Click **Sub-device Management** \> **Add Sub-device**. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18811/153820538810897_en-US.png) 
3.  Enter information of the sub-device in the dialog box. 

    |Parameter|Description|
    |---------|-----------|
    |Product|Select the name of the product that the sub-device belongs to.|
    |Device|Select the name of the device that you want to add as a sub-device.|
    |If the gateway connection protocol of the sub-device is Modbus, set the following parameters.|
    |Associated Channel|Required. Select a channel that the sub-device uses from the created sub-device channels.|
    |Slave Station Number|Enter an integer in the range of 1-247.|
    |If the gateway connection protocol of the sub-device is OPC UA, set the following parameters.|
    |Associated Channel|Required. Select a channel that the sub-device uses from the created sub-device channels.|
    |Node Path|Enter a node path. For example, Objects/Device1. In this example, Objects is a fixed root node, and Device1 is the node name of the device node path. Use `/` to separate node names.|
    |If the gateway connection protocol of the sub-device is custom protocol, set the following parameters.|
    |Associated Channel|Optional. Select a channel that the sub-device uses from the created sub-device channels.|
    |Custom Configuration|If you have selected an associated channel, you must customize the configuration. Only JSON format is supported.|

4.  On the details page of the sub-device, you can view the gateway device information. Click **Edit** to modify the configuration information. 

-   You can refer to [Alink protocol](../../../../intl.en-US/Developer Guide (Devices)/Alink Protocol.md#) to develop your own devices and assign the configurations between the gateway device and the sub-device to the device client.

