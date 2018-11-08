# Develop Core SDK {#concept_jly_vcm_42b .concept}

You can integrate the IoT Platform bridge service with existing connection services or platforms that use the general protocol core SDK to allow devices or servers to quickly access Alibaba Cloud IoT Platform.

## Prerequisites {#section_s2z_tjs_q2b .section}

For information about the concepts, functions, and Maven dependencies of the general protocol core SDK, see [Overview](reseller.en-US/Developer Guide (Devices)/General protocols/Overview.md#).

## Configuration management {#section_fy2_5s1_p2b .section}

The general protocol core SDK uses file-based configuration management by default. For information about customized configurations, see [Custom components \> Configuration management](#section_sz2_5s1_p2b). The general protocol core SDK supports:

-   Java Properties, JSON, and [HOCON](https://github.com/lightbend/config/blob/master/HOCON.md) formats.
-   Structured configuration to simplify maintenance.
-   The override of file configurations with Java system properties, such as java -Dmyapp.foo.bar=10.
-   Configuration file separation and nested references.

|Parameter|Required|Description|
|:--------|:-------|:----------|
|productKey|Yes|The product ID of the net bridge product.|
|deviceName|No|The device name of the net bridge device. The default value is the ECS instance MAC address.|
|deviceSecret|No|The device secret of the net bridge device.|
|http2Endpoint|Yes|HTTP/2 gateway service address.The address format is `$\{UID\}.iot-as-http2. $\{RegionId\}.aliyuncs.com:443`.

where:

-   $\{UID\} indicates your account ID. To view your account ID, log on to the Alibaba Cloud console, hover your mouse over your account image, and click **Security Settings**. You are then directed to the Account Management page that displays your account ID.
-   $\{RegionId\} indicates the region ID where your service is located. For example, if the region is Shanghai, the HTTP/2 gateway service address is `123456789.iot-as-http2.cn-shanghai.aliyuncs.com:443`.

For information about RegionId expressions, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).


|
|authEndpoint|Yes|Device authentication service addressDevice authentication service address: `https://iot-auth. $\{RegionId\}.aliyuncs.com/auth/bridge`.

$\{RegionId\} indicates the region ID where your service is located. For example, if the region is Shanghai, the device authentication service address is `https://iot-auth.cn-shanghai.aliyuncs.com/auth/bridge`.

For information about RegionId expressions, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

|
|popClientProfile|Yes|Call APIs to configure the client. For details, see the [API client configuration](#table_xr3_12v_hfb).|

|Parameter|Required|Description|
|:--------|:-------|:----------|
|accessKey|Yes|The access key of the API caller.|
|accessSecret|Yes|The secret key of the API caller.|
|name|Yes|The region name of the API.|
|region|Yes|The region ID of the API.|
|product|Yes|The name of the product. Set it to `Iot` if not specified.|
|endpoint|Yes|The endpoint of the API.Endpoint structure: `iot. $\{RegionId\}.aliyuncs.com`.

$\{RegionId\} indicates the region ID of your service. For example, If the region is Shanghai, the endpoint is `iot.cn-shanghai.aliyuncs.com`.

For information about RegionId expressions, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

|

**devices.conf**

Configure the ProductKey, DeviceName, and DeviceSecret of the device. For information about customizing configuration files, see [Custom components \> Configuration management](#section_sz2_5s1_p2b).

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

`com.aliyun.iot.as.bridge.core.BridgeBootstrap` initializes the communication between the device and Alibaba Cloud IoT Platform. After the BridgeBootstrap instance is created, the [Basic configurations](#section_fy2_5s1_p2b) component of the gateway will be initialized. For information about customizing configurations, see [Custom components \> Configuration management](#section_sz2_5s1_p2b).

Complete the initialization using one of the following interfaces:

-   `bootstrap()`: initialization without downstream messaging.
-   `bootstrap(DownlinkChannelHandler handler)`: initialize using DownlinkChannelHandler specified by the developer.

Sample code:

```
BridgeBootstrap bootstrap = new BridgeBootstrap();
// Do not implement downstream messaging
bootstrap.bootstrap();
```

**Connect devices to IoT Platform**

Only devices that are online can establish a connection with or send connection requests to IoT Platform. There are two methods that can enable devices to get online: local session initialization and device authentication.

1.  Session initialization

    The general protocol SDK provides non-persistent local session management. See [Custom components \> Session management](#section_sz2_5s1_p2b) for information on customization.

    Interfaces for creating new instances:

    -   `com.aliyun.iot.as.bridge.core.model.Session.newInstance(String originalIdentity, Object channel)`
    -   `com.aliyun.iot.as.bridge.core.model.Session.newInstance(String originalIdentity, Object channel, int heartBeatInterval)`
    -   `com.aliyun.iot.as.bridge.core.model.Session.newInstance(String originalIdentity, Object channel, int heartBeatInterval, int heartBeatProbes)`
    originalIdentity indicates the unique device identifier and has the same function as SN in the original protocol. channel is the communication channel between devices and bridge service, and has the same function as a channel in Netty. heartBeatInterval and heartBeatProbes are used for heartbeat monitoring. The unit of heartBeatInterval is seconds. heartBeatProbes indicates the maximum number of undetected heartbeats that is allowed. If this number is exceeded, a heartbeat timeout event will be sent. To handle a timeout event, register `com.aliyun.iot.as.bridge.core.session.SessionListener`.

2.  Authenticate devices

    After the initialization of local device session, use `com.aliyun.iot.as.bridge.core.handler.UplinkChannelHandler.doOnline(Session newSession, String originalIdentity, String... credentials)` to complete local device authentication and Alibaba Cloud IoT Platform online authentication. The device will then either be allowed to communicate or will be disconnected according to the authentication result. SDK provides online authentication for IoT Platform. By default, local authentication is disabled. If you need to set up local authentication, see [Customized components \> Connection authentication](#section_sz2_5s1_p2b).

    Sample code:

    ```
    UplinkChannelHandler uplinkHandler = new UplinkChannelHandler();
    Session session = session. newinstance (device, Channel );
    boolean success = uplinkHandler.doOnline(session, originalIdentity);
    if (success) {
        // Successfully got online, and will accept communication requests.
    } else {
        // Failed to get online, and will reject communication requests and disconnect (if connected).
    }
    ```


**Device Offline**

When a device disconnects or detects that it needs to disconnect, a device offline operation must be initiated. Use `com.aliyun.iot.as.bridge.core.handler.UplinkChannelHandler.doOffline(String originalIdentity)` to bring a device offline.

Sample code:

```
UplinkChannelHandler uplinkHandler = new UplinkChannelHandler();
Uplinkhandler. dooffline (originalidentity );
```

**Report Data**

You can use `com.aliyun.iot.as.bridge.core.handler.UplinkChannelHandler` to report data to Alibaba Cloud IoT Platform. Data reporting involves three key steps: identify the device that is going to report data, locate the corresponding session for this device, and report data to IoT Platform. Use the following interfaces to report data.

**Note:** Make sure that the data report has been managed and security issues have been handled.

-   `CompletableFuture doPublishAsync(String originalIdentity, String topic, byte[] payload, int qos)`: send data asynchronously and return immediately. You can then obtain the sending result using future.
-   `CompletableFuture doPublishAsync(String originalIdentity, ProtocolMessage protocolMsg)`: send data asynchronously and return immediately. You can then obtain the sending result using future.
-   `boolean doPublish(String originalIdentity, ProtocolMessage protocolMsg, int timeout)`: send data asynchronously and wait for the response.
-   `boolean doPublish(String originalIdentity, String topic, byte[] payload, int qos, int timeout)`: send data asynchronously and wait for the response.

Sample code:

```
UplinkChannelHandler uplinkHandler = new UplinkChannelHandler(); 
DeviceIdentity identity = ConfigFactory.getDeviceConfigManager().getDeviviceIdentity(device); 
if (identity == null) { 
    // Devices are not mapped with those registered on IoT Platform, and messages are dropped. 
    return; 
} 
Session session = SessionManagerFactory.getInstance().getSession(device); 
if (session == null) { 
    // The device is not online. You can either get the device online or drop messages. Make sure devices are online before reporting data to IoT Platform. 
} 
boolean success = uplinkHandler.doPublish(session, topic, payload, 0, 10); 
if(success) { 
    // Data is successfully reported to Alibaba Cloud IoT Platform. 
} else { 
    // Failed to report data to IoT Platform
}   
```

**Downstream Messaging**

The general protocol SDK provides `com.aliyun.iot.as.bridge.core.handler.DownlinkChannelHandler` as the downstream data distribution processor. It supports unicast and broadcast \(if the message sent from the cloud does not include specific device information\).

Sample code:

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

## Custom components {#section_sz2_5s1_p2b .section}

You can customize the device connection authentication, session management, and configuration management components. You must complete the initialization and substitution of those components before calling BridgeBootstrap intialization.

**Connection authentication**

To customize the device connection authentication, implement `com.aliyun.iot.as.bridge.core.auth.AuthProvider` and then, before initializing BridgeBootstrapcall, call `com.aliyun.iot.as.bridge.core.auth.AuthProviderFactory.init(AuthProvider customizedProvider)` to replace the original authentication component with the customized component.

**Session management**

To customize the session management, implement `com.aliyun.iot.as.bridge.core.session.SessionManager` and then, before initializing BridgeBootstrapcall, call `com.aliyun.iot.as.bridge.core.session.SessionManagerFactory.init(SessionManager <? > customizedSessionManager)` to replace the original session management component with the customized component.

**Configuration management**

To customize the configuration management, implement `com.aliyun.iot.as.bridge.core.config.DeviceConfigManager` and `com.aliyun.iot.as.bridge.config.BridgeConfigManager`. Then, before initializing BridgeBootstrapcall, call `com.aliyun.iot.as.bridge.core.config.ConfigFactory.init(BridgeConfigManager bcm, DeviceConfigManager dcm)` to replace the original configuration management component with the customized component. If the parameters are left empty, the general protocol SDK default values will be used.

