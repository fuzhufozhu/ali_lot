# Core SDK {#concept_jly_vcm_42b .concept}

You can integrate the IoT Platform bridge service with existing connection services or platforms using the general protocol core SDK, allowing devices or services to quickly access Alibaba Cloud IoT Platform.

## Prerequisites {#section_s2z_tjs_q2b .section}

Refer to [Overview](reseller.en-US/Developer Guide (Devices)/General protocols/Overview.md#) for concepts, functions and Maven dependencies of the general protocol core SDK.

## Configuration Management {#section_fy2_5s1_p2b .section}

The general protocol core SDK uses file-based configuration management by default. For customized configurations, refer to [Custom components \> Configuration Management](#section_vz2_5s1_p2b).

-   Supported formats include: Java Properties, JSON and high readable [HOCON](https://github.com/lightbend/config/blob/master/HOCON.md).
-   Supports structured configuration to simplify maintenance.
-   Supports override file configurations with Java system properties like java -Dmyapp.foo.bar=10.
-   Supports configuration file separation and nested references.

**application.conf**

|Parameter|Description|Required|
|---------|-----------|--------|
|productKey|The ProductKey of the net bridge product|Yes|
|deviceName|The device name of the net bridge device. The default value is ECS MAC address.|No|
|deviceSecret|The device secret of the gateway|No|
|http2Endpoint|HTTP/2 gateway service address|Yes|
|authEndpoint|The service address of the device authentication|Yes|
|popClientProfile|Call Open API to configure the client. For details, refer to [API Client Configuration](reseller.en-US/Developer Guide (Devices)/General protocols/Core SDK.md#table_my2_5s1_p2b).|Yes|

**API Client Configuration**

|Parameter|Description|Required|
|---------|-----------|--------|
|accessKey|The access key of the API caller|Yes|
|accessSecret|The secret key of the API caller|Yes|
|name |The profile name of the API client|Yes|
|region|The region of the service|Yes|
|product|The name of the product. Set it to *Iot* if not specified.|Yes|
|endpoint|The endpoint of the service|Yes|

**devices.conf**

Configure the ProductKey, DeviceName, and DeviceSecret of the device on Alibaba Cloud IoT Platform. For customizing configuration files, refer to [Custom Components \> Configuration Management](#section_vz2_5s1_p2b).

```
XXXX //  Original identifiers of the device
{
    "productKey": "123",
    deviceName: "",
    deviceSecret: ""
}

```

## Interfaces {#section_sy2_5s1_p2b .section}

**Initialization**

com.aliyun.iot.as.bridge.core.BridgeBootstrap initializes the communication between the device and Alibaba Cloud IoT Platform. After the BridgeBootstrap instance is created, the [Basic Configuraions](#section_fy2_5s1_p2b) component of the gateway will be initialized. Refer to [Custom Components \> Configuration Management](#section_uz2_5s1_p2b) for information about customizing configurations.

Complete the initialization using one of the following interfaces:

-   bootstrap\(\): initialization without downstream messaging
-   bootstrap\(DownlinkChannelHandler handler\): initialize using DownlinkChannelHandler specified by the developer.

Example:

```
BridgeBootstrap bootstrap = new BridgeBootstrap();
// Do not implement downstream messaging
bootstrap.bootstrap();
```

**Device Online**

Only online devices can establish a connection with or send connection requests to IoT Platform. There are two steps for devices to get online: local session initialization and device authentication.

1.  Session Initialization

    The general protocol SDK provides non-persistent local session management. Refer to [Custom Components \> Session Management](#section_uz2_5s1_p2b) for customization.

    Interfaces for creating new instances:

    -   com.aliyun.iot.as.bridge.core.model.Session.newInstance\(String originalIdentity, Object channel\)
    -   com.aliyun.iot.as.bridge.core.model.Session.newInstance\(String originalIdentity, Object channel, int heartBeatInterval\)
    -   com.aliyun.iot.as.bridge.core.model.Session.newInstance\(String originalIdentity, Object channel, int heartBeatInterval, int heartBeatProbes\)
    originalIdentity represents the unique device identifier, and has the same function as SN in original protocol. channel is the communication channel between devices and bridge service, and has the same function as a channel in Netty. heartBeatInterval and heartBeatProbes are used for heartbeat monitoring. The unit of heartBeatInterval is seconds. heartBeatProbes indicates the maximum number of lost heartbeats that is allowed. If this number is exceeded, a heartbeat timeout event will be sent. Developers can handle the timeout event by registering com.aliyun.iot.as.bridge.core.session.SessionListener.

2.  Device Authentication

    After the initialization of local device session, use com.aliyun.iot.as.bridge.core.handler.UplinkChannelHandler.doOnline\(Session newSession, String originalIdentity, String... credentials\) to complete local device authentication and Alibaba Cloud IoT Platform online authentication. The device will then either be allowed to communicate or will be disconnected according to the authentication result. SDK provides online authentication for IoT Platform. By default, local authentication is disabled. If you need to set up local authentication, refer to [Customized Components \> Connection Authentication](#section_tz2_5s1_p2b).

    Example:

    ```
    UplinkChannelHandler uplinkHandler = new UplinkChannelHandler();
    Session session = session. newinstance (device, Channel );
    boolean success = uplinkHandler.doOnline(session, originalIdentity);
    if (success) {
        // Successfully got online, and will accept communication requests.
    } else {
        // Failed to get online, will reject communication requests and disconnect (if connected).
    }
    ```


**Device Offline**

When a device disconnects or detects that it needs to disconnect, a device offline operation should be initiated. Use com.aliyun.iot.as.bridge.core.handler.UplinkChannelHandler.doOffline\(String originalIdentity\) to bring a device offline.

Example:

```
UplinkChannelHandler uplinkHandler = new UplinkChannelHandler();
Uplinkhandler. dooffline (originalidentity );
```

**Report Data**

Developers can use com.aliyun.iot.as.bridge.core.handler.UplinkChannelHandler to report data to Alibaba Cloud IoT Platform. Three steps for data report: identify the device who is going to report data, found the corresponding session for this device, report data to IoT Platform. Use the following interfaces to report data.

**Note:** Be sure that data report is fully under control and security issues has been taken care of.

-   CompletableFuture doPublishAsync\(String originalIdentity, String topic, byte\[\] payload, int qos\): send data asynchronously and return immediately. Developers obtain the sending result using future.
-   CompletableFuture doPublishAsync\(String originalIdentity, ProtocolMessage protocolMsg\): send data asynchronously and return immediately. Developers obtain the sending result using future.
-   boolean doPublish\(String originalIdentity, ProtocolMessage protocolMsg, int timeout\): send data asynchronously and wait for the response.
-   boolean doPublish\(String originalIdentity, String topic, byte\[\] payload, int qos, int timeout\): send data asynchronously and wait for the response.

Example:

```
UplinkChannelHandler uplinkHandler = new UplinkChannelHandler(); 
DeviceIdentity identity = ConfigFactory.getDeviceConfigManager().getDeviviceIdentity(device); 
if (identity == null) { 
    // Devices are not mapped with those registered on IoT Platform. Messages are dropped. 
    return; 
} 
Session session = SessionManagerFactory.getInstance().getSession(device); 
if (session == null) { 
    // The device is not online. Please get online or drop messages. Make sure devices are online before reporting data to IoT Platform. 
} 
boolean success = uplinkHandler.doPublish(session, topic, payload, 0, 10); 
if(success) { 
    // Data is successfully reported to Alibaba Cloud IoT Platform. 
} else { 
    // Failed to report data to IoT Platform
}   
```

**Downstream Messaging**

The general protocol SDK provides com.aliyun.iot.as.bridge.core.handler.DownlinkChannelHandler as the downstream data distribution processor. It supports unicast and broadcast \(if the message sent by the cloud does not include specific device information\).

Example:

```
public class SampleDownlinkHandler implements DownlinkChannelHandler {
    @Override
    public boolean pushToDevice(Session session, String topic, byte[] payload) {
        // Process messages pushed to the device
    }

    @Override
    public boolean broadcast(String topic, byte[] payload) {
        // Process broadcast
    }
}

```

## Custom Components {#section_sz2_5s1_p2b .section}

Developers can customize device connection authentication, session management, and configuration management components. Complete the initialization and substitution of those components before calling BridgeBootstrap intialization.

**Connection Authentication**

Custom device connection authentication should implement com.aliyun.iot.as.bridge.core.auth.AuthProvider and before initialize BridgeBootstrapcall, call com.aliyun.iot.as.bridge.core.auth.AuthProviderFactory.init\(AuthProvider customizedProvider\) to replace the original authentication component with the customized one.

**Session Management**

Custom session management should implement com.aliyun.iot.as.bridge.core.session.SessionManager and before initialize BridgeBootstrapcall, call com.aliyun.iot.as.bridge.core.session.SessionManagerFactory.init\(SessionManager <?\> customizedSessionManager\) to replace the original session management component with the customized one.

**Configuration Management**

Custom Configuration Management should implement com.aliyun.iot.as.bridge.core.config.DeviceConfigManager and com.aliyun.iot.as.bridge.config.BridgeConfigManager. Before initialize BridgeBootstrapcall, call com.aliyun.iot.as.bridge.core.config.ConfigFactory.init\(BridgeConfigManager bcm, DeviceConfigManager dcm\) to replace the original configuration management component with the customized one. Parameters can be left blank. If so, the general protocol SDK default values will be used.

