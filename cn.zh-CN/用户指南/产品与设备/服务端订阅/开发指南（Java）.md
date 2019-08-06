# 开发指南（Java） {#concept_cgg_k2v_y2b .concept}

本文档为服务端订阅功能的开发指南，介绍了Java版本的开发方法。完成后，您可以成功使用SDK，通过HTTP/2通道，接收物联网平台推送过来的设备消息。

服务端订阅Demo：[HTTP/2 SDK \(Java\) server side demo](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/java-http2-sdk-demo/http2-server-side-demo.zip)。

以下简单介绍服务端订阅的开发流程。

## 配置服务端订阅 {#section_tbd_2s5_42b .section}

1.  登录[物联网平台控制台](https://iot.console.aliyun.com/product/region/cn-shanghai)。
2.  左侧导航栏选择**设备管理** \> **产品**。
3.  在产品列表中，搜索到要配置服务端订阅的产品，并单击该产品对应的**查看**按钮，进入产品详情页。
4.  单击**服务端订阅** \> **设置**。
5.  选择推送的消息类型。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18850/156507077412666_zh-CN.png)

    -   **设备上报消息**：指产品下所有设备 Topic 列表中，具有发布权限的 Topic 中的消息。勾选后，可以通过 HTTP/2 SDK 接收这些消息。

        设备上报消息，包括设备上报的自定义数据和物模型数据（属性上报、事件上报、属性设置响应和服务调用响应）。

        例如，一个产品有3个Topic类，分别是：

        -   /$\{YourProductKey\}/$\{YourDeviceName\}/user/get，具有订阅权限。
        -   /$\{YourProductKey\}/$\{YourDeviceName\}/user/update，具有发布权限。
        -   /sys/$\{YourProductKey\}/$\{YourDeviceName\}/thing/event/property/post，具有发布权限。
        那么，服务端订阅会推送具有发布权限的Topic类中的消息，即/$\{YourProductKey\}/$\{YourDeviceName\}/user/update和/sys/$\{YourProductKey\}/$\{YourDeviceName\}/thing/event/property/post中的消息。其中，/sys/$\{YourProductKey\}/$\{YourDeviceName\}/thing/event/property/post中的数据已经过系统处理。

    -   **设备状态变化通知**：指一旦该产品下的设备状态变化时通知的消息，例如设备上线、下线的消息。设备状态消息的发送 Topic 为 /as/mqtt/status/$\{YourProductKey\}/$\{YourDeviceName\}。勾选后，可以通过 HTTP/2 SDK 接收设备状态变化的通知消息。
    -   **网关发现子设备上报**：网关可以将发现的子设备信息上报给物联网平台。需要网关上的应用程序支持。网关产品特有消息类型。
    -   **设备拓扑关系变更**：指子设备和网关之间的拓扑关系建立和解除消息。网关产品特有消息类型。
    -   **设备生命周期变更**：包括设备创建、删除、禁用、启用等消息。
    **说明：** 设备上报物模型消息、设备状态变化通知、网关发现子设备上报消息、设备拓扑关系变更消息和设备生命周期变更消息均默认为QoS=0；设备上报消息（除物模型相关消息外），您可以在设备端上设置为QoS=0或QoS=1。


## 接入 SDK {#section_v3d_gj5_42b .section}

在Maven工程项目中添加以下依赖，安装阿里云IoT SDK。

``` {#codeblock_rsu_dk8_swk}
<dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>iot-client-message</artifactId>
    <version>1.1.3</version>
</dependency>

<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>aliyun-java-sdk-core</artifactId>
    <version>3.7.1</version>
</dependency>
```

## 身份认证 {#section_atv_yl5_42b .section}

使用服务端订阅功能，需要基于您的阿里云 AccessKey 进行身份认证并建立连接。

建立连接示例如下：

``` {#codeblock_6tf_lfm_t6d}
// 阿里云accessKey
String accessKey = "xxxxxxxxxxxxxxx";
// 阿里云accessSecret
String accessSecret = "xxxxxxxxxxxxxxx";
// regionId
String regionId = "cn-shanghai";
// 阿里云uid
String uid = "xxxxxxxxxxxx";
// endPoint:  https://${uid}.iot-as-http2.${region}.aliyuncs.com
String endPoint = "https://" + uid + ".iot-as-http2." + regionId + ".aliyuncs.com";

// 连接配置
Profile profile = Profile.getAccessKeyProfile(endPoint, regionId, accessKey, accessSecret);

// 构造客户端
MessageClient client = MessageClientFactory.messageClient(profile);
// 数据接收
client.connect(messageToken -> {
    Message m = messageToken.getMessage();
    System.out.println("receive message from " + m);
    return MessageCallback.Action.CommitSuccess;
});
```

以上示例中账号相关信息的获取方法：

|参数|获取途径|
|:-|:---|
|accessKey|您的账号AccessKey ID。 登录阿里云控制台，将光标移至账号头像上，然后单击**accesskeys**，跳转至用户信息管理页，即可获取。

 |
|accessSecret|您的账号AccessKey Secret。获取方式同accessKey。|
|uid|您的账号ID。 用主账号登录阿里云控制台，单击账号头像，跳转至账号管理控制台，即可获取账号UID。

 |
|regionId|您的物联网平台服务所在地域代码。 在物联网平台控制台页，右上方即可查看地域（Region）。RegionId 的表达方法，请参见文档[地域和可用区](../../../../intl.zh-CN/通用参考/地域和可用区.md#)。

 |

## 调用连接相关的方法 {#section_mml_5j3_scq .section}

|方法|说明|
|--|:-|
|connect方法|调用该方法建立服务端HTTP/2 SDK和物联网平台的连接。连接建立后，服务端便可以收到物联网平台推送的消息。 ``` {#codeblock_5uh_5g0_6fv}
void connect(MessageCallback callback);
```

 |
|disconnect方法|调用该方法断开服务端HTTP/2 SDK与物联网平台连接，停止接收消息。 ``` {#codeblock_esh_dyt_g1i}
void disconnect();
```

 |
|isConnected方法|调用该方法判断HTTP/2 SDK是否已连接物联网平台。 ``` {#codeblock_yqs_wvt_u7c}
boolean isConnected();
```

 |
|setConnectionCallback方法|调用该方法设置连接状态监听回调。 ``` {#codeblock_g7u_xt6_s4a}
void setConnectionCallback(ConnectionCallback var1);
```

 示例：

 ``` {#codeblock_423_bng_uhe}
client.setConnectionCallback(new ConnectionCallback() {
    @Override
    public void onConnectionLost() {
        System.out.println("连接断开");
    }

    @Override
    public void onConnected(boolean isReconnected) {
        System.out.println("连接成功，是否为重连: " + isReconnected);

    }
});
```

 |

## 设置消息接收接口 {#section_fnd_3x5_42b .section}

连接建立后，服务端会立即向SDK推送已订阅的消息。因此，建立连接时，需要提供消息接收接口，用于处理未设置回调的消息。建议在连接之前，调用 setMessageListener 设置消息回调。

您需要通过 MessageCallback接口的consume方法，和调用messageClient的setMessageListener\(\)方法来设置消息接收接口。

consume方法的返回值决定SDK是否发送 ACK（acknowledgement，即回复确认消息）。

设置消息接收接口的方法如下：

``` {#codeblock_gsk_jxi_mas}
MessageCallback messageCallback = new MessageCallback() {
    @Override
    public Action consume(MessageToken messageToken) {
        Message m = messageToken.getMessage();
        log.info("receive : " + new String(messageToken.getMessage().getPayload()));
        return MessageCallback.Action.CommitSuccess;
    }
};
messageClient.setMessageListener("/${YourProductKey}/#",messageCallback);
```

其中，

-   参数MessageToken指消息回执的消息体。通过MessageToken.getMessage\(\)可获取消息体。MessageToken可以用于手动回复ACK。

    消息体包含的内容如下：

    ``` {#codeblock_1b6_qnd_apa}
    public class Message {
        // 消息体
        private byte[] payload;
        // Topic 
        private String topic;
        // 消息ID
        private String messageId;
        // QoS
        private int qos;
    }
    ```

    各类型消息的具体格式，请参考[数据格式](intl.zh-CN/用户指南/规则引擎/数据流转/数据格式.md#)。

    **说明：** 关于设备上下线状态，为避免消息时序紊乱造成影响，建议您根据消息中的lastTime字段来判断最终设备状态。

-   示例中，`messageClient.setMessageListener("/${YourProductKey}/#",messageCallback);`用于设置指定Topic回调。

    您可以设置为指定Topic回调，也可以设置为通用回调。

    -   指定Topic回调

        指定Topic回调的优先级高于通用回调。一条消息匹配到多个Topic时，按字典顺序优先调用，并且仅回调一次。

        设置回调时，可以指定带通配符的Topic，如/$\{YourProductKey\}/$\{YourDeviceName\}/\# 。

        示例：

        ``` {#codeblock_s1b_7jc_8go}
        messageClient.setMessageListener("/alEddfaXXXX/device1/#",messageCallback);
        //当收到消息的Topic，如"/alEddfaXXXX/device1/update"，匹配指定Topic时，会优先调用该回调
        ```

    -   通用回调

        未指定Topic回调的消息，则调用通用回调。

        设置通用回调方法：

        ``` {#codeblock_w04_3i5_4pa}
        messageClient.setMessageListener(messageCallback);
        //当收到消息topic未匹配到已定指的Topic 回调时，调用该回调
        ```

-   设置回复ACK。

    QOS=1的消息消费后，需要回复ACK。支持设置为自动回复ACK和手动回复ACK。默认为自动回复 ACK。本示例中未设置回复 ACK，则默认为自动回复。

    -   自动回复ACK：设置为自动回复ACK后，若MessageCallback.consume的返回值为true， SDK会发送ACK；返回false或抛出异常，则不会返回ACK。对于QOS=1且未回复ACK的消息，服务器会重新发送。
    -   手动回复ACK方法：

        ``` {#codeblock_agy_uri_s9w}
        CompletableFuture<Boolean> ack(MessageToken var1);
        ```

        调用该方法的请求参数为MessageToken，表示需要回复ACK的消息。该参数值从MessageCallback回调信息中获取。

        设置回复ACK示例：

        ``` {#codeblock_34v_3da_p96}
        client.connect(messageToken -> {
            System.out.println(messageToken.getMessage());
            asyncHandleMessage(messageToken);
            return MessageCallback.Action.CommitAckManually;
        });
        public void asyncHandleMessage(MessageToken messageToken) {
          client.ack(messageToken)
        }
        ```


## 相关文档 {#section_xh3_hmg_dyk .section}

-   服务端订阅使用限制说明，请参见[使用限制](intl.zh-CN/用户指南/产品与设备/服务端订阅/使用限制.md#)
-   查看各类型消息的具体格式，请参见[数据格式](intl.zh-CN/用户指南/规则引擎/数据流转/数据格式.md#)。

