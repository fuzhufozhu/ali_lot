# Android-SDK {#concept_t5l_5km_b2b .concept}

本章节介绍如何使用Android-SDK，将Android设备接入物联网平台。

Android SDK封装了 MQTT 建连、长连接维护和基于MQTT协议的上下行请求的功能。

## 快速接入 {#section_zmr_ykm_b2b .section}

1.  SDK引入。
    1.  在根目录下的build.gradle 基础配置文件中，加入仓库地址，进行仓库配置。

        ```
        allprojects { 
           repositories { 
             jcenter()
             // 阿里云仓库地址
             maven { 
                url "http://maven.aliyun.com/nexus/content/repositories/releases/" 
        } 
        }
        }
        ```

    2.  在模块的 build.gradle 中，添加 public-channel-core 的依赖，引入 SDK :public-channel-core。

        ```
        compile('com.aliyun.alink.linksdk:public-channel-core:0.2.8')
        ```

    3.  SDK 基于Paho mqttv3 Java开源库完成MQTT连接，自动间接依赖相关库。

        ```
        compile 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'
        compile 'com.alibaba:fastjson:1.2.40'
        ```

2.  登录阿里云物联网平台的控制台，选择设备管理，单击设备后的**查看**，获取需要的三元组信息，即ProductKey、DeviceName及DeviceSecret。
3.  SDK API开发指引，具体请参见下文各个SDK详细说明。

## SDK 日志开关 {#section_b4c_2r3_b2b .section}

打开SDK内部日志输出开关：

```
ALog.setLevel(ALog.LEVEL_DEBUG);
```

## SDK 环境配置 {#section_ozz_4r3_b2b .section}

SDK环境配置，需要在SDK初始化之前。

SDK支持自定义切换Host。

默认连接的地址为上海节点： `ssl://${YourProductKey}.iot-as-mqtt.cn-shanghai.aliyuncs.com:1883`

还包括美西节点和新加坡节点：

```
MqttConfigure.HOST = "ssl://${YourProductKey}.iot-as-mqtt.us-west-1.aliyuncs.com:1883";//美西节点
MqttConfigure.HOST = "ssl://${YourProductKey}.iot-as-mqtt.ap-southeast-1.aliyuncs.com:1883";//新加坡节点
```

## SDK 初始化 {#section_qrs_tr3_b2b .section}

使用三元组信息对SDK进行初始化，SDK会进行 MQTT 建联。

```
MqttInitParams initParams = new MqttInitParams(productKey,deviceName,deviceSecret);
PersistentNet.getInstance().init(context,initParams);
```

## 添加通道状态变化监听 {#section_tpr_cs3_b2b .section}

添加通道状态变化监听，获取MQTT建连结果的反馈和监听后续通道状态变化。

```
PersistentEventDispatcher.getInstance().registerOnTunnelStateListener(channelStateListener,isUiSafe);// 注册监听
PersistentEventDispatcher.getInstance().unregisterOnTunnelStateListener(connectionStateListener);//取消监听
    public interface IConnectionStateListener {
        void onConnectFail(String msg); //通道连接失败
        void onConnected(); // 通道已连接
        void onDisconnect(); // 通道断连
}
```

## 上行请求接口 {#section_xmk_fs3_b2b .section}

调用上行请求接口，SDK 封装了上行Publish请求、订阅Subscribe和取消订阅unSubscribe等接口。

```
/**
* @param request 发送数据对象基类，需要根据不同的INet实现类传入不同的ARequest子类对象
*               长连接传入：PersistentRequest对象
*               短连接传：TransitoryRequest对象
* @param listener 发送结果回调接口
* @return AConnect对象，可以用于重试
*/
ASend asyncSend(ARequest request, IOnCallListener listener);
/**重新发送请求
* @param connect 上次调用asyncSend返回值
*/
void retry(ASend connect);
/** 订阅下行消息，订阅后的消息统一在 IOnPushListener 中接收
* @param topic
* @param listener
*/
void subscribe(String topic,IOnSubscribeListener listener);
/** 取消订阅下行消息
* @param topic
* @param listener
*/
void unSubscribe(String topic,IOnSubscribeListener listener);
```

调用示例：

```
//Publish 请求
MqttPublishRequest publishRequest = new MqttPublishRequest();
publishRequest.isRPC = false;
publishRequest.topic = "/productKey/deviceName/update";
publishRequest.payloadObj = "content";
PersistentNet.getInstance().asyncSend(publishRequest, new IOnCallListener() {
        @Override
        public void onSuccess(ARequest request, AResponse response) {
            ALog.d(TAG,"send , onSuccess");
}
         @Override
         public void onFailed(ARequest request, AError error) {
             ALog.d(TAG,"send , onFailed");
}
         @Override
         public boolean needUISafety() {
              return false;
}
});
//订阅
PersistentNet.getInstance().subscribe(topic, listener);
//取消订阅
PersistentNet.getInstance().unSubscribe(topic, listener);
```

## 下行数据监听 {#section_gnv_hs3_b2b .section}

订阅Topic后，云端推送的下行消息监听。

```
PersistentEventDispatcher.getInstance().registerOnPushListener(onPushListener,isUiSafe);// 注册监听
PersistentEventDispatcher.getInstance().unregisterOnPushListener(onPushListener);//取消监听
         public interface IOnPushListener {
             /**下推消息回调接口
               * @param topic
               * @param data 下推的消息内容
              */
             void onCommand(String topic,String data);
            /**根据method过滤消息
             * @param topic : 本次下推消息的命令名称
              * @return : 若返回true，则onCommand被调用，返回false，则onCommand不会被调用
             */
             boolean shouldHandle(String topic);
}
```

## 实例代码 {#section_ccz_nlm_b2b .section}

完整Android工程项目Demo，请访问[aliyunIoTAndroidDemo.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/64183/cn_zh/1527151318870/aliyunIoTAndroidDemo.zip)下载。

