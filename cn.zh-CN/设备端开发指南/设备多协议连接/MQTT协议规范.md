# MQTT协议规范 {#concept_jfq_vjw_vdb .concept}

## 支持版本 {#section_mly_bkw_vdb .section}

目前阿里云支持MQTT标准协议接入，兼容3.1.1和3.1版本协议，具体的协议请参考 [MQTT 3.1.1](http://mqtt.org/) 和 [MQTT 3.1](http://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html) 协议文档。

## 与标准MQTT的区别 {#section_gl4_2kw_vdb .section}

-   支持MQTT 的 PUB、SUB、PING、PONG、CONNECT、DISCONNECT和UNSUB等报文。
-   支持clean session。
-   不支持will、retain msg。
-   不支持QoS2。
-   基于原生的MQTT Topic上支持RRPC同步模式，服务器可以同步调用设备并获取设备回执结果。

## 安全等级 {#section_eqt_kkw_vdb .section}

支持 TLSV1、 TLSV1.1和TLSV1.2 版本的协议建立安全连接。

-   TCP通道基础＋芯片级加密（ID2硬件集成）： 安全级别高。
-   TCP通道基础＋对称加密（使用设备私钥做对称加密）：安全级别中。
-   TCP方式（数据不加密）： 安全级别低。

## Topic规范 {#section_z5j_pkw_vdb .section}

Topic定义及分类，请查看[什么是Topic](../../../../../intl.zh-CN/用户指南/产品与设备/Topic/什么是Topic.md#)。

系统默认通信类Topic可前往控制台设备详情页查看，功能类Topic可前往具体功能文档页查看。

物联网平台Topic约定如下：

-   /sys开头的Topic：属于系统约定的应用协议通信标准，不允许自定义，定的Topic中传输的数据必须符合阿里云Alink格式数据标准。
-   /broadcast开头的Topic类：广播使用的特定Topic。
-   RRPC相关Topic：用于设备端于服务器进行同步通信。

