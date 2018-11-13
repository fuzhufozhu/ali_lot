# 下载设备端SDK {#concept_jlk_mjl_vdb .concept}

物联网平台提供各类设备端SDK，简化开发过程，使设备快速上云。若您不使用SDK，请参考[Alink协议](intl.zh-CN/设备端开发指南/不使用SDK开发/基于Alink协议开发/Alink协议.md#)文档自行开发。

## 前提条件 {#section_j13_sxr_52b .section}

在设备端开发之前，您需要首先完成控制台所需操作，以获取设备开发阶段的必要信息，包括设备信息、Topic信息等。具体内容请参考用户指南。

## 设备端SDK {#section_e4x_yxr_52b .section}

您可以根据实际环境的协议、功能需求，选择合适的设备端SDK进行开发。推荐使用实现了更多逻辑的C SDK。

**说明：** 如果提供的SDK不能满足您的需求，您也可以基于[Alink协议](intl.zh-CN/设备端开发指南/不使用SDK开发/基于Alink协议开发/Alink协议.md#)自行开发。

| |C SDK|Java SDK|Android SDK|iOS SDK|HTTP/2 SDK|泛化协议|
|--|-----|--------|-----------|-------|----------|----|
|MQTT|√|√|√|√| | |
|CoAP|√| | | | | |
|HTTP/HTTPS|√| | | | | |
|HTTP/2| | | | |√| |
|其他协议| | | | | |√|
|设备认证：一机一密|√|√|√|√|√|√|
|设备认证：一型一密|√| |√| | | |
|OTA开发|√| | | | | |
|子设备接入|√| |√| | | |
|设备影子|√|√|√| | | |
|基于物模型的设备开发|√| |√| | | |
|远程配置|√| |√| | | |

## 适配平台 {#section_rs1_m1s_52b .section}

您可以单击[此处](https://certification.aliyun.com/open/#/certificationlist)，查询阿里云物联网平台适配的平台及详细信息。

如果您使用的平台未被适配，请访问[官方Github](https://github.com/aliyun/iotkit-embedded/issues)，给我们提出建议。

## 下载SDK {#section_t5j_y1s_52b .section}

-   C SDK

    |版本号|发布日期|开发环境|下载链接|更新内容|
    |---|----|----|----|----|
    |V2.2.1|2018/09/03|64位Linux, GNU Make|[RELEASED\_V2.2.1](https://linkkit-sdk-download.oss-cn-shanghai.aliyuncs.com/linkkit2.2.1.tar.gz)|     -   WiFi配网、设备本地控制开源
    -   支持离线倒计时例程
    -   OTA支持使用iTls下载固件
 |
    |V2.1.0|2018/03/31|64位Linux, GNU Make|[RELEASED\_V2\_10\_20180331.zip](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iot-sdk-c/RELEASED_V2_10_20180331.7z)|     -   支持cmake: 支持cmake编译方式，可以直接在linux和windows下使用QT或者VS2017打开工程进行编译运行。
    -   支持云端对物模型的抽象 : 设置`FEATURE_CMP_ENABLED = y`和`FEATURE_DM_ENABLED = y`, 可以支持物模型抽象，提供属性，服务和事件的接口。
    -   支持一型一密: 设置`FEATURE_SUPPORT_PRODUCT_SECRET = y`可以支持一型一密功能，优化产线流程。
    -   支持iTLS功能: 设置`FEATURE_MQTT_DIRECT_NOTLS = y`和`FEATURE_MQTT_DIRECT_NOITLS = n`可以支持ID2加密方式，使用iTLS进行数据建连，增加安全性，降低内存消耗。
    -   支持远程配置: 设置`FEATURE_SERVICE_OTA_ENABLED = y`和`FEATURE_SERVICE_COTA_ENABLED = y`，可以支持云端推送配置信息到设备。
    -   优化主子设备功能：主子设备添加部分功能。
 |

-   Java SDK

    |支持协议|更新历史|下载链接|
    |----|----|----|
    |MQTT|2017-05-27：支持华东2节点的设备认证流程，同时添加java端设备影子demo。|[iotx-sdk-mqtt-java](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iotx-sdk-java/iotx-sdk-mqtt-java-20170526.zip?spm=a2c4g.11186623.2.20.VMVBFk&file=iotx-sdk-mqtt-java-20170526.zip) ：MQTT的JAVA版只是使用开源库实现的一个demo，仅用于参考。|

    使用说明：[JAVA-SDK](intl.zh-CN/设备端开发指南/JAVA-SDK.md#)。

-   Android SDK

    使用说明：[Android-SDK](intl.zh-CN/设备端开发指南/Android-SDK/工程配置.md#)

-   iOS SDK

    下载地址：

    -   [https://github.com/CocoaPods/Specs.git](https://github.com/CocoaPods/Specs.git)
    -   [https://github.com/aliyun/aliyun-specs.git](https://github.com/aliyun/aliyun-specs.git)
    使用说明：[iOS-SDK](intl.zh-CN/设备端开发指南/iOS-SDK.md#)

-   HTTP/2 SDK

    下载地址：[iot-http2-sdk-demo](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/java-http2-sdk-demo/iot-http2-sdk-demos.zip)。

    使用说明：[HTTP/2 SDK](intl.zh-CN/设备端开发指南/HTTP__2 SDK.md#)

-   泛化协议

    使用说明：[泛化协议](intl.zh-CN/设备端开发指南/泛化协议/概览.md#)

-   其他开源库参考

    下载地址为：[https://github.com/mqtt/mqtt.github.io/wiki/libraries](https://github.com/mqtt/mqtt.github.io/wiki/libraries?spm=a2c4g.11186623.2.22.VMVBFk)


