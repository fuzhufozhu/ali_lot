# MQTT协议规范 {#concept_jfq_vjw_vdb .concept}

MQTT是基于TCP/IP协议栈构建的异步通信消息协议，是一种轻量级的发布/订阅信息传输协议。MQTT在时间和空间上，将消息发送者与接受者分离，可以在不可靠的网络环境中进行扩展。适用于设备硬件存储空间有限或网络带宽有限的场景。物联网平台支持设备使用MQTT协议接入。

## 支持版本 {#section_mly_bkw_vdb .section}

目前阿里云支持MQTT标准协议接入，兼容3.1.1和3.1版本协议，具体的协议请参见 [MQTT 3.1.1](http://mqtt.org/) 和 [MQTT 3.1](http://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html) 协议文档。

## 与标准MQTT的区别 {#section_gl4_2kw_vdb .section}

-   支持MQTT 的 PUB、SUB、PING、PONG、CONNECT、DISCONNECT和UNSUB等报文。
-   支持clean session。
-   不支持will、retain msg。
-   不支持QoS2。
-   基于原生的MQTT Topic上支持RRPC同步模式，服务器可以同步调用设备并获取设备回执结果。

## 安全等级 {#section_eqt_kkw_vdb .section}

-   TCP通道基础 + TLS协议（TLSV1、 TLSV1.1和TLSV1.2 版本）：安全级别高。
-   TCP通道基础＋芯片级加密（ID²硬件集成）： 安全级别高。
-   TCP通道基础＋对称加密（使用设备私钥做对称加密）：安全级别中。
-   TCP方式（数据不加密）： 安全级别低。

## Topic规范 {#section_z5j_pkw_vdb .section}

Topic定义及分类，请查看[什么是Topic](../../../../intl.zh-CN/用户指南/产品与设备/Topic/什么是Topic.md#)。

系统默认通信类Topic可前往控制台设备详情页查看，功能类Topic可前往具体功能文档页查看。

