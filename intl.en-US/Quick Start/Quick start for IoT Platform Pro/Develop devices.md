# Develop devices {#task_c5t_ktf_vdb .task}

Alibaba Cloud IoT Platform provides device SDKs to allow devices to communicate with the platform. This topic describes how to download the SDKs.

This example uses the C SDK for Linux . We recommend that you use Ubuntu16.04 to compile the SDK.

This task shows how to download an SDK for Smart Irrigation.

To develop a device, follow these steps:

1.  Download the SDK.
2.  Set the three key fields \(ProductKey, DeviceName, and DeviceSecret\) for authentication.
3.  Configure a TSL model.

1.   Log on to a Linux VM instance. 
2.   Run the following command to install the software that is required for the SDK development and compilation environment. 

    `apt-get install -y build-essential make git gcc`

    The software includes make-4.1, git-2.7.4, gcc-5.4.0, gcov-5.4.0, lcov-1.12, bash-4.3.48, tar-1.28, and mingw-5.3.1.

3.   In the root directory, run the following command to rehost the source code of the SDK to GitHub. 

    `git clone https://github.com/aliyun/iotkit-embedded`

4.   Run the following commands to view all available SDK versions. 

    `cd iotkit-embedded/`

    `git branch -r`

    The system displays the following versions:

    ```
    
    origin/HEAD -> origin/master
    origin/RELEASED_V1_0_1_20170629
    origin/RELEASED_V2_00_20170818
    origin/RELEASED_V2_01_20171010
    origin/RELEASED_V2_02_20171130
    origin/RELEASED_V2_03_20180131
    **origin/RELEASED\_V2\_10\_20180331**
    origin/master
    
    
    ```

5.   Run the following command to update the current SDK to the latest version. SDK files are named by release time. In this example,  the latest version is origin/RELEASED\_V2\_10\_20180331. 

    `git checkout SDK version`

6.   Run the following command to modify the make.setting file to configure the TSL model of the device, which is supported by IoT Platform Pro. 

    Set both FEATURE\_CMP\_ENABLEDand FEATURE\_DM\_ENABLED to y.

    `vi iotkit-embedded/make.settings`

    The system displays the following features:

    ```
    
    FEATURE_MQTT_COMM_ENABLED = y
    FEATURE_MQTT_DIRECT = y
    FEATURE_MQTT_DIRECT_NOTLS = n
    FEATURE_MQTT_DIRECT_NOITLS = y
    FEATURE_COAP_COMM_ENABLED = n
    FEATURE_HTTP_COMM_ENABLED = y
    FEATURE_SUBDEVICE_ENABLED = n
    **FEATURE\_CMP\_ENABLED = y**
    **FEATURE\_DM\_ENABLED = y**
    FEATURE_SERVICE_OTA_ENABLED = y
    
    
    ```

7.   Run the following command to modify the ProductKey, DeviceName, and DeviceSecret in the iotkit-embedded/sample/linkkit/samples/linkkit\_sample.c file. 

    `vi iotkit-embedded/sample/linkkit/samples/linkkit_sample.c`

    Modify the ProductKey, DeviceName, and DeviceSecret of Smart Irrigation saved in [5](intl.en-US/Quick Start/Quick start for IoT Platform Pro/Add devices.md#step5) as follows:

    ```
    
    #define DM_PRODUCT_KEY_1 "a1a3Xryxx97"
    #define DM_DEVICE_NAME_1 "SK-1"
    #define DM_DEVICE_SECRET_1 "Dud7czC0DZgIo9pUlGgZ9zg9raoruMTC"
    
    
    ```

8.   Modify the TSL model in the iotkit-embedded/sample/linkkit/samples/linkkit\_sample.c file. 

    You can use the following methods to obtain the TSL model: 

    -   During the operation, the device publishes a message to a topic to run the `/sys/{productKey}/{deviceName}/thing/dsltemplate/get` command to request the TSL model from IoT Platform.

        This method is memory-intensive and will produce a certain amount of data traffic. A TSL model consumes approximately 20 KB  of memory and produces approximately 10 KB of network traffic. The actual amounts of memory and network traffic vary by the complexity of a TSL model.

    -   Manually export the TSL model from IoT Platform, and save the TSL model to the linkkit\_sample.c file to preprovision a TSL model for the device. Make sure that the TSL model has been defined before you develop your device. Once the development starts, do not edit the TSL model. Otherwise, you must synchronize every modification to the preprovisioned TSL model on the device.
    This example uses the TSL model downloaded from [8](intl.en-US/Quick Start/Quick start for IoT Platform Pro/Create a product/Define Thing Specification Language models.md#step8).

    Modify the following information:

    ```
    
    #if !( WIN32)
    **const char TSL\_STRING\[\] =** "{\"schema\":\"https://iot-tsl.oss-cn-shanghai.aliyuncs.com/schema.json\",\"profile\":{\"productKey\":\"a1a3Xryxx97\"},\"services\":[{\"outputData\":[],\"identifier\":\"set\",\"inputData\":[{\"identifier\":\"PowerSwitch\",\"dataType\":{\"specs\":{\"0\":\"Off\",\"1\":\"On\"},\"type\":\"bool\"},\"name\":\"Power Switch\"}],\"method\":\"thing.service.property.set\",\"name\":\"set\",\"required\":true,\"callType\":\"sync\",\"desc\":\"Set Properties\"},{\"outputData\":[{\"identifier\":\"PowerSwitch\",\"dataType\":{\"specs\":{\"0\":\"Off\",\"1\":\"On\"},\"type\":\"bool\"},\"name\":\"Power Switch\"}],\"identifier\":\"get\",\"inputData\":[\"PowerSwitch\"],\"method\":\"thing.service.property.get\",\"name\":\"get\",\"required\":true,\"callType\":\"sync\",\"desc\":\"Get Properties\"},{\"outputData\":[],\"identifier\":\"AutoSprinkle\",\"inputData\":[{\"identifier\":\"SprinkleTime\",\"dataType\":{\"specs\":{\"unit\":\"min\",\"min\":\"0\",\"unitName\":\"minute\",\"max\":\"60\"},\"type\":\"int\"},\"name\":\"Sprinkling Interval\"},{\"identifier\":\"SprinkleVolume\",\"dataType\":{\"specs\":{\"unit\":\"mL\",\"min\":\"0\",\"unitName\":\"milliliter\",\"max\":\"1000\"},\"type\":\"int\"},\"name\":\"Sprinkling Amount\"}],\"method\":\"thing.service.AutoSprinkle\",\"name\":\"Automatic Sprinkler\",\"required\":false,\"callType\":\"async\"}],\"properties\":[{\"identifier\":\"PowerSwitch\",\"dataType\":{\"specs\":{\"0\":\"Off\",\"1\":\"On\"},\"type\":\"bool\"},\"name\":\"Power Switch\",\"accessMode\":\"rw\",\"required\":false}],\"events\":[{\"outputData\":[{\"identifier\":\"PowerSwitch\",\"dataType\":{\"specs\":{\"0\":\"Off
    \",\"1\":\"On\"},\"type\":\"bool\"},\"name\":\"Power Switch\"}],\"identifier\":\"post\",\"method\":\"thing.event.property.post\",\"name\":\"post\",\"type\":\"info\",\"required\":true,\"desc\":\"Report Properties\"},{\"outputData\":[{\"identifier\":\"ErrorCode\",\"dataType\":{\"specs\":{\"0\":\"Voltage Anomaly\",\"1\":\"Overcurrent\",\"2\":\"Network Fault\"},\"type\":\"enum\"},\"name\":\"Fault Code\"}],\"identifier\":\"ErrorCode\",\"method\":\"thing.event.ErrorCode.post\",\"name\":\"Fault Report\",\"type\":\"error\",\"required\":false}]}";
    
    
    ```

    **Note:** The downloaded TSL model of Smart Irrigation is originally in .json format. When you change the format to the C language, be careful with the format and escape characters.


