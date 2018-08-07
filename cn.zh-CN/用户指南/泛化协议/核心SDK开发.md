# 核心SDK开发 {#concept_jly_vcm_42b .concept}

基于泛化协议核心SDK，您可以将物联网平台桥接服务与已有的接入服务或平台进行高效整合，使设备或者服务得以快速接入阿里云物联网平台。

## 准备工作 {#section_s2z_tjs_q2b .section}

泛化协议核心SDK概念、功能及Maven依赖，请参考[概览](cn.zh-CN/用户指南/泛化协议/概览.md#)。

## 配置管理 {#section_fy2_5s1_p2b .section}

泛化协议核心SDK默认使用基于文件的配置管理。自定义配置请参见[自定义组件\>配置管理](#section_vz2_5s1_p2b)。

-   支持格式包括Java Proterties、JSON以及可读性较好的[HOCON](https://github.com/lightbend/config/blob/master/HOCON.md)（JSON超集）。
-   支持结构化配置项以提升可维护性。
-   可以通过Java系统属性覆盖文件配置，如java -Dmyapp.foo.bar=10。
-   支持配置文件分离和嵌套引用。

**application.conf**

|名称|描述|是否必须|
|--|--|----|
|productKey|网桥所属产品的ProductKey|是|
|deviceName|网桥对应的DeviceName，默认值为ECS MAC地址|否|
|deviceSecret|网关对应的DeviceSecret|否|
|http2Endpoint|HTTP2网关服务地址|是|
|authEndpoint|设备认证服务地址|是|
|popClientProfile|Open API调用客户端相关配置，参见[API Client配置](#table_my2_5s1_p2b)。|是|

**API Client配置**

|名称|描述|是否必须|
|--|--|----|
|accessKey|Open API调用用户身份标识|是|
|accessSecret|Open API调用用户身份标识对应的秘钥|是|
|name|API客户端profile名称|是|
|region|调用的服务端所在地域|是|
|product|产品名称，除非特殊情况否则请配置为 *Iot* |是|
|endpoint|调用的服务端地址|是|

**devices.conf**

配置设备在阿里云物联网平台的映射，即配置设备三元组。自定义配置文件请参见[自定义组件\>配置管理](#section_vz2_5s1_p2b)。

```
XXXX // 表示设备的原始标识
{
    productKey: "",
    deviceName: "",
    deviceSecret: ""
}

```

## 开发接口 {#section_sy2_5s1_p2b .section}

**初始化**

com.aliyun.iot.as.bridge.core.BridgeBootstrap负责初始化设备到阿里云物联网平台的通信。创建新的BridgeBootstrap实例后，网关[基本配置](#section_fy2_5s1_p2b)的管理组件会被初始化。使用自定义的配置管理，请参见[自定义组件\>配置管理](#section_uz2_5s1_p2b)。

请通过以下接口完成初始化。

-   bootstrap\(\): 不实现消息下行功能并进行初始化。
-   bootstrap\(DownlinkChannelHandler handler\): 采用开发者指定的DownlinkChannelHandler进行初始化。

示例代码：

```
BridgeBootstrap bootstrap = new BridgeBootstrap();
// 不实现下行通讯
bootstrap.bootstrap();
```

**设备上线**

当设备建立连接或者发送连接请求时，应发起设备上线操作，只有设备上线后，才能与阿里云物联网平台通讯。设备上线分为两个部分：本地Session的初始化以及设备认证。

1.  Session初始化

    泛化协议SDK提供非持久化的本地Session管理功能，关于自定义Session管理，请参见[自定义组件\>Session管理](#section_uz2_5s1_p2b)。

    创建接口：

    -   com.aliyun.iot.as.bridge.core.model.Session.newInstance\(String originalIdentity, Object channel\)
    -   com.aliyun.iot.as.bridge.core.model.Session.newInstance\(String originalIdentity, Object channel, int heartBeatInterval\)
    -   com.aliyun.iot.as.bridge.core.model.Session.newInstance\(String originalIdentity, Object channel, int heartBeatInterval, int heartBeatProbes\)
    其中originalIdentity表示原始协议中设备的唯一标识，如序列号、编号等；channel为设备连接至桥接服务的通信通道如Netty的Channel；heartBeatInterval与heartBeatProbes用于自动的心跳过期监测，分别代表心跳的间隔时间（以秒为单位）以及允许的最大心跳丢失次数，超过该次数后将发送心跳超时事件，开发者可通过注册com.aliyun.iot.as.bridge.core.session.SessionListener来处理心跳超时事件。

2.  设备认证

    设备本地Session初始化完成后请调用com.aliyun.iot.as.bridge.core.handler.UplinkChannelHandler.doOnline\(Session newSession, String originalIdentity, String... credentials\)完成本地设备认证和阿里云物联网平台上线认证，并根据结果允许设备后续通信或者断开连接。默认无本地认证，到阿里云物联网平台上线认证由SDK提供支持，如需使用自定义本地认证，请参见[自定义组件\>连接认证](#section_tz2_5s1_p2b)。

    示例代码：

    ```
    UplinkChannelHandler uplinkHandler = new UplinkChannelHandler();
    Session session = Session.newInstance(device, channel);
    boolean success = uplinkHandler.doOnline(session, originalIdentity);
    if (success) {
        // 上线成功，接受后续通信请求
    } else {
        // 上线失败，拒绝后续通信请求，断开连接（如有）
    }
    ```


**设备下线**

当设备连接断开或者探测到需要断开连接的时候，应发起设备下线操作，通过接口com.aliyun.iot.as.bridge.core.handler.UplinkChannelHandler.doOffline\(String originalIdentity\)实现设备下线。

示例代码：

```
UplinkChannelHandler uplinkHandler = new UplinkChannelHandler();
uplinkHandler.doOffline(originalIdentity);
```

**上报数据**

开发者可通过com.aliyun.iot.as.bridge.core.handler.UplinkChannelHandler上报数据到阿里云物联网平台。操作包括以下三步：获取上行数据关联的设备、找到设备关联的Session、上报数据到阿里云物联网平台。请通过以下接口完成数据上报。

**说明：** 请开发者务必注意控制是否上报数据以及安全相关前置处理 。

-   CompletableFuture doPublishAsync\(String originalIdentity, String topic, byte\[\] payload, int qos\)：异步发送数据并立即返回，开发者需根据future判断发送结果。
-   CompletableFuture doPublishAsync\(String originalIdentity, ProtocolMessage protocolMsg\)：异步发送数据并立即返回，开发者需根据future判断发送结果。
-   boolean doPublish\(String originalIdentity, ProtocolMessage protocolMsg, int timeout\)：异步发送数据并等待发送结果返回。
-   boolean doPublish\(String originalIdentity, String topic, byte\[\] payload, int qos, int timeout\)：异步发送数据并等待发送结果返回。

示例代码：

```
UplinkChannelHandler uplinkHandler = new UplinkChannelHandler(); 
DeviceIdentity identity = ConfigFactory.getDeviceConfigManager().getDeviviceIdentity(device); 
if (identity == null) { 
    // 设备未映射到阿里云物联网平台上的设备，丢弃消息 
    return; 
} 
Session session = SessionManagerFactory.getInstance().getSession(device); 
if (session == null) { 
    // 设备尚未上线，上报数据到阿里云物联网平台前请务必确保设备已上线，请上线设备或者丢弃消息 
} 
boolean success = uplinkHandler.doPublish(session, topic, payload, 0, 10); 
if(success) { 
    // 上报数据到阿里云物联网平台成功 
} else { 
    // 上报数据到阿里云物联网平台失败
}   
```

**下行消息**

泛化协议SDK提供了com.aliyun.iot.as.bridge.core.handler.DownlinkChannelHandler作为下行到设备的数据分发处理器，支持单播和广播\(如果云端下发的数据不包含特定设备信息则为广播\)。

示例代码：

```
public class SampleDownlinkHandler implements DownlinkChannelHandler {
    @Override
    public boolean pushToDevice(Session session, String topic, byte[] payload) {
        // 处理设备消息推送
    }

    @Override
    public boolean broadcast(String topic, byte[] payload) {
        // 处理广播
    }
}

```

## 自定义组件 {#section_sz2_5s1_p2b .section}

开发者可以自定义设备连接认证、Session管理以及配置管理组件，如果期望使用自定义的组件，请务必在调用BridgeBootstrap初始化之前完成这些组件的初始化和替换。

**连接认证**

自定义设备连接认证需实现接口com.aliyun.iot.as.bridge.core.auth.AuthProvider，并在BridgeBootstrap初始化前调用com.aliyun.iot.as.bridge.core.auth.AuthProviderFactory.init\(AuthProvider customizedProvider\)将认证组件替换为自定义实现。

**Session管理**

自定义Session管理需实现接口com.aliyun.iot.as.bridge.core.session.SessionManager，并在BridgeBootstrap初始化前调用com.aliyun.iot.as.bridge.core.session.SessionManagerFactory.init\(SessionManager<?\> customizedSessionManager\)将Session组件替换为自定义实现。

**配置管理**

自定义配置管理需实现接口com.aliyun.iot.as.bridge.core.config.DeviceConfigManager和com.aliyun.iot.as.bridge.config.BridgeConfigManager，并在BridgeBootstrap初始化前调用 com.aliyun.iot.as.bridge.core.config.ConfigFactory.init\(BridgeConfigManager bcm, DeviceConfigManager dcm\)将配置管理组件替换为自定义实现，其中参数均可为空，如果为空则使用泛化协议SDK默认实现。

