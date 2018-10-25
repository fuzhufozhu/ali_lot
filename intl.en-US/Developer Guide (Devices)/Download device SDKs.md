# Download device SDKs {#concept_jlk_mjl_vdb .concept}

IoT Platform provides multiple device SDKs to help you develop your devices and quickly connect them to the Cloud. As an alternative to SDKs, you can also use [Alink Protocol](intl.en-US/Developer Guide (Devices)/Alink Protocol.md#) for development.

## Prerequisites {#section_j13_sxr_52b .section}

Before developing devices, finish all console configurations, and obtain necessary informations such as the device details and topic information. For more information, see the User Guide.

## Device SDKs {#section_e4x_yxr_52b .section}

Select a device SDK according to the protocol and the features that you require. We recommend that you use C SDK as it provides more features.

**Note:** If you have specific development requirements that cannot be met by the current SDKs, you can develop according to the [Alink protocol](intl.en-US/Developer Guide (Devices)/Alink Protocol.md#).

| |C SDK|Java SDK|Android SDK|iOS SDK|HTTP/2 SDK|General protocol|
|--|-----|--------|-----------|-------|----------|----------------|
|MQTT|√|√|√|√| | |
|CoAP|√| | | | | |
|HTTP/HTTPS|√| | | | | |
|HTTP/2| | | | |√| |
|Other protocols| | | | | |√|
|Device certification: unique-certificate-per-device authentication|√|√|√|√|√|√|
|Device certification: unique-certificate-per-product authentication|√| |√| | | |
|OTA development|√| | | | | |
|Connecting sub-devices to IoT Platform|√| | | | | |
|Device shadow|√|√|√| | | |
|Device development based on TSL|√| |√| | | |
|Remote configuration|√| | | | | |

## Supported platforms {#section_rs1_m1s_52b .section}

Click [here](https://certification.aliyun.com/open/#/certificationlist) to view and query the platforms supported by Alibaba Cloud IoT Platform.

If the platform you want to use is not supported by IoT Platform, please open an issue on the [Github](https://github.com/aliyun/iotkit-embedded/issues) page.

## Download SDKs {#section_t5j_y1s_52b .section}

-   C SDK

    |Version number|Release date|Development environment|Download link|Updates|
    |--------------|------------|-----------------------|-------------|-------|
    |V2.2.1|2018/09/03|GNU make on 64-bit Linux|[RELEASED\_V2.2.1](https://linkkit-sdk-download.oss-cn-shanghai.aliyuncs.com/linkkit2.2.1.tar.gz)|     -   Added supports for connecting devices to WiFi and using open-source applications to locally control devices.
    -   Added supports for countdown routine before devices go offline.
    -   Added supports for OTA using iTls to download firmware files.
 |
    |V2.1.0|2018/03/31|GNU make on 64-bit Linux|[RELEASED\_V2\_10\_20180331.zip](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iot-sdk-c/RELEASED_V2_10_20180331.7z)|     -   Added support for CMake: You can use QT or VS2017 on Linux or Windows to open a project and compile software in CMake compiling method.
    -   Added support for TSL model definition on IoT Platform: You can set `FEATURE_CMP_ENABLED = y` and`FEATURE_DM_ENABLED = y` to define TSL models to provide API operations for properties, events, and services.
    -   Added support for unique-certificate-per-product: You can set `FEATURE_SUPPORT_PRODUCT_SECRET = y` to enable unique-certificate-per-product authentication and streamline the production queuing process.
    -   Added support for iTLS: You can set `FEATURE_MQTT_DIRECT_NOTLS = y` and `FEATURE_MQTT_DIRECT_NOITLS = n` to enable ID² encryption. You can use iTLS to establish data connections to enhance security and reduce memory consumption.
    -   Added support for remote configuration: You can set `FEATURE_SERVICE_OTA_ENABLED = y` and `FEATURE_SERVICE_COTA_ENABLED = y` to enable the cloud to push configuration information to devices.
    -   Optimized sub-device management of gateways: Added some features.
 |

-   Java SDK

    |Supported protocol|Update history|Download link|
    |------------------|--------------|-------------|
    |MQTT|2017-05-27: Added support for device authentication in the China \(Shanghai\) region. Added the device shadow demo on the Java client.|[iotx-sdk-mqtt-java](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iotx-sdk-java/iotx-sdk-mqtt-java-20170526.zip?spm=a2c4g.11186623.2.20.VMVBFk&file=iotx-sdk-mqtt-java-20170526.zip): The Java version that supports MQTT is only a demo of open-source library implementation. It is used only for reference.|

    Instructions: See [Java SDK](intl.en-US/Developer Guide (Devices)/Java SDK.md#).

-   iOS SDK

    Download link:

    -   [https://github.com/CocoaPods/Specs.git](https://github.com/CocoaPods/Specs.git)
    -   [https://github.com/aliyun/aliyun-specs.git](https://github.com/aliyun/aliyun-specs.git)
    Instructions:[iOS SDK](intl.en-US/Developer Guide (Devices)/iOS SDK.md#)

-   HTTP/2 SDK

    Download link: [iot-http2-sdk-demo](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/java-http2-sdk-demo/iot-http2-sdk-demos.zip).

-   General protocol

    Instructions: See [General protocol](intl.en-US/Developer Guide (Devices)/General protocols/Overview.md#).

-   Other open-source libraries

    Download link: [https://github.com/mqtt/mqtt.github.io/wiki/libraries](https://github.com/mqtt/mqtt.github.io/wiki/libraries?spm=a2c4g.11186623.2.22.VMVBFk)


