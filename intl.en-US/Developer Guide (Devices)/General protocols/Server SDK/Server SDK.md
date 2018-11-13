# Server SDK {#concept_ld3_tvk_q2b .concept}

You can use the general protocol server SDK to quickly build a bridge service that connects your existing devices or services to Alibaba Cloud IoT Platform.

## Prerequisites {#section_s2z_tjs_q2b .section}

Refer to [Overview](intl.en-US/Developer Guide (Devices)/General protocols/Overview.md#) for concepts, functions and Maven dependencies of the general protocol server SDK.

## Configuration Management {#section_a4y_psc_p2b .section}

The general protocol server SDK uses file-based configuration management by default. Add the **socketServer** parameter in [application.conf](intl.en-US/Developer Guide (Devices)/General protocols/Develop Core SDK.md#section_fy2_5s1_p2b), and set the socket server related parameters listed in the following table. For customized configuration, refer to [Custom Components \> Configuration Management](#section_l4y_psc_p2b).

|Parameter|Description|Required|
|---------|-----------|--------|
|address|The connection listening address. Supports network names like eth1, and IPv4 addresses with 10.30 prefix.|No|
|backlog|The number of backlogs for TCP connection.|No|
|ports:|Connection listening port. The default port is 9123. You can specify multiple ports.|No|
|listenType|The type of socket server. Can be udp or tcp. The default value is tcp. Case insensitive.|No|
|broadcastEnabled|Whether UDP broadcasts are supported. Used when listenType is udp. The default value is true.|No|
|unsecured|Whether unencrypted TCP connection is supported. Used when listenType is tcp.|No|
|keyPassword|The certificate store password. Used when listenType is tcp.| No

 **Note:** Effective when keyPassword, keyStoreFile, and keyStoreType are all configured. Otherwise, keyPassword does not need to be configured.

 |
|keyStoreFile|The file address of the certificate store. Used when listenType is tcp.|No|
|keyStoreType|The type of certificate store. Used when listenType is tcp.|No|

## Interfaces {#section_j4y_psc_p2b .section}

The following two articles assume that you have a basic understanding of Netty-based development. Refer to [Netty Documentation](https://netty.io/wiki/user-guide-for-4.x.html) for more details on Netty-based development.

-   [Interfaces for TCP](intl.en-US/Developer Guide (Devices)/General protocols/Server SDK/Interfaces for TCP.md#)
-   [Interfaces for UDP](intl.en-US/Developer Guide (Devices)/General protocols/Server SDK/Interfaces for UDP.md#)

## Custom Components {#section_l4y_psc_p2b .section}

Besides file-based configuration, you can also set your own customized configurations.

If you want to customize configurations, implement com.aliyun.iot.as.bridge.server.config.BridgeServerConfigManager first and call com.aliyun.iot.as.bridge.server.config.ServerConfigFactory.init\(BridgeServerConfigManager bcm\) to replace default configuration management components with customized ones, and then initialize these components. Then, connect the net bridge products to the Internet.

