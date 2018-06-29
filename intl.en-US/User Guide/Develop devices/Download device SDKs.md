# Download device SDKs {#concept_jlk_mjl_vdb .concept}

This topic describes where to download the different versions of the SDK files.

-   C SDK, Java SDK, iOS SDK, and Android SDK support the MQTT protocol.
-   Only the C SDK supports HTTPS and CoAP.

You can also use the open-source MQTT, CoAP, and HTTP clients to develop new SDKs to connect to IoT Platform.

The official Alibaba Cloud C SDK provides more features, such as authentication, OTA configuration, device shadow management, and the sub-device management of the gateway. If you use the open-source version, follow the existing standards to apply more features.

## C SDKs {#section_bqx_lqm_vdb .section}

The code of the device SDK has been hosted on GitHub. The download links are as follows:

-   [https://github.com/aliyun/iotkit-embedded](https://github.com/aliyun/iotkit-embedded)
-   [https://github.com/aliyun/iotkit-embedded/wiki](https://github.com/aliyun/iotkit-embedded/wiki)

Supported platforms are described in [Table 1](#table_cth_frm_vdb). If your platform is not supported, go to [Official Github](https://github.com/aliyun/iotkit-embedded/issues) to download the C SDK. If you want IoT Platform to support your platform, tell us in feedback.

|Development board|Network support|Link of the vendor SDK|Purchase link of the development board|Alibaba Cloud  SDK version|
|-----------------|---------------|----------------------|--------------------------------------|--------------------------|
|ESP32|Wi-Fi|[esp32-aliyun](https://github.com/espressif/esp32-aliyun)|[Espressif Systems \(Shanghai\) Pte., Ltd.](https://espressif.taobao.com/)|V2.01|
|ESP8266|Wi-Fi|[esp8266-aliyun](https://github.com/espressif/esp8266-aliyun)|[Espressif Systems \(Shanghai\) Pte., Ltd.](https://espressif.taobao.com/)|V2.01|

[Table 2](#table_ewb_vsm_vdb) shows the supported SDK versions.

|Version number|Release date|Development language|Development environment|Download link|Updates|
|--------------|------------|--------------------|-----------------------|-------------|-------|
|V2.10|2018/03/31|C language|GNU make on 64-bit Linux|[RELEASED\_V2\_10\_20180331.zip](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iot-sdk-c/RELEASED_V2_10_20180331.7z)| -   Supports CMake. You can use QT or VS2017 on Linux or Windows to open a project and compile and run the project.
-   Supports TSL model definition on IoT Platform: By setting `FEATURE_CMP_ENABLED = y` and `FEATURE_DM_ENABLED = y`, you can define TSL models to provide API operations for properties, events, and services.
-   Supports unique certificate per product: You can set `FEATURE_SUPPORT_PRODUCT_SECRET = y` to enable unique-certificate-per-product authentication and streamline the production queuing process.
-   Supports iTLS: You can set `FEATURE_MQTT_DIRECT_NOTLS = y` and `FEATURE_MQTT_DIRECT_NOITLS = n` to enable ID² encryption. You can use iTLS to establish data connections to enhance security and reduce memory consumption.
-   Supports remote configuration: You can set `FEATURE_SERVICE_OTA_ENABLED = y` and `FEATURE_SERVICE_COTA_ENABLED = y` to enable the cloud to push configuration information to the device.
-   Optimizes sub-device management of the gateway: Added some features.

 |
|V2.03|2018/01/31|C language|GNU make on 64-bit Linux|[RELEASED\_V2\_03\_20180131.zip](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iot-sdk-c/RELEASED_V2_03_20180130.7z?spm=a2c4g.11186623.2.12.VMVBFk&file=RELEASED_V2_03_20180130.7z)| -   Supports sub-device management of the gateway: You can set `FEATURE_SUBDEVICE_ENABLED = y` to allow sub-devices to exchange data with the cloud through the gateway.
-   Updated HTTP connections: Optimized the HTTP connection establishment process.
-   Optimized TLS: Fixed memory leakage issues.
-   Optimized OTA configuration: You can enable or disable OTA more effectively.
-   Updated MQTT connections: Supports longer topics and more subscription requests. MQTT supports multiple threads.

 |
|V2.02|2017/11/30|C language|GNU make on 64-bit Linux|[RELEASED\_V2\_02\_20171130.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_02_20171130.zip?spm=a2c4g.11186623.2.13.VMVBFk&file=RELEASED_V2_02_20171130.zip)| -   Officially supports multi-platform: You can use `make reconfig` to view and select a supported platform except `Ubuntu16.04`.
-   Added Windows versions: You can use mingw32 to compile the database and examples of `Win7`.
-   Supports OpenSSL: Added a HAL reference implementation to `openssl-0.9.x`+`Windows`.
-   Optimized the HTTP port: The HTTP port can send packets without terminating the TLS connection.
-   Included a secure library: Added a tailored version of the `mbedtls` secure library. This secure library can be supported in Linux and Windows platforms.

 |
|V2.01|2017/10/10|C language|GNU make on 64-bit Linux|[RELEASED\_V2\_01\_20171010.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_01_20171010.zip?spm=a2c4g.11186623.2.14.VMVBFk&file=RELEASED_V2_01_20171010.zip)| -   Supports CoAP+OTA: Supported CoAP-based OTA.
-   Supports HTTP+TLS: Supports HTTP connections in addition to MQTT and CoAP connecitons.
-   Refined OTA status: Optimized part of the OTA code. The optimization allows the cloud to differentiate the OTA firmware download status of the device.
-   Supports ArmCC: Fixed the errors that may occur when the SDK is complied in an ArmCC compiler.

 |
|V2.00|2017/08/21|C language|GNU make on 64-bit Linux|[RELEASED\_V2\_00\_20170818.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_00_20170818.zip?spm=a2c4g.11186623.2.15.VMVBFk&file=RELEASED_V2_00_20170818.zip)| -   Supports MQTT connections: Instead of using HTTPS or HTTP, you can establish faster and more light-weight MQTT connections. See [Release notes](https://help.aliyun.com/document_detail/57164.html?spm=a2c4g.11186623.2.16.VMVBFk).
-   Supports CoAP connections: Based on UDP, CoAP connections consume less memory for pure data transfer. See [Release notes](https://help.aliyun.com/document_detail/57566.html?spm=a2c4g.11186623.2.17.VMVBFk).
-   Supports OTA channels: Provided OTA-related API operations to query, trigger, or download firmware that was uploaded by the users.
-   Updated the build system: You can organize and configure the SDK more efficiently.

 |
|V1.0.1|2017/06/29|C language|GNU make on 64-bit Linux|[RELEASED\_V1\_0\_1\_20170629.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V1_0_1_20170629.zip?spm=a2c4g.11186623.2.18.VMVBFk&file=RELEASED_V1_0_1_20170629.zip)| -   **China \(Shanghai\)**: The first device SDK officially released for China \(Shanghai\) with full source code.
-   **Added device shadow**: See [Description of device shadows](https://help.aliyun.com/document_detail/53930.html?spm=a2c4g.11186623.2.19.VMVBFk).

 |

## Java version {#section_dfl_dxm_vdb .section}

|Supported protocol|Update history|Download link|
|------------------|--------------|-------------|
|MQTT|2017-05-27: Supports device authentication in the China \(Shanghai\) region, and added the device shadow demo on the Java client.|[iotx-sdk-mqtt-java:](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iotx-sdk-java/iotx-sdk-mqtt-java-20170526.zip?spm=a2c4g.11186623.2.20.VMVBFk&file=iotx-sdk-mqtt-java-20170526.zip) The Java version that supports MQTT is only a demo of open-source library implementation. It is used only for reference.|

## Android version {#section_crn_dym_vdb .section}

Download link: [https://github.com/eclipse/paho.mqtt.android](https://github.com/eclipse/paho.mqtt.android?spm=a2c4g.11186623.2.21.VMVBFk&file=paho.mqtt.android)

## iOS version {#section_dzm_xtn_12b .section}

Download links:

-   [https://github.com/CocoaPods/Specs.git](https://github.com/CocoaPods/Specs.git)
-   [https://github.com/aliyun/aliyun-specs.git](https://github.com/aliyun/aliyun-specs.git)

## Other open-source libraries {#section_klv_2ym_vdb .section}

Download link: [https://github.com/mqtt/mqtt.github.io/wiki/libraries](https://github.com/mqtt/mqtt.github.io/wiki/libraries?spm=a2c4g.11186623.2.22.VMVBFk)

