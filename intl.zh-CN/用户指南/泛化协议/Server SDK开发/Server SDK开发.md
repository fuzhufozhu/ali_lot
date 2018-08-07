# Server SDK开发 {#concept_ld3_tvk_q2b .concept}

基于泛化协议Server SDK，您可以快速搭建桥接物联网平台的接入服务，使设备或者服务快速接入阿里云物联网平台。

## 准备工作 {#section_s2z_tjs_q2b .section}

泛化协议Server SDK概念、功能及Maven依赖，请参考[概览](intl.zh-CN/用户指南/泛化协议/概览.md#)。

## 配置管理 {#section_a4y_psc_p2b .section}

泛化协议Server SDK默认提供基于文件的配置管理，可通过在[application.conf](intl.zh-CN/用户指南/泛化协议/核心SDK开发.md#section_fy2_5s1_p2b) 中增加 **socketServer** 配置项设置如下表所示的Socket Server相关参数。自定义配置管理请参见[自定义组件\>配置管理](#section_l4y_psc_p2b)。

|名称|描述|是否必须|
|--|--|----|
|address|连接服务监听地址，支持网卡名称如eth1，IPv4地址前缀如10.30，建议明确指定|否|
|backlog|TCP连接backlog的数量|否|
|ports|连接服务监听端口，可指定多个，默认为：9123|否|
|listenType|socket server类型。取值为udp或tcp，默认为tcp。不区分大小写|否|
|broadcastEnabled|是否支持udp广播。当listenType为udp时有效，默认为true|否|
|unsecured|是否支持不加密的TCP连接。当listenType为tcp时有效|否|
|keyPassword|证书库密码。当listenType为tcp时有效| 否

 **说明：** 只有同时配置keyPassword、keyStoreFile和keyStoreType才会生效，否则配置将被忽略。

 |
|keyStoreFile|证书库文件地址。当listenType为tcp时有效|否|
|keyStoreType|证书库类型。当listenType为tcp时有效|否|

## 开发接口 {#section_j4y_psc_p2b .section}

以下两篇文档假设开发者对基于Netty的开发已有基本了解。关于Netty的知识，请参考[Netty文档](https://netty.io/wiki/user-guide-for-4.x.html)。

-   [TCP类型Server开发接口](intl.zh-CN/用户指南/泛化协议/Server SDK开发/TCP Server开发接口.md#)
-   [UDP类型Server开发接口](intl.zh-CN/用户指南/泛化协议/Server SDK开发/UDP Server开发接口.md#)

## 自定义组件 {#section_l4y_psc_p2b .section}

除基于文件的配置管理外，开发者还可以自定义server的配置管理。

自定义配置管理需实现接口com.aliyun.iot.as.bridge.server.config.BridgeServerConfigManager，调用 com.aliyun.iot.as.bridge.server.config.ServerConfigFactory.init\(BridgeServerConfigManager bcm\)将默认配置管理组件替换为自定义组件，完成自定义组件的初始化。然后，再启动网桥。

