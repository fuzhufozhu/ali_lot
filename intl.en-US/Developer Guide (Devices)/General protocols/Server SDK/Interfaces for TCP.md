# Interfaces for TCP {#concept_qth_4hd_p2b .concept}

You can build an access service which uses TCP transmission protocol and bridge it to Alibaba Cloud IoT Platform using the interfaces of the general protocol SDK for TCP.

## Bootstrap {#section_slg_2nh_p2b .section}

com.aliyun.iot.as.bridge.server.BridgeServerBootstrap is the bootstrap class for booting socket server and bridge service. After a new BridgeServerBootstrap is created, components based on configuration files will be initialized.

Example:

```
BridgeServerBootstrap bootstrap = new BridgeServerBootstrap(new TcpDecoderFactory() {
    @Override
    public ByteToMessageDecoder newInstance() {
        // Return decoder
    }
}, new TcpEncoderFactory() {
    @Override
    public MessageToByteEncoder<? > newInstance() {
// Return encoder
    }
}, new TcpBasedProtocolAdaptorHandlerFactory() {
@ Override
    public CustomizedTcpBasedProtocolHandler newInstance() {
// Return protocol adapter
    }
}, new DefaultDownlinkChannelHandler());
try {
    bootstrap.start();
} catch (BootException | ConfigException e) {
// Process boot exception
}

```

**Instantiation of TCP type BridgeServerBootstrap**

-   com.aliyun.iot.as.bridge.server.channel.factory.TcpDecoderFactory: Create a new decoder instance using factory method to decode upload data. Thread is secure. Can be null.
-   com.aliyun.iot.as.bridge.server.channel.factory.TcpEncoderFactory: Create a new encoder instance using factory method to encode downstream data to adapt to TCP protocol. Thread is secure. Can be null.
-   com.aliyun.iot.as.bridge.server.channel.factory.TcpBasedProtocolAdaptorHandlerFactory: Create a new protocol adapter instance using factory method to adapt decoded data so they can be uploaded to the cloud. Thread is secure. Cannot be null.
-   com.aliyun.iot.as.bridge.core.handler.DownlinkChannelHandler: Distributor for downstream data. Supports unicast and broadcast. Unicast forwards data directly to the device by default. Broadcast settings must be customized by developers. Can be null. Null indicates that downstream data is not allowed.

**Start socket server**

After the creation of BridgeServerBootstrap, call com.aliyun.iot.as.bridge.server.BridgeServerBootstrap.start\(\) to start the socket server.

## Protocol decoding {#section_vlg_2nh_p2b .section}

The component for protocol decoding derives from io.netty.handler.codec.ByteToMessageDecoder. Refer to [ByteToMessageDecoder Documentation](http://netty.io/4.1/api/io/netty/handler/codec/ByteToMessageDecoder.html) for details.

Example:

```
public class SampleDecoder extends ByteToMessageDecoder {
    @Override
    protected void decode(ChannelHandlerContext ctx, ByteBuf in, List<Object> out) throws Exception {
// The decoding protocol
    }
}
```

## Protocol encoding {#section_wlg_2nh_p2b .section}

The component for protocol encoding derives from io.netty.handler.codec.MessageToByteEncoder<I\>. Refer to [MessageToByteEncoder Documentation](http://netty.io/4.1/api/io/netty/handler/codec/MessageToByteEncoder.html) for details.

Example:

```
public class SampleEncoder extends MessageToByteEncoder<String>{
    @Override
    protected void encode(ChannelHandlerContext ctx, String msg, ByteBuf out) throws Exception {
// Protocol encoding
    }
}
```

## Protocol adapter {#section_xlg_2nh_p2b .section}

To reduce cost and improve the efficiency of development, the general protocol server SDK provides protocol adapters with extensible and customizable basic capability class com.aliyun.iot.as.bridge.server.channel.CustomizedTcpBasedProtocolHandler. It encapsulates details to access Alibaba Cloud IoT Platform, so you can focus on protocol related business. The protocol adapter derives from this class.

**Device Online**

Only online devices can establish a connection with or send connection requests to IoT Platform. There are two steps for devices to get online: local session initialization and device authentication.

1.  Session Initialization

    Refer to [Core SDK develop \> Device Online \> Session Initialization](reseller.en-US/Developer Guide (Devices)/General protocols/Core SDK develop.md#section_vy2_5s1_p2b)

2.  Device Authentication

    After local session initialization, call doOnline\(ChannelHandlerContext ctx, Session newSession, String originalIdentity, String... credentials\) to complete local device authentication and Alibaba Cloud IoT Platform online authentication. The device can communicate with IoT Platform if authentication succeeds, and will be disconnect from IoT Platform if authentication fails.

    Example:

    ```
    Session session = Session.newInstance(device, channel);
    boolean success = doOnline(session, originalIdentity);
    if (success) {
        // Successfully got online, and will accept communication requests.
    } else {
        // Failed to get online, will reject communication requests and disconnect (if connected).
    }
    ```


**Device Offline**

When the device is disconnected or detects that it needs to be disconnected, the device offline action should be initiated. Using server SDK, devices will automatically get offline when they are disconnected, so you can focus on other tasks. Call doOffline\(Session session\) to bring devices offline.

**Report Data**

The protocol adapter needs to use override channelRead\(ChannelHandlerContext ctx, Object msg\). It is the entrance for all devices to report data. Object msg is the data output from the decoder.

There are three steps for data reporting: identify the device that is going to report data, find the corresponding session for this device, and then report data to IoT Platform. Use the following interfaces to report data:

-   CompletableFuture doPublishAsync\(Session session, String topic, byte\[\] payload, int qos\): send data asynchronously and return immediately. Developers obtain the sending result using future.
-   CompletableFuture doPublishAsync\(Session session, ProtocolMessage protocolMsg\): send data asynchronously and return immediately. Developers obtain the sending result using future.
-   boolean doPublish\(Session session, ProtocolMessage protocolMsg, int timeout\): send data asynchronously and wait for the response.
-   boolean doPublish\(Session session, String topic, byte\[\] payload, int qos, int timeout\): send data asynchronously and wait for the response.

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

Refer to [Core SDK development \> Downstream Messaging](reseller.en-US/Developer Guide (Devices)/General protocols/Core SDK develop.md#) for details.

The SDK provides com.aliyun.iot.as.bridge.core.handler.DefaultDownlinkChannelHandler as the downstream data distributor. It supports unicast and broadcast. Unicast forwards data from the cloud directly to the device by default, and broadcast requires developers to customize specific implementations. Customization can be realized by deriving subclass.

Example:

```
import io.netty.channel.Channel;
import Io. netty. Channel. channelfuture;
...

public class SampleDownlinkChannelHandler implements DownlinkChannelHandler {
    @Override
    public boolean pushToDevice(Session session, String topic, byte[] payload) {
        // Obtain communication channel from device's corresponding session.
        Channel channel = (Channel) session.getChannel().get();
        if (channel ! = null && channel.isWritable()) {
            String body = new String(payload, StandardCharsets.UTF_8);
            // Send downstream data to devices
            ChannelFuture future = channel.pipeline().writeAndFlush(body);
            future.addListener(ChannelFutureListener.FIRE_EXCEPTION_ON_FAILURE);
            return true;
        }
        return false;
    }

    @Override
    public boolean broadcast(String topic, byte[] payload) {
        throw new RuntimeException("not implemented");
    }
}
```

