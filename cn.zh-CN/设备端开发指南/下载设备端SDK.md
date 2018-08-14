# 下载设备端SDK {#concept_jlk_mjl_vdb .concept}

本章节介绍各种版本的SDK文件的下载路径。

-   C语言版、JAVA版、IOS版和Android版的SDK支持MQTT协议。
-   仅C语言版的SDK支持HTTPS和CoAP协议。

开发者也可以使用开源的MQTT、CoAP和HTTP客户端自行开发，实现和云端的连接。

其中，官方提供的C语言版实现了更多逻辑，比如认证逻辑、OTA、设备影子和网关主子设备能力等功能，如果您使用开源版本，扩展能力需要按照已有规范实现。

## C语言版 {#section_bqx_lqm_vdb .section}

设备端SDK代码托管迁移到Github，下载地址为

-   [https://github.com/aliyun/iotkit-embedded](https://github.com/aliyun/iotkit-embedded)
-   [https://github.com/aliyun/iotkit-embedded/wiki](https://github.com/aliyun/iotkit-embedded/wiki)

已适配平台，如[表 1](#table_cth_frm_vdb)所示，如果您使用的平台未被适配, 请访问[官方Github](https://github.com/aliyun/iotkit-embedded/issues)，给我们提出建议。

|开发板|网络支持|厂商SDK链接|开发板购买链接|阿里云SDK版本|
|---|----|-------|-------|--------|
|ESP32|Wi-Fi|[esp32-aliyun](https://github.com/espressif/esp32-aliyun)|[乐鑫信息科技](https://espressif.taobao.com/)|V2.01|
|ESP8266|Wi-Fi|[esp8266-aliyun](https://github.com/espressif/esp8266-aliyun)|[乐鑫信息科技](https://espressif.taobao.com/)|V2.01|

支持的设备端SDK版本如[表 2](#table_ewb_vsm_vdb)所示。

|版本号|发布日期|开发语言|开发环境|下载链接|更新内容|
|---|----|----|----|----|----|
|版本V2.10|2018/03/31|C语言|64位Linux, GNU Make|[RELEASED\_V2\_10\_20180331.zip](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iot-sdk-c/RELEASED_V2_10_20180331.7z)| -   支持cmake: 支持cmake编译方式，可以直接在linux和windows下使用QT或者VS2017打开工程进行编译运行。
-   支持云端对物模型的抽象 : 设置`FEATURE_CMP_ENABLED = y`和`FEATURE_DM_ENABLED = y`, 可以支持物模型抽象，提供属性，服务和事件的接口。
-   支持一型一密: 设置`FEATURE_SUPPORT_PRODUCT_SECRET = y`可以支持一型一密功能，优化产线流程。
-   支持iTLS功能: 设置`FEATURE_MQTT_DIRECT_NOTLS = y`和`FEATURE_MQTT_DIRECT_NOITLS = n`可以支持ID2加密方式，使用iTLS进行数据建连，增加安全性，降低内存消耗。
-   支持远程配置: 设置`FEATURE_SERVICE_OTA_ENABLED = y`和`FEATURE_SERVICE_COTA_ENABLED = y`，可以支持云端推送配置信息到设备。
-   优化主子设备功能：主子设备添加部分功能。

 |
|版本V2.03|2018/01/31|C语言|64位Linux, GNU Make|[RELEASED\_V2\_03\_20180131.zip](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iot-sdk-c/RELEASED_V2_03_20180130.7z?spm=a2c4g.11186623.2.12.VMVBFk&file=RELEASED_V2_03_20180130.7z)| -   支持主子设备功能: 设置`FEATURE_SUBDEVICE_ENABLED = y`，可以支持子设备通过主设备\(网关设备\)进行数据交互。
-   升级HTTP通道: 优化HTTP流程。
-   优化TLS: 修复内存泄漏问题。
-   优化OTA的配置: 可以更合理的开关OTA功能。
-   升级MQTT通道: 支持topic更长，更多的订阅请求，MQTT支持多线程。

 |
|版本V2.02|2017/11/30|C语言|64位Linux, GNU Make|[RELEASED\_V2\_02\_20171130.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_02_20171130.zip?spm=a2c4g.11186623.2.13.VMVBFk&file=RELEASED_V2_02_20171130.zip)| -   正式的多平台支持: 使用`make reconfig`可弹出和选择`Ubuntu16.04`以外的已适配平台。
-   新增Windows版本: 支持用mingw32工具链编译`Win7`版本的库和例程。
-   新增OpenSSL适配: 新增了配合`openssl-0.9.x`+`Windows`版本的HAL参考实现。
-   优化HTTP接口: HTTP通道方面接口优化, 支持发送报文而不断开TLS连接。
-   自包含的安全库: 新增裁剪版本的安全库`mbedtls`, 目前可适配Linux/Windows平台。

 |
|版本V2.01|2017/10/10|C语言|64位Linux, GNU Make|[RELEASED\_V2\_01\_20171010.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_01_20171010.zip?spm=a2c4g.11186623.2.14.VMVBFk&file=RELEASED_V2_01_20171010.zip)| -   新增CoAP+OTA: 允许配置成基于CoAP通知方式的OTA。
-   新增HTTP+TLS: 在MQTT/CoAP之外, 新增HTTP的通道。
-   细化OTA状态: 优化OTA部分代码, 使云端可以更细化的区分设备的OTA固件下载状态。
-   ArmCC支持: 修正了SDK在ArmCC编译器编译时会出现的报错。

 |
|版本V2.00|2017/08/21|C语言|64位Linux, GNU Make|[RELEASED\_V2\_00\_20170818.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_00_20170818.zip?spm=a2c4g.11186623.2.15.VMVBFk&file=RELEASED_V2_00_20170818.zip)| -   新增MQTT直连: 支持更快更轻的连接IoT套件, 去掉对HTTPS/HTTP的依赖, 可看[公告](https://help.aliyun.com/document_detail/57164.html?spm=a2c4g.11186623.2.16.VMVBFk)。
-   新增CoAP通道: 基于UDP, 在纯上报数据场景更节省资源， 可看[公告](https://help.aliyun.com/document_detail/57566.html?spm=a2c4g.11186623.2.17.VMVBFk)。
-   新增OTA通道: 提供一系列OTA相关的API, 可查询/触发/下载用户自主上传的固件。
-   升级构建系统: 支持更灵活的组织和配置SDK。

 |
|版本V1.0.1|2017/06/29|C语言|64位Linux, GNU Make|[RELEASED\_V1\_0\_1\_20170629.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V1_0_1_20170629.zip?spm=a2c4g.11186623.2.18.VMVBFk&file=RELEASED_V1_0_1_20170629.zip)| -   **华东2站点:** 第一个正式配合华东2站点的设备端SDK, 全源码发布。
-   **新增设备影子功能:** 具体可参看[设备影子介绍页面](https://help.aliyun.com/document_detail/53930.html?spm=a2c4g.11186623.2.19.VMVBFk)。

 |

## JAVA版本 {#section_dfl_dxm_vdb .section}

|支持协议|更新历史|下载链接|
|----|----|----|
|MQTT|2017-05-27：支持华东2节点的设备认证流程，同时添加java端设备影子demo。|[iotx-sdk-mqtt-java](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iotx-sdk-java/iotx-sdk-mqtt-java-20170526.zip?spm=a2c4g.11186623.2.20.VMVBFk&file=iotx-sdk-mqtt-java-20170526.zip) ：MQTT的JAVA版只是使用开源库实现的一个demo，仅用于参考。|

## Android版本 {#section_crn_dym_vdb .section}

下载地址：[https://github.com/eclipse/paho.mqtt.android](https://github.com/eclipse/paho.mqtt.android?spm=a2c4g.11186623.2.21.VMVBFk&file=paho.mqtt.android)

## IOS版本 {#section_dzm_xtn_12b .section}

下载地址为：

-   [https://github.com/CocoaPods/Specs.git](https://github.com/CocoaPods/Specs.git)
-   [https://github.com/aliyun/aliyun-specs.git](https://github.com/aliyun/aliyun-specs.git)

## 其它开源库参考 {#section_klv_2ym_vdb .section}

下载地址为：[https://github.com/mqtt/mqtt.github.io/wiki/libraries](https://github.com/mqtt/mqtt.github.io/wiki/libraries?spm=a2c4g.11186623.2.22.VMVBFk)

