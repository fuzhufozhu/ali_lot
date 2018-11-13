# Interfaces for UDP {#concept_r33_rhd_p2b .concept}

You can build an access service which uses UDP transmission protocol and bridge it to Alibaba Cloud IoT Platform using the interfaces of the general protocol SDK for UDP.

## Bootstrap {#section_slg_2nh_p2b .section}

com.aliyun.iot.as.bridge.server.BridgeServerBootstrap is the bootstrap class for booting socket server and bridge service. After a new BridgeServerBootstrap is created, components based on configuration files will be initialized.

Example:

```
BridgeServerBootstrap bootstrap = new BridgeServerBootstrap(new UdpDecoderFactory() {
    @Override
 public MessageToMessageDecoder newInstance() {
// Return decoder
    }
}, new UdpEncoderFactory() {
    @Override
public MessageToMessageEncoder<?> newInstance() {
// Return encoder
    }
}, new UdpBasedProtocolAdaptorHandlerFactory() {
    @Override
public CustomizedUdpBasedProtocolHandler newInstance() {
// Return protocol adapter
    }
});
try {
    bootstrap.start();
} catch (BootException | ConfigException e) {
// Process boot exception
}

```

**Instantiation of UDP type BridgeServerBootstrap**

-   com.aliyun.iot.as.bridge.server.channel.factory.UdpDecoderFactory: Create a new decoder instance using the factory method to decode upload data. Thread is secure. Can be null.
-   com.aliyun.iot.as.bridge.server.channel.factory.UdpEncoderFactory: Create a new encoder instance using the factory method to encode downstream data to adapt to UDP protocol. Thread is secure. Can be null.
-   com.aliyun.iot.as.bridge.server.channel.factory.UdpBasedProtocolAdaptorHandlerFactory: Create a new protocol adapter instance using the factory method to adapt decoded data so they can be uploaded to the cloud. Thread is secure. Cannot be null.

**Start socket server**

After the creation of BridgeServerBootstrap, call com.aliyun.iot.as.bridge.server.BridgeServerBootstrap.start\(\) to start the socket server.

## Protocol decoding {#section_vlg_2nh_p2b .section}

The component for protocol decoding derives from io.netty.handler.codec.MessageToMessageDecoder<I\>. Refer to [MessageToMessageDecoder Documentation](http://netty.io/4.1/api/index.html?io/netty/handler/codec/MessageToMessageDecoder.html) for details.

Example:

```
public class SampleDecoder extends  MessageToMessageDecoder<DatagramPacket> {
    @Override
protected void decode(ChannelHandlerContext ctx, DatagramPacket in, List<Object> out) throws Exception {
// The decoding protocol
    }
}
```

## Protocol encoding {#section_wlg_2nh_p2b .section}

The component for protocol encoding derives from io.netty.handler.codec.MessageToMessageEncoder<I\>. Refer to [MessageToMessageEncoder Documentation](http://netty.io/4.1/api/index.html?io/netty/handler/codec/MessageToMessageEncoder.html) for details.

Example:

```
public class SampleEncoder extends MessageToMessageEncoder<T>{
    @Override
    protected void encode(ChannelHandlerContext ctx, T msg, ByteBuf out) throws Exception {
// Protocol encoding
    }
}
```

## Protocol adapter {#section_xlg_2nh_p2b .section}

To reduce cost and improve the efficiency of development, the general protocol server SDK provides protocol adapters with extensible and customizable basic capability class com.aliyun.iot.as.bridge.server.channel.CustomizedUdpBasedProtocolHandler. It encapsulates details to access Alibaba Cloud IoT Platform, so you can focus on other business. The protocol adapter derives from this class.

**Device Online**

Only online devices can establish a connection with or send connection requests to IoT Platform.  There are two steps for devices to get online: local session initialization and device authentication.

1.  Session Initialization

    Refer to [Core SDK develop \> Device Online \> Session Initialization](intl.en-US/Developer Guide (Devices)/General protocols/Develop Core SDK.md#section_vy2_5s1_p2b) for details.

2.  Device Authentication

    After local session initialization, call doOnline\(Session newSession, String originalIdentity, String... credentials\) or doOnline\(String originalIdentity, String... credentials\) to complete local device authentication and Alibaba Cloud IoT Platform online authentication. The device can communicate with IoT Platform if authentication succeeds, and will be disconnect from IoT Platform if authentication fails.

    Example:

    ```
    Session session = Session.newInstance(device, channel);
    boolean success = doOnline(session, originalIdentity);
    if (success) {
        // Successfully got online, and will accept communication requests.
    } else {
        // Failed to get online, and will reject communication requests and disconnect (if connected).
    }
    ```


**Device Offline**

When the device is disconnected or detects that it needs to be disconnected, the device offline action should be initiated. Using server SDK, devices will automatically get offline when they are disconnected, so you can focus on other tasks. Call doOffline\(Session session\) to bring devices offline.

**Report Data**

The protocol adapter needs to use override channelRead\(ChannelHandlerContext ctx, Object msg\). It is the entrance for all devices to report data. Object msg is the data output from the decoder.

There are three steps for data reporting: identify the device that is going to report data, find the corresponding session for this device, and then report data to IoT Platform. Use the following interfaces to report data:

-   CompletableFuture doPublishAsync\(String originalIdentity, String topic, byte\[\] payload, int qos\): send data asynchronously and return immediately. Developers obtain the sending result using future.
-   CompletableFuture doPublishAsync\(String originalIdentity, ProtocolMessage protocolMsg\): send data asynchronously and return immediately. Developers obtain the sending result using future.
-   boolean doPublish\(String originalIdentity, ProtocolMessage protocolMsg, int timeout\): send data asynchronously and wait for the response.
-   boolean doPublish\(String originalIdentity, String topic, byte\[\] payload, int qos, int timeout\): send data asynchronously and wait for the response.

Example:

```
DeviceIdentity identity = ConfigFactory.getDeviceConfigManager().getDeviviceIdentity(device);
if (identity == null) {
    // Devices are not mapped with those registered on IoT Platform. Messages are dropped.
    return;
}
Session session = SessionManagerFactory.getInstance().getSession(device);
if (session == null) { 
    // The device is not online. Please get online or drop messages.  Make sure devices are online before reporting data to IoT Platform.
}
boolean success = doPublish(session, topic, payload, 0, 10);
if(success) {
    // Data is successfully reported to Alibaba Cloud IoT Platform.
} else {
    // Failed to report data to IoT Platform
}    
```

**Downstream Messaging**

Not supported yet.

