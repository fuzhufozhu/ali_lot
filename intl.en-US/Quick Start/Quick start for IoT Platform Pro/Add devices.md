# Add devices {#task_e2t_ktf_vdb .task}

This topic describes how to add devices to a product after the product has been added.

This example shows how to add a device named SK-1 to Smart Irrigation.

1.   From the left-side navigation pane in the IoT Platform console, select **Devices**. 

    The Devices page appears, as shown in [Figure 1](#fig_sqb_4xh_vdb).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12788/2121_en-US.png "Devices")

2.   From the dropdown list of the product in the upper-left side of the page , select Smart Irrigation, and click **Add Device**. The Add Device page appears, as shown in [Figure 2](#fig_i3h_3yh_vdb).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12788/2122_en-US.png "Add Device")

3.   Set the device name to SK-1. 

    **Note:** A device name uniquely identifies a device under a product and can be used to communicate with IoT Hub. 

4.   Click **OK**. 

    The Added page appears, as shown in [Figure 3](#fig_snz_td3_vdb).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12788/2123_en-US.png "Added")

5.   Click **Copy** to save the three key fields \(ProductKey, DeviceName, and DeviceSecret\) of the device. 

    The three key fields will be added to the device SDK and used for authentication when the device connects to IoT Platform.

    ```
    "product_key":"a1ekkixgSv1",
    "device_name":"SK-1",
    "device_secret":"Mqfov2L55E3IwTvy49ikhqs69FfQkScF",
    ```

    -   product\_key: Identifies a product category.
    -   device\_name: Identifies a device in a category.
    -   device\_secret: Device key.
6.   Click **Close**. 

