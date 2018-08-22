# Connect a device to IoT Platform {#task_rmv_ktf_vdb .task}

This topic describes how to connect a device to IoT Platform.

1.   Run the following commands to compile the SDK to generate a sample program. 

    `cd iotkit-embedded`

    `make distclean`

    `make`

    A sample file is generated and saved to the output/release/bin/mqtt-example directoty.

2.   Run the following command to connect the device to the platform. 

    `./output/release/bin/mqtt-example`

    The system displays the following messages, indicating that the device has successfully connected to the platform.

    ```
    [dbg] guider_print_dev_guider_info(248): ....................................................
    [dbg] guider_print_dev_guider_info(249): ProductKey : ***********
    [dbg] guider_print_dev_guider_info(250): DeviceName : SK-1
    [dbg] guider_print_dev_guider_info(251): DeviceID : ***********.
    device-test[dbg] guider_print_dev_guider_info(253): ....................................................
    [dbg] guider_print_dev_guider_info(254): PartnerID Buf : ,partner_id=example.demo.partner-id
    [dbg] guider_print_dev_guider_info(255): ModuleID Buf : ,module_id=example.demo.module-id
    [dbg] guider_print_dev_guider_info(256): Guider URL : 
    [dbg] guider_print_dev_guider_info(258): Guider SecMode : 2 
    [dbg] guider_print_dev_guider_info(260): Guider Timestamp : 2524608000000
    [dbg] guider_print_dev_guider_info(261): ....................................................
    [inf] iotx_mc_connect(2035): **mqtt connect success!**
    ```

3.   Log on to the IoT Platform console, select Devices, select Smart Irrigation from the product list, select SK-1 from the device list, and then click **View**. Verify that the device is in Online status, as shown in [Figure 1](#fig_hnk_bz3_vdb). 

    ![](images/2125_en-US.png "Online status")


