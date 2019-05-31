# Use the basic features {#concept_jly_vcm_42b .concept}

Based on the generic protocol SDK, your device can connect to and communicate with Alibaba Cloud IoT Platform by using the bridge service. This topic describes how to configure the generic protocol SDK to implement basic capabilities, including device connection and disconnection and message upstreaming and downstreaming.

See [generic protocol SDK demo](https://github.com/aliyun/alibabacloud-iot-bridge-core-demo) in GitHub.

## Flow diagram {#section_irb_g4r_ce2 .section}

The following flow diagram shows the overall process for how to use the generic protocol SDK to connect a device to IoT Platform.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16157/155928792945089_en-US.png)

## Import the SDK {#section_d2z_rji_a7t .section}

Add the following dependency in your maven project to import the generic protocol SDK.

``` {#codeblock_wpx_yfv_ge7}
<dependency>
  <groupId>com.aliyun.openservices</groupId>
  <artifactId>iot-as-bridge-sdk-core</artifactId>
  <version>2.0.0</version>
</dependency>
```

## Initialization {#section_zxw_lz9_bpx .section}

**Initialize the SDK**

You must create a BridgeBootstrap object and call the bootstrap method. After the generic protocol SDK initialization is complete, the SDK reads the bridge information and initiates a request for the bridge to connect to IoT Platform.

In addition to calling bootstrap, you can also register the DownlinkChannelHandler callback with the generic protocol SDK to receive downstream messages from IoT Platform.

Sample code:

``` {#codeblock_z2r_93s_6md}
BridgeBootstrap bridgeBootstrap = new BridgeBootstrap();
bridgeBootstrap.bootstrap(new DownlinkChannelHandler() {
    @Override
    public boolean pushToDevice(Session session, String topic, byte[] payload) {
        //get message from cloud
        String content = new String(bytes);
        log.info("Get DownLink message, session:{}, {}, {}", session, topic, content);
        return true;
    }

    @Override
    public boolean broadcast(String topic, byte[] payload) {
        return false;
    }
});
```

**Configure bridge information**

By default, a bridge is configured based on a configuration file. By default, the configuration file is read from application.conf under the default resource file path of the Java project \(generally src/main/resources/\). The file is in the format of [HOCON](https://github.com/lightbend/config/blob/master/HOCON.md) \(JSON superset\). The generic protocol SDK uses typesafe.config to parse the configuration file.

You can configure a bridge device either by specifying a bridge device or dynamically registering a bridge device. This topic only describes how to specify a bridge device. For more information about how to dynamically register a bridge device, see [Dynamically register a bridge device](intl.en-US/User Guide/Generic protocol SDK/Use the advanced features.md#section_xmx_dyi_nok) in [Use the advanced features](intl.en-US/User Guide/Generic protocol SDK/Use the advanced features.md#).

|Parameter|Required|Description|
|:--------|--------|:----------|
|productKey|Yes|The key of the product to which the bridge device belongs .|
|deviceName|No|The device name of the bridge device. -   You must provide this parameter if you have registered the bridge device in advance and want to configure the device based on the specified device certificate information.
-   You do not need to provide this parameter if you have not registered the bridge device in advance and want to use the MAC address of the bridge server as the device name to dynamically register a device with IoT Platform.

 |
|deviceSecret|No|The device secret of the bridge device. -   You must provide this parameter if you have registered the bridge device in advance and want to configure the device based on the specified device certificate information.
-   You do not need to provide this parameter if you choose to dynamically register the bridge device rather than have the bridge device registered in advance.

 |
|http2Endpoint|Yes| The endpoint of the HTTP/2 gateway service. The bridge device and IoT Platform establish a persistent connection over the HTTP/2 protocol. The endpoint is in the format of `${productKey}.iot-as-http2 .${RegionId}.aliyuncs.com:443`.

 Replace $\{ProductKey\} with the ProductKey of the product to which your bridge device belongs.

 Replace $\{RegionId\} with the ID of the region where your service is located. For more information about regions, see [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm).

 For example, if the ProductKey of the bridge device is alabcabc123, the region is China \(Shanghai\), then the HTTP/2 gateway service endpoint is `alabcabc123.iot-as-http2.cn-shanghai.aliyuncs.com:443`.

 |
|authEndpoint|Yes| The service URL for device authentication. The device authentication service URL is in the format of `https://iot-auth .${RegionId}.aliyuncs.com/auth/bridge`.

 Replace $\{RegionId\} with the ID of the region where your service is located. For more information about regions, see [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm).

 For example, if the region is China \(Shanghai\), then the device authentication service address is `https://iot-auth.cn-shanghai.aliyuncs.com/auth/bridge`.

 |
|popClientProfile|No|This parameter must be provided if you use the MAC address of the bridge server to dynamically register the bridge device. For more information, see [Use the advanced features](intl.en-US/User Guide/Generic protocol SDK/Use the advanced features.md#).

 |

Use the following format to configure the bridge device certificate:

``` {#codeblock_4wp_97o_lbw}
# Server endpoint
http2Endpoint = "https://a1tN7OBmTcd.iot-as-http2.cn-shanghai.aliyuncs.com:443"
authEndpoint = "https://iot-auth.cn-shanghai.aliyuncs.com/auth/bridge"

# Gateway device info, productKey & deviceName & deviceSecret
productKey = ${bridge-ProductKey-in-Iot-Plaform}
deviceName = ${bridge-DeviceName-in-Iot-Plaform}
deviceSecret = ${bridge-DeviceSecret-in-Iot-Plaform}
```

## Device authentication and connection {#section_9jq_4yb_dvx .section}

**Configure device connection**

The device connection interface in the generic protocol SDK:

``` {#codeblock_5t3_4ah_5ql}
/**
 * Device authentication
 * @param newSession  Device session information, which is returned in a downstream callback.
 * @param originalIdentity  The  original identity of the device
 * @return
 */
public boolean doOnline(Session newSession, String originalIdentity);
```

When the device is connected to the bridge device, it must pass in a session. When a downstream message is called back, the session is called back to the bridge device. The session contains the original identifier field, so that the bridge device can determine which device the message came from.

In addition, the session also has an optional channel field, which can be designed to store device connection information. For example, your bridge server is built based on Netty. You can use this field to store the channel object corresponding to the persistent connection of the device. When a message is sent from IoT Platform, the bridge device can directly obtain the channel from the session for subsequent operations. The data type of the channel field is Object. The generic protocol SDK does not process data stored in the channel field. You can also store any device-related information in the channel field according to the scenario.

Sample code for device connection:

``` {#codeblock_rjw_p93_xhl}
UplinkChannelHandler uplinkHandler = new UplinkChannelHandler();
//Create a session
Object channel = new Object();
Session session = Session.newInstance(originalIdentity, channel);
//Connect the device to the bridge
boolean success = uplinkHandler.doOnline(session, originalIdentity);
if (success) {
    //If the device is connected, the bridge device accepts new communication requests from the device.
} else {
    //If the device connection fails, the bridge device rejects subsequent communication requests, such as disconnection requests.
}
```

**Map an original identifier to a device certificate**

You must configure the mapping between the device certificate and the original identifier of a device. By default, a configuration file is used to configure the mapping. The configuration file is read from devices.conf under the default resource file path of the Java project \(generally src/main/resources/\). The file is in the format of [HOCON](https://github.com/lightbend/config/blob/master/HOCON.md) \(JSON superset\). The generic protocol SDK uses typesafe.config to parse the configuration file.

Use the following format to configure the device certificate information:

``` {#codeblock_6gk_c1g_ymx}
${device-originalIdentity} {
  prodyctKey : ${device-ProductKey-in-Iot-Plaform}
  deviceName : ${device-DeviceName-in-Iot-Platform}
  deviceSecret : ${device-DeviceSceret-in-Iot-Platform}
}
```

|Parameter|Required|Description|
|:--------|:-------|-----------|
|productKey|Yes|The key of the product to which the device belongs.|
|deviceName|Yes|The device name.|
|deviceSecret|Yes|The device secret.|

## Device sends data to IoT Platform {#section_7j5_9cq_com .section}

The interface for data upstreaming in the generic protocol SDK:

``` {#codeblock_oi8_4zc_vvz}
/**
 * Send upstream messages from the device by synchronously calling the interface
 * @param originalIdentity  The original identifier of the device
 * @param protocolMsg  The message to be sent, including the topic, payload, and QoS information
 * @param timeout  The timeout period in seconds
 * @return  Indicates whether the message is sent successfully within the timeout period
 */
boolean doPublish(String originalIdentity, ProtocolMessage protocolMsg, int timeout);
/**
 * Send upstream messages from the device by asynchronously calling the interface
 * @param originalIdentity  The original identifier of the device
 * @param protocolMsg  The message to be sent, including the topic, payload, and QoS information
 * @return  After this interface is called, CompletableFuture is returned immediately. The caller can further process this Future.
 */
CompletableFuture<ProtocolMessage> doPublishAsync(String originalIdentity, 
                                                  ProtocolMessage protocolMsg);
```

Sample code:

``` {#codeblock_xnv_kr2_ygb}
DeviceIdentity deviceIdentity = 
    ConfigFactory.getDeviceConfigManager().getDeviceIdentity(originalIdentity);
ProtocolMessage protocolMessage = new ProtocolMessage();
protocolMessage.setPayload("Hello world".getBytes());
protocolMessage.setQos(0);
protocolMessage.setTopic(String.format("/%s/%s/update", 
        deviceIdentity.getProductKey(), deviceIdentity.getDeviceName()));
//Synchronous sending
int timeoutSeconds = 3;
boolean success = upLinkHandler.doPublish(originalIdentity, protocolMessage, timeoutSeconds);
//Asynchronous sending
upLinkHandler.doPublishAsync(originalIdentity, protocolMessage);
```

## Bridge device pushes data to device {#section_teb_hgt_5f7 .section}

When the bridge device calls the bootstrap method, it registers DownlinkChannelHandler with the generic protocol SDK. When the generic protocol SDK receives a downstream message, it calls back the pushToDevice method in DownlinkChannelHandler. You can edit the pushToDevice method to configure the bridge device to process downstream messages.

**Note:** Do not create a time-consuming logic in the pushToDevice method. Otherwise, the thread that receives downstream messages will be blocked. Use the asynchronous transmission if a time-consuming logic or I/O logic exists, for example, sending downstream messages through a persistent connection to the devices.

Sample code:

``` {#codeblock_fx6_31v_inv}
private static ExecutorService executorService  = new ThreadPoolExecutor(
    Runtime.getRuntime().availableProcessors(),
    Runtime.getRuntime().availableProcessors() * 2,
    60, TimeUnit.SECONDS,
    new LinkedBlockingQueue<>(1000),
    new ThreadFactoryBuilder().setDaemon(true).setNameFormat("bridge-downlink-handle-%d").build(),
    new ThreadPoolExecutor.AbortPolicy());
public static void main(String args[]) {
    //Use application.conf & devices.conf by default
    bridgeBootstrap = new BridgeBootstrap();
    bridgeBootstrap.bootstrap(new DownlinkChannelHandler() {
        @Override
        public boolean pushToDevice(Session session, String topic, byte[] payload) {
            //get message from cloud
            //get downlink message from cloud
            executorService.submit(() -> handleDownLinkMessage(session, topic, payload));
            return true;
        }
        @Override
        public boolean broadcast(String s, byte[] bytes) {
            return false;
        }
    });
}
private static void handleDownLinkMessage(Session session, String topic, byte[] payload) {
    String content = new String(payload);
    log.info("Get DownLink message, session:{}, topic:{}, content:{}", session, topic, content);
    Object channel = session.getChannel();
    String originalIdentity = session.getOriginalIdentity();
    //for example, you can send the message to device via channel, it depends on you specific server implementation
}
```

|Parameter|Description|
|:--------|:----------|
|Session|A session is transmitted by a device when the device is connecting to the bridge device. A session can be used to identify the device to which the downstream message is sent.|
|topic|The topic of the downstream message.|
|payload| The payload of a downstream message in binary format.

 |

## Device disconnection {#section_dx9_225_h22 .section}

A device is disconnected under the following situations:

-   When the bridge device is disconnected from IoT Platform, all connected devices are automatically disconnected from IoT Platform.
-   The bridge device reports a disconnection request for a device to IoT Platform.

The interface for bridge device to report device disconnection in the generic protocol SDK:

``` {#codeblock_tri_3el_03f}
/**
 * Report a disconnection request to IoT Platform for a device
 * @param originalIdentity  The original identifier of the device
 * @return  Indicates whether the message is sent successfully
 */
boolean doOffline(String originalIdentity);
```

Sample code:

``` {#codeblock_wj5_4z3_42q}
upLinkHandler.doOffline(originalIdentity);
```

