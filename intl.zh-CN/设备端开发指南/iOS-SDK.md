# iOS-SDK {#concept_nwy_yjm_b2b .concept}

本章节介绍如何使用iOS-SDK，将iOS手机接入物联网平台。

本章节中SDK 采用 cocoapods 管理依赖，建议采用 1.1.1 以上版本。

iOS SDK封装了 MQTT 建连、长连接维护和基于MQTT协议的上下行请求的功能。

## 快速接入 {#section_jgt_ckm_b2b .section}

1.  在xcode工程Podfile中添加以下行，集成SDK。

    ```
    
    source 'https://github.com/CocoaPods/Specs.git'
    source 'https://github.com/aliyun/aliyun-specs.git'
    target "necslinkdemo" do
    pod 'IMLChannelCore', '1.1.1'
    end
    ```

2.  登录阿里云物联网平台的控制台，选择设备管理，单击设备后的**查看**，获取需要的三元组信息，即ProductKey、DeviceName及DeviceSecret。
3.  SDK API开发，具体请参见下文各SDK实现说明。

## SDK 初始化 {#section_iwb_cxv_wdb .section}

初始化时，使用三元组信息跟物联网平台建立可信安全的长连接通道，配置自己的server地址以及端口。

```
#import 
#import 
LKIoTConnectConfig * config = [LKIoTConnectConfig new];
config.productKey = @"your product key";
config.deviceName = @"your device name";
config.deviceSecret = @"your device secret";
config.server = @"www.youserver.com";//设为nil表示使用IoT平台作为连接服务器
config.port = 1883,//your server port。如果server被设置为nil。则port也不要设置。
config.receiveOfflineMsg = NO;//如果希望收到客户端离线时的消息，可以设为YES.
[[LKIoTExpress sharedInstance]startConnect:config connectListener:self];
//如果config.server置为nil，则默认连接的地址为上海节点：${yourProductKey}.iot-as-mqtt.cn-shanghai.aliyuncs.com:1883`
```

## 上行请求接口 {#section_kjb_gxv_wdb .section}

SDK 封装了上行Publish请求、订阅Subscribe和取消订阅unSubscribe等接口。

上行请求需要在 SDK 初始化建联成功之后才能正常调用。

```
/**
RPC请求接口，封装了业务的上行request以及下行resp。request业务报文由SDK内部按alink标准协议封装，形如
{
"id":"msgId" //消息id
"system": {
"version": "1.0", // 必填，消息版本，目前为1.0
"time": "" // 必填，消息发生的时间，毫秒,
},
"request": {
},
"params": {
}
}
@param topic rpc请求的topic，由具体业务确定，完整topic形如：
/sys/${YourProductKey}/${YourDeviceName}/app/abc/cba
@param opts 可选配置项，可空，预留
示例，{"extraParam":{"method":"thing.topo.add"}}
则会将"method":"thing.topo.add"塞到最终的业务报文里,跟业务报文中的params平级。
@param params 业务参数。
@param responseHandler 业务服务器响应回调接口，参见LKExpressResponse
*/
-(void)invokeWithTopic:(NSString *)topic opts:(NSDictionary* _Nullable)opts
params:(NSDictionary*)params respHandler:(LKExpressResponseHandler)responseHandler;
/**
上行数据，直接透传，不会再按alink业务报文协议封装
@param topic 消息topic
@param dat 需透传的数据
@param completeCallback 数据上行结果回调
*/
-(void)uploadData:(NSString *)topic data:(NSData *)dat complete:(LKExpressOnUpstreamResult)completeCallback;
/**
上行数据，不会有回执。SDK会按alink标准协议封装业务报文。
@param topic 消息topic,完整的topic
@param params 业务参数
@param completeCallback 数据上行结果回调
*/
-(void)publish:(NSString *) topic params:(NSDictionary *)params complete:(LKExpressOnUpstreamResult)completeCallback;
/**
订阅topic的接口
@param topic 订阅的消息的topic，由具体业务确定，需要传完整的topic区段,形如：
/sys/${YourProductKey}/${YourDeviceName}/app/abc/cba
@param completionHandler 订阅流程结束的callback，如果error为空表示订阅成功，否则订阅失败
*/
- (void)subscribe:(NSString *)topic complete: (void (^)(NSError * _Nullable error))completionHandler;
/**
取消订阅topic的接口
@param topic 订阅的消息的topic，由具体业务确定，需要传完整的topic区段,形如：
/sys/${YourProductKey}/${YourDeviceName}/app/abc/cba
@param completionHandler 取消订阅流程结束的callback，如果error为空表示订阅成功，否则订阅失败
*/
- (void)unsubscribe : (NSString *)topic complete: (void (^)(NSError * _Nullable error))completionHandler;
```

三个上行方法的区别如下：

-   `-(void)invokeWithTopic:(NSString )topic opts:(NSDictionary _Nullable)opts respHandler:(LKExpressResponseHandler)responseHandler;`：这个是业务请求响应模型，发一个请求回去，服务端的响应会在responseHandler回调中抛回来。但是此API对 topic有要求。比如你 发送一个topic为 /$\{productKey\}/$\{deviceName\}/req 的请求。则对方在处理完业务逻辑后必须回复一个 /$\{productKey\}/$\{deviceName\}/req\_reply。否则会出现响应超时的错误。
-   `uploadData:(NSString )topic data:(NSData )dat complete:(LKExpressOnUpstreamResult)completeCallback;`：这个是数据透传接口，不进行任何处理，直接上行到云端，也不会有响应回来。
-   `-(void)publish:(NSString *\) topic params:\(NSDictionary*)params complete:(LKExpressOnUpstreamResult)completeCallback;`：数据上行时会按alink业务报文协议封装后再上行。alink业务报文协议在api reference里有较详细的说明。

业务请求响应模型调用示例：

```
NSString *topic = @"/sys/${YourProductKey}/${YourDeviceName}/account/bind";
NSDictionary *params = @{
@"iotToken": token,
};
[[LKIoTExpress sharedInstance] invokeWithTopic:topic
opts:nil
params:params
respHandler:^(LKExpressResponseHandler * _Nonnull response) {
if (![response successed]) {
NSLog(@"业务请求失败");
}
}];
//其中${YourProductKey}为三元组信息之ProductKey
//其中${YourDeviceName}为三元组信息之DeviceName
```

订阅topic调用示例：

```
NSString *topic = @"/sys/${YourProductKey}/${YourDeviceName}/app/down/event";
[[LKIoTExpress sharedInstance] subscribe:topic complete: ^(NSError * error) {
if (error != nil) {
NSLog(@"业务请求失败");
}
}];
```

## 下行数据监听 {#section_elb_wxv_wdb .section}

订阅Topic后，云端推送的下行消息监听。

```
@protocol LKExpressDownListener<NSObject>
-(void)onDownstream:(NSString *) topic data: (id _Nullable) data;///<topic-消息topic，data-消息内容,NSString 或者 NSDictionary

-(BOOL)shouldHandle:(NSString *)topic;///<数据使用onDownstream:data:上抛时，可以先过滤一遍，如返回NO，则不上传，返回YES，则会使用onDownstream:data:上抛
@end

[[LKIoTExpress sharedInstance] addDownstreamListener:LKExpressDownListenerTypeGw listener:(id<LKExpressDownListener>)downListener];
//downListener在本SDK中 是 weak reference，所以需要调用者保证生命周期。
```

## 设计建议 {#section_n4p_zl3_b2b .section}

建议通道建连时，所使用的三元组跟C端用户账号解耦。即应用只使用一个三元组去物联网平台建立连接。

C端用户账号可以任意切换，切换账号时，只需要变更C端账号跟三元组的绑定关系即可。

最终的形态是C端账号A可以使用该通道，切到另一个C端账号B，重新绑定三元组关系后，也可以使用该通道，即C端账号跟通道的绑定关系可以动态变更。

