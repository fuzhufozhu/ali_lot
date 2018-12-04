# UDP Server开发接口 {#concept_r33_rhd_p2b .concept}

基于泛化协议SDK的UDP Server开发接口，您可以快速构建以UDP为传输协议的接入服务，并将其与阿里云物联网平台桥接。

## 启动引导 {#section_slg_2nh_p2b .section}

com.aliyun.iot.as.bridge.server.BridgeServerBootstrap是启动Socket服务器和桥接服务的引导类。创建新的BridgeServerBootstrap实例后，会初始化基于配置文件的基本配置组件。

示例代码：

```
BridgeServerBootstrap bootstrap = new BridgeServerBootstrap(new UdpDecoderFactory() {
    @Override
    public MessageToMessageDecoder newInstance() {
        // 返回解码器
    }
}, new UdpEncoderFactory() {
    @Override
    public MessageToMessageEncoder<?> newInstance() {
        // 返回编码器
    }
}, new UdpBasedProtocolAdaptorHandlerFactory() {
    @Override
    public CustomizedUdpBasedProtocolHandler newInstance() {
        // 返回协议适配器
    }
});
try {
    bootstrap.start();
} catch (BootException | ConfigException e) {
    // 处理启动失败
}

```

**实例化UDP类型BridgeServerBootstrap**

-   com.aliyun.iot.as.bridge.server.channel.factory.UdpDecoderFactory: 协议解码器工厂类具体实现，负责如何创建一个新的解码器实例，用于上行数据的解析，线程安全，可为null。
-   com.aliyun.iot.as.bridge.server.channel.factory.UdpEncoderFactory: 协议编码器工厂类具体实现，负责如何创建一个新的编码器实例，用于下行数据到协议编码的转换，线程安全，可为null。
-   com.aliyun.iot.as.bridge.server.channel.factory.UdpBasedProtocolAdaptorHandlerFactory: 协议适配器工厂类具体实现，负责如何创建一个新的协议适配器实例，用于解码器解析出来的数据到云端数据的转换，线程安全，不可为null。

**启动Socket Server**

创建BridgeServerBootstrap完成后，需调用com.aliyun.iot.as.bridge.server.BridgeServerBootstrap.start\(\)完成Socket Server启动。

## 协议解码 {#section_vlg_2nh_p2b .section}

协议解码组件需派生自io.netty.handler.codec.MessageToMessageDecoder<I\>，具体内容请参考[MessageToMessageDecoder文档](http://netty.io/4.1/api/index.html?io/netty/handler/codec/MessageToMessageDecoder.html)。

示例代码：

```
public class SampleDecoder extends  MessageToMessageDecoder<DatagramPacket> {
    @Override
    protected void decode(ChannelHandlerContext ctx, DatagramPacket in, List<Object> out) throws Exception {
        // 解码协议
    }
}
```

## 协议编码 {#section_wlg_2nh_p2b .section}

协议编码组件需派生自io.netty.handler.codec.MessageToMessageEncoder<I\>，具体内容请参考[MessageToMessageEncoder文档](http://netty.io/4.1/api/index.html?io/netty/handler/codec/MessageToMessageEncoder.html)。

示例代码：

```
public class SampleEncoder extends MessageToMessageEncoder<T>{
    @Override
    protected void encode(ChannelHandlerContext ctx, T msg, ByteBuf out) throws Exception {
        // 协议编码
    }
}
```

## 协议适配器 {#section_xlg_2nh_p2b .section}

为了降低开发成本，提高泛化协议设备接入与桥接服务的开发效率，Server SDK为协议适配器提供了可扩展、可定制的基础能力类com.aliyun.iot.as.bridge.server.channel.CustomizedUdpBasedProtocolHandler。其封装了与阿里云物联网平台的对接细节，使您可以专注于协议本身的业务，无需关注具体的对接细节。泛化协议的协议处理适配器需派生自该类。

**设备上线**

当设备建立连接或发送连接请求时，应触发设备上线操作。设备上线分为两个部分：网桥本地Session的初始化以及设备认证。

1.  Session初始化

    请参见[核心SDK\>设备上线\>Session初始化](intl.zh-CN/用户指南/泛化协议/核心SDK开发.md#section_vy2_5s1_p2b)。

2.  设备认证

    本地Session初始化完成后即可调用doOnline\(Session newSession, String originalIdentity, String... credentials\)或doOnline\(String originalIdentity, String... credentials\)完成本地设备认证和阿里云物联网平台上线认证。认证成功，设备可以进行通信；认证失败，设备将断开连接。

    示例代码：

    ```
    Session session = Session.newInstance(device, channel);
    boolean success = doOnline(session, originalIdentity);
    if (success) {
        // 上线成功，接受后续通信请求
    } else {
        // 上线失败，拒绝后续通信请求，断开连接（如有）
    }
    ```


**设备下线**

当设备连接断开或者网桥探测到需要断开连接时，应触发设备下线操作。泛化协议Server SDK自身提供了设备连接断开时自动下线设备的能力，开发者只需负责处理主动触发设备下线操作的情形即可，下线某个设备可通过接口 doOffline\(Session session\)实现。

**上报数据**

协议适配器需override channelRead\(ChannelHandlerContext ctx, Object msg\)方法，该方法是所有设备上行数据的入口，msg为decoder所返回的数据。

上报数据到物联网平台包括三步：获取上行数据关联的设备、找到设备关联的Session、上报数据到物联网平台。需通过以下接口完成数据上报：

-   CompletableFuture doPublishAsync\(String originalIdentity, String topic, byte\[\] payload, int qos\)：异步发送数据并立即返回，开发者需根据future判断发送结果。
-   CompletableFuture doPublishAsync\(String originalIdentity, ProtocolMessage protocolMsg\)：异步发送数据并立即返回，开发者需根据future判断发送结果。
-   boolean doPublish\(String originalIdentity, ProtocolMessage protocolMsg, int timeout\)：异步发送数据并等待发送结果返回。
-   boolean doPublish\(String originalIdentity, String topic, byte\[\] payload, int qos, int timeout\)：异步发送数据并等待发送结果返回。

示例代码：

```
DeviceIdentity identity = ConfigFactory.getDeviceConfigManager().getDeviviceIdentity(device);
if (identity == null) {
    // 设备未成功映射到阿里云物联网平台上的设备，丢弃消息
    return;
}
Session session = SessionManagerFactory.getInstance().getSession(device);
if (session == null) {
    // 设备尚未上线，请上线设备或者丢弃消息。上报数据到物联网平台前请务必确保设备已上线。
}
boolean success = doPublish(session, topic, payload, 0, 10);
if(success) {
    // 成功上报数据到阿里云物联网平台
} else {
    // 上报数据到阿里云物联网平台失败
}    
```

**下行消息**

暂不支持

