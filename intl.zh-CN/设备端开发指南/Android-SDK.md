# Android-SDK {#concept_e5x_b3l_z2b .concept}

您可以使用物联网平台提供的Android SDK，搭建设备与云端的双向数据通道。SDK包含设备动态注册、初始化建联和数据上下行的接口。

**说明：** Android SDK于2018年9月1日发布新版。旧版用户可参考[旧版文档](https://help.aliyun.com/document_detail/89740.html)。

## 工程配置 {#section_sfs_xjr_z2b .section}

1.  在Android工程根目录下的build.gradle 基础配置文件中，加入阿里云仓库地址，进行仓库配置。

    ```
    allprojects { 
       repositories { 
         jcenter()
         // 阿里云仓库地址
         maven { 
                url "http://maven.aliyun.com/nexus/content/repositories/releases/" 
            } 
         maven {
                url "http://maven.aliyun.com/nexus/content/repositories/snapshots"
            }
        }
    }
    ```

2.  在模块的 build.gradle 中，添加SDK的依赖，引入 SDK :iot-linkkit。

    ```
    compile('com.aliyun.alink.linksdk:iot-linkkit:1.3.0.1')
    ```


## 初始化 {#section_ogl_kkr_z2b .section}

参考如下代码初始化设备信息，建立设备与云端的连接。

设备初始化参数包括：

-   设备证书信息
    -   若使用[一机一密](intl.zh-CN/设备端开发指南/C-SDK/设备安全认证/一机一密.md#)认证方式，需要有ProductKey、DeviceName和DeviceSecret。
    -   若使用[一型一密](intl.zh-CN/设备端开发指南/C-SDK/设备安全认证/一型一密.md#)认证方式，需要有ProductKey、ProductSecret和DeviceName，并在控制台开启动态注册。
-   云端接口请求域名

    请参考[地域和可用区](https://help.aliyun.com/document_detail/40654.html)查看支持的域名。

-   设备属性的初始值

    仅在使用[物模型](../../../../intl.zh-CN/用户指南/产品与设备/物模型/什么是物模型.md#)时，才需设置此项。


```
DeviceInfo deviceInfo = new DeviceInfo();
deviceInfo.productKey = productKey; // 三元组 产品型号（必填）
deviceInfo.deviceName = deviceName; // 三元组 设备标识 （必填）
deviceInfo.deviceSecret = deviceSecret; // 三元组 设备密钥 （一机一密必填）
deviceInfo.productSecret = productSecret; // 产品密钥（一型一密必填）

// 请求域名
IoTApiClientConfig userData = new IoTApiClientConfig();
// userData.domain 默认值是 "iot.cn-shanghai.aliyuncs.com"
// 如果需要修改请按如下方式设置
// userData.domain = "xxxx";

// 设备属性初始值
Map<String, ValueWrapper> propertyValues = new HashMap<>();
// TODO 开发者需根据实际产品从设备获取属性值，如果不使用物模型无需设置属性初始值
//propertyValues.put("LightSwitch", new ValueWrapper.BooleanValueWrapper(0));

LinkKitInitParams params = new LinkKitInitParams();
params.deviceInfo = deviceInfo;
params.propertyValues = propertyValues;
params.connectConfig = userData;

// 如果不设置建联之后会从云端更新最新的TSL
// 如果主动设置TSL，需确保TSL和线上完全一致，且功能定义与云端一致
// params.tsl = "{xxx}"; // 不建议用户设置，直接依赖SDK从云端更新最新的TSL即可

//  #######  一型一密动态注册接口开始 ######
// 如果是一型一密的设备，需要先调用动态注册接口；一机一密设备不需要执行此流程
HubApiRequest hubApiRequest = new HubApiRequest();
hubApiRequest.path = "/auth/register/device";
LinkKit.getInstance().deviceRegister(context, params, hubApiRequest, new IConnectSendListener() {
    @Override
    public void onResponse(ARequest aRequest, AResponse aResponse) {
        // aRequest 用户的请求数据
        if (aResponse != null && aResponse.data != null) {
            ResponseModel<Map<String, String>> response = JSONObject.parseObject(aResponse.data.toString(),
                    new TypeReference<ResponseModel<Map<String, String>>>() {
                    }.getType());
            if ("200".equals(response.code) && response.data != null && response.data.containsKey("deviceSecret") &&
                    !TextUtils.isEmpty(response.data.get("deviceSecret"))) {
                deviceInfo.deviceSecret = response.data.get("deviceSecret");
                // getDeviceSecret success, to build connection.
                // 持久化 deviceSecret 初始化建联的时候需要
                // TODO  用户需要按照实际场景持久化设备的三元组信息，用于后续的连接
                // 成功获取 deviceSecret，调用初始化接口建联
                // TODO 调用设备初始化建联
            }
        }
    }

    @Override
    public void onFailure(ARequest aRequest, AError aError) {
        Log.d(TAG, "onFailure() called with: aRequest = [" + aRequest + "], aError = [" + aError + "]");
    }
});
//  ####### 一型一密动态注册接口结束  ######

// 设备初始化建联  如果是一型一密设备，需要在获取deviceSecret成功之后执行该操作。
LinkKit.getInstance().init(context, params, new ILinkKitConnectListener() {
    @Override
    public void onError(AError error) {
        // 初始化失败 error包含初始化错误信息 
    }

    @Override
    public void onInitDone(Object data) {
        // 初始化成功 data 作为预留参数
    }
});
```

**监听**

如果需要监听设备的上下线信息，云端下发的所有数据，可以设置以下监听器。

```
// 注册下行监听
// 包括长连接的状态
// 云端下行的数据
LinkKit.getInstance().registerOnPushListener(new IConnectNotifyListener() {
    @Override
    public void onNotify(String connectId, String topic, AMessage aMessage) {
        // 云端下行数据回调
        // connectId 连接类型 topic 下行 topic aMessage 下行数据
    }

    @Override
    public boolean shouldHandle(String connectId, String topic) {
        // 选择是否不处理某个 topic 的下行数据
        // 如果不处理某个topic，则onNotify不会收到对应topic的下行数据
        return true; //TODO 根基实际情况设置
    }

    @Override
    public void onConnectStateChange(String connectId, ConnectState connectState) {
        // 对应连接类型的连接状态变化回调，具体连接状态参考 SDK ConnectState
    }
});
```

**日志**

打开SDK内部日志输出开关：

```
ALog.setLevel(ALog.LEVEL_DEBUG);
```

**初始化结果**

初始化成功会收到 onInitDone 回调，失败会收到 onError 回调。DeviceInfo 结构体内容可参见 [DeviceInfo ApiReference](http://gaic.alicdn.com/ztms/android-iot-device-sdk-api-reference-v1/com/aliyun/alink/dm/api/DeviceInfo.html)。

## 物模型数据上下行通道 {#section_zjl_q4r_z2b .section}

设备可以使用[物模型](../../../../intl.zh-CN/用户指南/产品与设备/物模型/什么是物模型.md#)功能，实现属性上报（如上报设备状态）、事件上报（上报设备异常或错误）和服务调用（通过云端调用设备提供的服务）。

**说明：** getDeviceThing\(\)返回的 IThing 接口 介绍参见 [IThing ApiReference](http://gaic.alicdn.com/ztms/android-iot-device-sdk-api-reference-v1/com/aliyun/alink/dm/api/IThing.html)。

**设备属性上报**

```
// 设备上报
Map<String, ValueWrapper> reportData  = new HashMap<>();
// identifier 是云端定义的属性的唯一标识，valueWrapper是属性的值
// reportData.put(identifier, valueWrapper);  // 参考示例，更多使用可参考demo 
LinkKit.getInstance().getDeviceThing().thingPropertyPost(reportData, new IPublishResourceListener() {
    @Override
    public void onSuccess(String resID, Object o) {
        // 属性上报成功 resID 设备属性对应的唯一标识
    }

    @Override
    public void onError(String resId, AError aError) {
        // 属性上报失败
    }
});
```

**设备属性获取**

```
// 根据 identifier 获取当前物模型中该属性的值
String identifier = "xxx";
LinkKit.getInstance().getDeviceThing().getPropertyValue(identifier);

// 获取所有属性
LinkKit.getInstance().getDeviceThing().getProperties()
```

**设备事件上报**

```
HashMap<String, ValueWrapper> hashMap = new HashMap<>();
// TODO 用户根据实际情况设置
// hashMap.put("ErrorCode", new ValueWrapper.IntValueWrapper(0));
OutputParams params = new OutputParams(hashMap);
LinkKit.getInstance().getDeviceThing().thingEventPost(identifier, params, new IPublishResourceListener() {
    @Override
    public void onSuccess(String resId, Object o) { // 事件上报成功
    }

    @Override
    public void onError(String resId, AError aError) { // 事件上报失败
    }
});
```

**设备服务获取**

Service 定义参见 [Service API Reference](http://gaic.alicdn.com/ztms/android-iot-device-sdk-api-reference-v1/com/aliyun/alink/linksdk/tmp/devicemodel/Service.html)。

```
// 获取设备支持的所有服务
LinkKit.getInstance().getDeviceThing().getServices()
```

**设备服务调用监听**

云端在添加设备服务时，需设置该服务的调用方式，由Service中的callType字段表示。同步服务调用时`callType="sync"`；异步服务调用`callType="async"`。设备属性设置和获取也是通过服务调用监听方式实现云端服务的下发。

-   异步服务调用

    设备先注册服务的处理监听器，当云端触发异步服务调用的时候，下行的请求会到注册的监听器中。一个设备会有多种服务，通常需要注册所有服务的处理监听器。onProcess 是设备收到的云端下行的服务调用，第一个参数是需要调用服务对应的 identifier，用户可以根据 identifier （identifier 是云端在创建属性或事件或服务的时候的标识符，可以在云端产品的功能定义找到每个属性或事件或服务对应的identifier）做不同的处理。云端调用设置服务的时候，设备需要在收到设置指令后，调用设备执行真实操作，操作结束后上报一条属性状态变化的通知。

    ```
    // 用户可以根据实际情况注册自己需要的服务的监听器
    LinkKit.getInstance().getDeviceThing().setServiceHandler(service.getIdentifier(), mCommonHandler);
    // 服务处理的handler
    private ITResRequestHandler mCommonHandler = new ITResRequestHandler() {
        @Override
        public void onProcess(String identify, Object result, ITResResponseCallback itResResponseCallback) {
            // 收到云端异步服务调用  identify 设备端属性或服务唯一标识 result 下行服务调用数据 
            // iotResResponseCallback 用户处理完服务调用之后响应云端 具体使用参见 Demo 代码
        }
    
        @Override
        public void onSuccess(Object tag, OutputParams outputParams) {
            // 服务注册成功 tag：用户传入的tag，未使用到  outputParams：异步回调成功的返回数据,outparams等类型
        }
    
        @Override
        public void onFail(Object tag, ErrorInfo errorInfo) {
            // 服务注册失败
        }
    };
    ```

-   同步服务调用

    同步服务调用是一个 RRPC 调用。用户可以注册一个下行数据监听，当云端触发服务调用时，可以在 onNotify 收到云端的下行服务调用。收到云端的下行服务调用之后，用户根据实际服务调用对设备做服务处理，处理之后需要回复云端的请求，即发布一个带有处理结果的请求到云端。RRPC 回复的 topic 将请求的 request 替换成 response， msgId 保持不变。

    ```
    LinkKit.getInstance().registerOnPushListener(new IConnectNotifyListener() {
        @Override
        public void onNotify(String connectId, String topic, AMessage aMessage) {
            if ("LINK_PERSISTENT".equals(connectId) && !TextUtils.isEmpty(topic) &&
                    topic.startsWith("/sys/" + DemoApplication.productKey + "/" + DemoApplication.deviceName + "/rrpc/request")){
                // showToast("收到云端同步下行");
                MqttPublishRequest request = new MqttPublishRequest();
                request.isRPC = false;
                request.topic = topic.replace("request", "response");
                String resId = topic.substring(topic.indexOf("rrpc/request/")+13);
                request.msgId = resId;
                // TODO 用户根据实际情况填写 仅做参考
                request.payloadObj = "{\"id\":\"" + resId + "\", \"code\":\"200\"" + ",\"data\":{} }";
                LinkKit.getInstance().publish(request, new IConnectSendListener() {
                    @Override
                    public void onResponse(ARequest aRequest, AResponse aResponse) {
                        // 上报结果
                    }
    
                    @Override
                    public void onFailure(ARequest aRequest, AError aError) {
                        // 上报失败 aError：错误原因
                    }
                });
            }
        }
    
        @Override
        public boolean shouldHandle(String connectId, String topic) {
            return true;
        }
    
        @Override
        public void onConnectStateChange(String connectId, ConnectState connectState) {
    
        }
    })
    ```


## 基础数据上下行通道 {#section_gc4_wsr_z2b .section}

Android SDK 提供了与云端长链接的基础能力接口，用户可以直接使用这些接口完成自定义 Topic 相关的功能。提供的基础能力包括：发布、订阅、取消订阅、RRPC、订阅下行。如果不想使用物模型，可以通过这部分接口实现云端数据的上下行。

**上行接口请求**

调用上行请求接口，SDK封装了上行发布、订阅和取消订阅等接口。

```
/**
 * 发布
 *
 * @param request  发布请求
 * @param listener 监听器
 */
void publish(ARequest request, IConnectSendListener listener);

/**
 * 订阅
 *
 * @param request  订阅请求
 * @param listener 监听器
 */
void subscribe(ARequest request, IConnectSubscribeListener listener);

/**
 * 取消订阅
 *
 * @param request  取消订阅请求
 * @param listener 监听器
 */
void unsubscribe(ARequest request, IConnectUnscribeListener listener);
```

调用示例：

```
// 发布
MqttPublishRequest request = new MqttPublishRequest();
request.isRPC = false;
request.topic = topic;
request.payloadObj = data;
LinkKit.getInstance().publish(request, new IConnectSendListener() {
    @Override
    public void onResponse(ARequest aRequest, AResponse aResponse) {
        // 发布成功
    }

    @Override
    public void onFailure(ARequest aRequest, AError aError) {
        // 发布失败
    }
});
// 订阅
MqttSubscribeRequest subscribeRequest = new MqttSubscribeRequest();
subscribeRequest.topic = subTopic;
subscribeRequest.isSubscribe = true;
LinkKit.getInstance().subscribe(subscribeRequest, new IConnectSubscribeListener() {
    @Override
    public void onSuccess() {
        // 订阅成功
    }

    @Override
    public void onFailure(AError aError) {
        // 订阅失败
    }
});
// 取消订阅
MqttSubscribeRequest unsubRequest = new MqttSubscribeRequest();
unsubRequest.topic = unSubTopic;
unsubRequest.isSubscribe = false;
LinkKit.getInstance().unsubscribe(unsubRequest, new IConnectUnscribeListener() {
    @Override
    public void onSuccess() {
        // 取消订阅成功
    }

    @Override
    public void onFailure(AError aError) {
        // 取消订阅失败
    }
});
```

**下行数据监听**

下行数据监听可以通过 RRPC 方式或者注册一个下行数据监听器实现。

```
/**
 * RRPC 接口
 *
 * @param request  RRPC 请求
 * @param listener 监听器
 */
void subscribeRRPC(ARequest request, IConnectRrpcListener listener);

/**
 * 注册下行数据监听器
 *
 * @param listener 监听器
 */
void registerOnPushListener(IConnectNotifyListener listener);

/**
 * 取消注册下行监听器
 *
 * @param listener 监听器
 */
void unRegisterOnPushListener(IConnectNotifyListener listener);
```

调用示例：

```
// 下行数据监听
IConnectNotifyListener onPushListener = new IConnectNotifyListener() {
    @Override
    public void onNotify(String connectId, String topic, AMessage aMessage) {
        // 下行数据通知
    }

    @Override
    public boolean shouldHandle(String connectId, String topic) {
        return true; // 是否需要处理 该 topic
    }

    @Override
    public void onConnectStateChange(String connectId, ConnectState connectState) {
        // 连接状态变化
    }
};
// 注册
LinkKit.getInstance().registerOnPushListener(onPushListener);
// 取消注册
LinkKit.getInstance().unRegisterOnPushListener(onPushListener);
```

RRPC 调用示例：

```
final MqttRrpcRegisterRequest registerRequest = new MqttRrpcRegisterRequest();
registerRequest.topic = rrpcTopic;
registerRequest.replyTopic = rrpcReplyTopic;
registerRequest.payloadObj = payload;
// 先订阅回复的 replyTopic
// 云端发布消息到 replyTopic
// 收到下行数据 回复云端 具体可参考 Demo 同步服务调用
LinkKit.getInstance().subscribeRRPC(registerRequest, new IConnectRrpcListener() {
    @Override
    public void onSubscribeSuccess(ARequest aRequest) {
        // 订阅成功
    }

    @Override
    public void onSubscribeFailed(ARequest aRequest, AError aError) {
        // 订阅失败
    }

    @Override
    public void onReceived(ARequest aRequest, IConnectRrpcHandle iConnectRrpcHandle) {
        // 收到云端下行
        AResponse response = new AResponse();
        response.data = responseData;
        iConnectRrpcHandle.onRrpcResponse(registerRequest.topic, response);
    }

    @Override
    public void onResponseSuccess(ARequest aRequest) {
        // RRPC 响应成功
    }

    @Override
    public void onResponseFailed(ARequest aRequest, AError aError) {
        // RRPC 响应失败
    }
});
```

## 混淆配置 {#section_zpm_ttr_z2b .section}

```
-dontwarn com.alibaba.fastjson.**
-dontwarn com.aliyun.alink.linksdk.**
-dontwarn com.facebook.**
-dontwarn okhttp3.internal.**

-keep class com.alibaba.** {*;}
-keep class com.aliyun.alink.linkkit.api.**{*;}
-keep class com.aliyun.alink.dm.api.**{*;}
-keep class com.aliyun.alink.dm.model.**{*;}
-keep class com.aliyun.alink.dm.shadow.ShadowResponse{*;}
## 设备sdk keep
-keep class com.aliyun.alink.linksdk.channel.**{*;}
-keep class com.aliyun.alink.linksdk.tmp.**{*;}
-keep class com.aliyun.alink.linksdk.cmp.**{*;}
-keep class com.aliyun.alink.linksdk.alcs.**{*;}
-keep public class com.aliyun.alink.linksdk.alcs.coap.**{*;}

-keep class com.http.helper.**{*;}
-keep class com.aliyun.alink.apiclient.**{*;}
```

## Android SDK Demo {#section_uz4_tyr_z2b .section}

单击下载[Android SDK Demo](http://gaic.alicdn.com/ztms/android-iot-device-sdk-demo/IoTDeviceSDKDemo.zip)。下载本Demo将默认您同意[本软件许可协议](https://files.alicdn.com/tpsservice/64be8487cc5d8fe6d3c84e4dc9a93c17.pdf)。

