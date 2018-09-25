# Sub-device channels {#concept_vlx_vvn_y2b .concept}

You can create sub-device channels for Pro Edition gateway devices. Gateway devices can then use the management channels to manage sub-devices. Currently, IoT Platform supports three kinds of channels: Modbus protocol channels, OPC UA protocol channels, and custom protocol channels.

1.  On the Devices page, find the gateway device for which you want to create channels, and click **View** next to it.
2.  Click **Sub-device Channels** and then create sub-device management channels according to your required protocol.

    ![](images/10890_en-US.png)

    -   Modbus

        In the Modbus tab, click **Create Modbus Channel** and enter the required information in the dialog box.

        |Parameter|Description|
        |---------|-----------|
        |Channel Name|The channel identifier. It must be unique under the gateway device.|
        |Transmission Mode|Supports RTU and TCP.|
        |If you select RTU as the transmission mode, you must set the following parameters:|
        |Select Serial Port|For example, /dev/tty0 or /dev/tty1.|
        |Baud Rate|Select a value from the drop-down list.|
        |Data Bit|Supports the following data bit values: 5, 6, 7, and 8.|
        |Check Bit|Supports no parity check, odd parity check, and even parity check.|
        |Stop Bit|Support the following stop bit values: 1, 1.5, and 2.|
        |If you select the transmission mode as TCP, you must set the following parameters:|
        |IP address|Enter an IP address in dot-decimal notation.|
        |Port Number|Enter an integer in the range of 0-65535.|

    -   OPC UA

        Click **OPC UA** \> **Create OCP UA Channel**, and enter the required information in the dialog box.

        |Parameter|Description|
        |---------|-----------|
        |Channel Name|The channel name must be unique under the gateway device.|
        |Connection Address|For example, opc.tcp://localhost:4840|
        |User Name|An optional parameter.|
        |Password|An optional parameter.|
        |Function Call Timeout|The unit is in seconds.|

    -   Custom

        1.  Click **Custom** \> **Create Customized Channel**.
        2.  Enter a channel name in the dialog box.
        3.  Enter your customized configuration content.

            **Note:** The configuration content must be in JSON format. We recommend that you prepare the JSON content in advance, and paste it in the box.


