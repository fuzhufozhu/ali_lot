# Android SDK {#concept_t5l_5km_b2b .concept}

This topic describes how to use the Android SDK to connect your Android devices to IoT Platform .

The Android SDK includes Message Queuing Telemetry Transport \(MQTT\) APIs, and supports MQTT-based communication and persistent connections.

## Integrate the SDK {#section_zmr_ykm_b2b .section}

1.  Reference the SDK.
    1.  Add the repository address to the build.gradle file in the root directory to configure the repository.

        ```
        
        allprojects { 
           repositories { 
             jcenter()
             // Alibaba Cloud repository address.
             maven { 
                url "http://maven.aliyun.com/nexus/content/repositories/releases/" 
        } 
        }
        }
        ```

    2.  In the build.gradle file, add a public-channel-core dependency to reference this SDK.

        ```
        compile('com.aliyun.alink.linksdk:public-channel-core:0.2.1')
        ```

    3.  This SDK provides MQTT connections based on Java open-source library Paho mqttv3. All dependencies of this library will be automatically added.

        ```
        
        compile 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'
        compile 'com.alibaba:fastjson:1.2.40'
        ```

2.  Log on to the IoT Platform console, go to the Devices page, and click **View** next to the target device to obtain the unique certificates, including ProductKey, DeviceName, and DeviceSecret.
3.  For more information about configuring API operations for this SDK, see the following descriptions of SDKs.

## SDK log enabler {#section_b4c_2r3_b2b .section}

Enable SDK log output:

```
ALog.setLevel(ALog.LEVEL_DEBUG);
```

## Configure the SDK environment {#section_ozz_4r3_b2b .section}

Configure the SDK environment before initializing the SDK.

The SDK supports custom host switching.

The device accesses the node in the China \(Shanghai\) region by default: `ssl://[productKey].iot-as-mqtt.cn-shanghai.aliyuncs.com:1883`

The device can also access the node in the US \(Silicon Valley\) region or the Singapore region:

```

MqttConfigure.HOST = "ssl://[productKey].iot-as-mqtt.us-west-1.aliyuncs.com:1883";//Access the node in US (Silicon Valley).
MqttConfigure.HOST = "ssl://[productKey].iot-as-mqtt.ap-southeast-1.aliyuncs.com:1883";//Access the node in Singapore.
```

## Initialize the SDK {#section_qrs_tr3_b2b .section}

Use the unique certificates, including ProductKey, DeviceName, and DeviceSecret, to initialize the SDK. Then, the SDK creates an MQTT-based connection.

```

MqttInitParams initParams = new MqttInitParams(productKey,deviceName,deviceSecret);
PersistentNet.getInstance().init(context,initParams);
```

## Listen on channel status changes {#section_tpr_cs3_b2b .section}

Listen on channel status changes to obtain the result of the MQTT-based connection and follow-up channel status.

```

PersistentEventDispatcher.getInstance().registerOnTunnelStateListener(channelStateListener,isUiSafe);// Register the listening process.
PersistentEventDispatcher.getInstance().unregisterOnTunnelStateListener(connectionStateListener);//Cancel the listening process.
    public interface IConnectionStateListener {
        void onConnectFail(String msg); //The connection failed.
        void onConnected(); // The device has been connected to IoT Platform.
        void onDisconnect(); // The device has been disconnected from IoT Platform.
}
```

## Upstream request operation {#section_xmk_fs3_b2b .section}

The SDK includes upstream request operations for publish, subscribe, and unsubscribe.

```

/**
* @param request Specifies the base class of the request that you want to send. The ARequest object to be passed in varies, depending on the iNet implementation class.
* Persistent connection: Specifies the PersistentRequest object.
* Transient connection: Specifies the TransitoryRequest object.
* @param listener Use this callback operation to obtain the request result.
* @return Indicates the AConnect object. You can use this object for reconnections to IoT Platform.
*/
ASend asyncSend(ARequest request, IOnCallListener listener);
/**Send the request again.
* @param connect Indicates the response to the last asyncSend operation.
*/
void retry(ASend connect);
/** Subscribe to downstream messages, and receive these messages using IOnPushListener.
* @param topic
* @param listener
*/
void subscribe(String topic,IOnSubscribeListener listener);
/** Unsubscribe from downstream messages.
* @param topic
* @param listener
*/
void unSubscribe(String topic,IOnSubscribeListener listener);
```

Examples of calling these operations

```

//Send a publishing request.
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
//Subscribe
PersistentNet.getInstance().subscribe(topic, listener);
//Unsubscribe
PersistentNet.getInstance().unSubscribe(topic, listener);
```

## Listen on downstream data {#section_gnv_hs3_b2b .section}

You can listen on downstream messages from the cloud after subscribing to related topics.

```

PersistentEventDispatcher.getInstance().registerOnPushListener(onPushListener,isUiSafe);// Register the listening process.
PersistentEventDispatcher.getInstance().unregisterOnPushListener(onPushListener);//Cancel the listening process.
         public interface IOnPushListener {
             /**Use this callback operation to obtain downstream messages.
               * @param topic
               * @param data Indicates the content of downstream messags.
              */
             void onCommand(String topic,String data);
            /**Filters messages based on related operations.
             * @param topic: Specifies the command name for the current downstream message.
              * @return: Responds with true to use onCommand, and responds with false to cancel onCommand.
             */
             boolean shouldHandle(String topic);
}
```

## Example code {#section_ccz_nlm_b2b .section}

For more information about downloading the complete Android project demo, see [aliyunIoTAndroidDemo.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/64183/cn_zh/1527151318870/aliyunIoTAndroidDemo.zip).

