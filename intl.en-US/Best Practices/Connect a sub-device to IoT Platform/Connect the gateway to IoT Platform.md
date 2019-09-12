# Connect the gateway to IoT Platform {#task_1545804 .task}

This topic describes how to configure a device SDK to connect a gateway to IoT platform.

[Create a gateway and a sub-device](intl.en-US/Best Practices/Connect a sub-device to IoT Platform/Create a gateway and a sub-device.md#)

[Initialize the SDKs](intl.en-US/Best Practices/Connect a sub-device to IoT Platform/Initialize the SDKs.md#)

## Configure the device SDK for a gateway {#section_rs9_bfg_i70 .section}

In this example,the DeviceTopoManager file in the directoryjava/src/main/com.aliyun.iot.api.common.openApi contains the code for connecting the gateway to IoT Platform.

-   Configure the gateway information for connection.

    ``` {#codeblock_7ro_9ua_3rx}
    
        private static String regionId = "cn-shanghai";
        private static final String TAG = "TOPO";
    
        //The gateway.
        private static String GWproductKey = "a1BxptK***";
        private static String GWdeviceName = "XMtrv3yvftEHAzrTfX1U";
        private static String GWdeviceSecret = "19xJNybifnmgcK057vYhazYK4b64****";
    
    
        public static void main(String[] args) {
            /**
             * The information about the MQTT connection.
             */
            DeviceTopoManager manager = new DeviceTopoManager();
    
            /**
             * The Java HTTP client of this device supports TSLv1.2.
             */
            System.setProperty("https.protocols", "TLSv2");
    
            manager.init();
        }
    ```

-   Establish a connection.

    ``` {#codeblock_n8f_z6l_6xz}
    
    public void init() {
            LinkKitInitParams params = new LinkKitInitParams();
            /**
             * Specifies the parameters for the MQTT initialization.
             */
            IoTMqttClientConfig config = new IoTMqttClientConfig();
            config.productKey = GWproductKey;
            config.deviceName = GWdeviceName;
            config.deviceSecret = GWdeviceSecret;
            config.channelHost = GWproductKey + ".iot-as-mqtt." + regionId + ".aliyuncs.com:1883";
            /**
             * Specifies whether to receive offline messages.
             * The cleanSession field corresponding to the MQTT connection.
             */
            config.receiveOfflineMsg = false;
            params.mqttClientConfig = config;
            ALog.setLevel(LEVEL_DEBUG);
            ALog.i(TAG, "mqtt connetcion info=" + params);
    
            /**
             * Specifies the initialization and passes in the certificate information of the gateway.
             */
            DeviceInfo deviceInfo = new DeviceInfo();
            deviceInfo.productKey = GWproductKey;
            deviceInfo.deviceName = GWdeviceName;
            deviceInfo.deviceSecret = GWdeviceSecret;
            params.deviceInfo = deviceInfo;
    
            /** Establishes a connection. **/
            LinkKit.getInstance().init(params, new ILinkKitConnectListener() {
                public void onError(AError aError) {
                    ALog.e(TAG, "Init Error error=" + aError);
                }
    
                public void onInitDone(InitResult initResult) {
                    ALog.i(TAG, "onInitDone result=" + initResult);
    
    
                    //Queries the topological relationships of this gateway, and checks whether a topological relationship exists between the gateway and the sub-device.
                    //If the topological relationship exists, reports the sub-device information to IoT Platform.
                    getGWDeviceTopo();
    
                    //Dynamically registers the sub-device to obtain the DeviceSecret of the sub-device.If the gateway has obtained the certificate of the sub-device, skips this step.
                    //When you create the sub-device in IoT Platform, you can use  the serial number or MAC address of the physical device as the DeviceName of the sub-device.
                    gatewaySubDevicRegister();
    
                    //The certificate information of the sub-device.
                    gatewayAddSubDevice();
    
                    //Connects the sub-device to IoT Platform through the gateway.
                    gatewaySubDeviceLogin();
    
                }
            });
        }
    ```


## Test the connection {#section_4ci_il1_1m0 .section}

After you configure the gateway in the device SDK, you can test the connection between the device SDK and IoT platform.

1.  Run DeviceTopoManager.
2.  Go to the [IoT Platform console](https://iot.console.aliyun.com/), and in the left-side navigation pane, choose **Devices**.
3.  In the **Device List** section, find the gateway and check the state of the device. If status of the gateway device is **Online**, the gateway is connected to IoT platform. 

    ![Connect a device to IoT Platform](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1040928/156828548452528_en-US.png)


[Connect the sub-device to IoT Platform](intl.en-US/Best Practices/Connect a sub-device to IoT Platform/Connect the sub-device to IoT Platform.md#)

