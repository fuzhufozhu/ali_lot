# 下载设备端SDK {#concept_jlk_mjl_vdb .concept}

物联网平台提供各类设备端SDK，简化开发过程，使设备快速上云。若您不使用SDK，请参考[Alink协议](cn.zh-CN/设备端开发指南/基于Alink协议开发/Alink协议.md#)文档自行开发。

## 前提条件 {#section_j13_sxr_52b .section}

在设备端开发之前，您需要首先完成控制台所需操作，以获取设备开发阶段的必要信息，包括设备信息、Topic信息等。具体内容请参考用户指南。

## 设备端SDK {#section_e4x_yxr_52b .section}

您可以根据实际环境的协议、功能需求，选择合适的设备端SDK进行开发。推荐使用实现了更多逻辑的C SDK。

**说明：** 如果提供的SDK不能满足您的需求，您也可以基于[Alink协议](cn.zh-CN/设备端开发指南/基于Alink协议开发/Alink协议.md#)自行开发。

| |C SDK|Java SDK|Android SDK|iOS SDK|Node.js SDK|HTTP/2 SDK|泛化协议|
|--|-----|--------|-----------|-------|-----------|----------|----|
|MQTT|√|√|√|√|√| | |
|CoAP|√| | | | | | |
|HTTP/HTTPS|√| | | | | | |
|HTTP/2| | | | | |√| |
|其他协议| | | | | | |√|
|设备认证：一机一密|√|√|√|√|√|√|√|
|设备认证：一型一密|√|√|√| | | | |
|OTA开发|√| |√| | | | |
|子设备接入|√| |√| |√| | |
|设备影子|√|√|√| | | | |
|基于物模型的设备开发|√|√|√| |√| | |
|远程配置|√| |√| | | | |

## 适配平台 {#section_rs1_m1s_52b .section}

您可以单击[此处](https://certification.aliyun.com/open/#/certificationlist)，查询阿里云物联网平台适配的平台及详细信息。

如果您使用的平台未被适配，请访问[官方Github](https://github.com/aliyun/iotkit-embedded/issues)，给我们提出建议。

## 下载SDK {#section_t5j_y1s_52b .section}

-   [C SDK](https://help.aliyun.com/document_detail/96623.html)

-   [Java SDK](https://help.aliyun.com/document_detail/97331.html)

-   [Android SDK](https://help.aliyun.com/document_detail/96607.html)

-   [Node.js SDK](https://help.aliyun.com/document_detail/96618.html)

-   iOS SDK

    下载地址：

    -   [https://github.com/CocoaPods/Specs.git](https://github.com/CocoaPods/Specs.git)
    -   [https://github.com/aliyun/aliyun-specs.git](https://github.com/aliyun/aliyun-specs.git)
    使用说明：[iOS-SDK](cn.zh-CN/设备端开发指南/iOS-SDK.md#)

-   HTTP/2 SDK

    下载地址：[iot-http2-sdk-demo](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/java-http2-sdk-demo/iot-http2-sdk-demos.zip)。

    使用说明：[HTTP/2 SDK](cn.zh-CN/设备端开发指南/HTTP__2 SDK.md#)

-   [泛化协议](cn.zh-CN/用户指南/泛化协议/概览.md#)
-   [其他开源库参考](https://github.com/mqtt/mqtt.github.io/wiki/libraries?spm=a2c4g.11186623.2.22.VMVBFk)

