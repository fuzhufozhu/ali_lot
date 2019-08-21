# 使用自定义Topic进行通信 {#concept_423001 .concept}

您可以在物联网平台上自定义Topic类，设备将消息发送到自定义Topic中，服务端通过HTTP/2 SDK获取设备上报消息；服务端通过调用云端API Pub向设备发布指令。

## 背景信息 {#section_ict_9m2_51s .section}

本示例中，电子温度计定期与服务器进行数据的交互，传递温度和指令等信息。温度计向服务器上行发送当前的温度；服务器向温度计下行发送精度设置指令。

![自定义Topic通信](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/341841/156636736148433_zh-CN.png)

## 准备开发环境 {#section_1bq_xtz_3q6 .section}

本文示例中，设备端和云端均使用Java语言的SDK，需先准备Java开发环境。可从[Java 官方网站](http://developers.sun.com/downloads/)下载、安装Java开发环境。

新建项目，添加以下Maven依赖，导入阿里云设备端SDK和云端SDK。

``` {#d7e45}
<dependencies>
 <dependency>
     <groupId>com.aliyun.alink.linksdk</groupId>
     <artifactId>iot-linkkit-java</artifactId>
     <version>1.2.0.1</version>
     <scope>compile</scope>
 </dependency>
 < dependency>
      <groupId>com.aliyun</groupId>
      <artifactId>aliyun-java-sdk-core</artifactId>
      <version>3.7.1</version>
  </dependency>
  <dependency>
      <groupId>com.aliyun</groupId>
      <artifactId>aliyun-java-sdk-iot</artifactId>
      <version>6.9.0</version>
  </dependency>
  <dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>iot-client-message</artifactId>
    <version>1.1.2</version>
</dependency>
</dependencies>
```

## 创建产品和设备 {#section_rrw_0cb_w3b .section}

先在物联网平台创建产品，自定义Topic类，定义物模型，设置服务端订阅，并创建设备。

1.  登录[物联网平台控制台](https://iot.console.aliyun.com/)。
2.  在左侧导航栏，单击**设备管理** \> **产品**。
3.  单击**创建产品**，创建一个温度计产品。

    详细操作指导，请参见[创建产品](../../../../intl.zh-CN/用户指南/产品与设备/创建产品.md#)。

4.  产品创建成功后，单击该产品对应的**查看**。
5.  在产品详情页的Topic类列表页签下，增加自定义Topic类。

    详细操作指导，请参见[自定义Topic](../../../../intl.zh-CN/用户指南/产品与设备/Topic/自定义Topic.md#)。

    本示例中，定义了以下两个Topic类：

    -   设备发布消息Topic：/$\{productKey\}/$\{deviceName\}/user/devmsg，权限为发布。
    -   设备订阅消息Topic：/$\{productKey\}/$\{deviceName\}/user/cloudmsg，权限为订阅。
6.  在服务端订阅页签下，设置HTTP/2通道服务端订阅，订阅**设备上报消息** 。

    详细操作指导，请参见服务端订阅[开发指南\(Java\)](../../../../intl.zh-CN/用户指南/产品与设备/服务端订阅/开发指南（Java）.md#)。

    ![自定义Topic通信](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/341841/156636736250122_zh-CN.png)

7.  在左侧导航栏，单击**设备**，然后在刚创建的温度计产品下，添加设备。详细操作指导，请参见[单个创建设备](../../../../intl.zh-CN/用户指南/产品与设备/创建设备/单个创建设备.md#)。

## 设备发送消息给服务器 {#section_1oo_pqf_80d .section}

流程图：

![自定义Topic通信](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/341841/156636736448448_zh-CN.png)

-   配置服务端HTTP/2 SDK，设置Topic回调。

    配置服务端订阅后，服务器监听产品下所有设备的全部上报消息。

    -   配置HTTP/2 SDK接入物联网平台 。

        ``` {#codeblock_n3y_vf6_trl}
        // 身份认证
        String accessKey = "您的阿里云accessKey";
        String accessSecret = "您的阿里云accessSecret";
        String uid = "您的阿里云账号ID";
        String regionId = "您设备所处区域regionId";
        String productKey = "您的产品productKey";
        String deviceName = "您的设备名字deviceName";
        String endPoint = "https://" + uid + ".iot-as-http2." + regionId + ".aliyuncs.com";
        // 连接配置
        Profile profile = Profile.getAccessKeyProfile(endPoint, regionId, accessKey, accessSecret);
        
        // 构造客户端
        MessageClient client = MessageClientFactory.messageClient(profile);
        
        // 数据接收
        client.connect(new MessageCallback() {
            @Override
            public Action consume(MessageToken messageToken) {
                Message m = messageToken.getMessage();
                System.out.println("\ntopic=" + m.getTopic());
                System.out.println("payload=" + new String(m.getPayload()));
                System.out.println("generateTime=" + m.getGenerateTime());
                // 此处标记CommitSuccess已消费，IoT平台会删除当前Message，否则会保留到过期时间
                return MessageCallback.Action.CommitSuccess;
             }
        });
        ```

    -   设置消息接收接口。

        本示例中，服务端订阅Topic：/$\{productKey\}/$\{deviceName\}/user/devmsg，因此设置该Topic的回调函数。

        ``` {#codeblock_s48_s7v_pcb}
        //定义回调函数
        MessageCallback messageCallback = new MessageCallback() {
        //获取消息并打印出来，服务器可以在此设置对应的动作
                    @Override
                    public Action consume(MessageToken messageToken) {
                        Message m = messageToken.getMessage();
                        log.info("receive : " + new String(messageToken.getMessage().getPayload()));
                        System.out.println(messageToken.getMessage());
                        return MessageCallback.Action.CommitSuccess;
                    }
                };
                //注册回调函数
                client.setMessageListener("/" + productKey + "/" + deviceName + "/user/devmsg", messageCallback);
        ```

    更多具体配置详情，请参见HTTP/2 SDK[开发指南\(Java\)](../../../../intl.zh-CN/用户指南/产品与设备/服务端订阅/开发指南（Java）.md#)。

-   配置设备端SDK发送消息。
    -   配置设备认证信息。

        ``` {#codeblock_8a0_qm6_mq4}
        final String productKey = "XXXXXX";
        final String deviceName = "XXXXXX";
        final String deviceSecret = "XXXXXXXXX";
        final String region = "XXXXXX";
        ```

    -   设置初始化连接参数，包括MQTT连接配置、设备信息和初始物模型属性。

        ``` {#codeblock_brg_alz_ra5}
        LinkKitInitParams params = new LinkKitInitParams();
        //LinkKit底层是mqtt协议，设置mqtt的配置
        IoTMqttClientConfig config = new IoTMqttClientConfig();
        config.productKey = productKey;
        config.deviceName = deviceName;
        config.deviceSecret = deviceSecret;
        config.channelHost = productKey + ".iot-as-mqtt." + region + ".aliyuncs.com:1883";
        //设备的信息
        DeviceInfo deviceInfo = new DeviceInfo();
        deviceInfo.productKey = productKey;
        deviceInfo.deviceName = deviceName;
        deviceInfo.deviceSecret = deviceSecret;
        //报备的设备初始状态
        Map<String, ValueWrapper> propertyValues = new HashMap<String, ValueWrapper>();
        
        params.mqttClientConfig = config;
        params.deviceInfo = deviceInfo;
        params.propertyValues = propertyValues;
        ```

    -   初始化连接。

        ``` {#codeblock_lrf_1dy_vcg}
        //连接并设置连接成功以后的回调函数
        LinkKit.getInstance().init(params, new ILinkKitConnectListener() {
             @Override
             public void onError(AError aError) {
                 System.out.println("Init error:" + aError);
             }
        
             //初始化成功以后的回调
             @Override
             public void onInitDone(InitResult initResult) {
                 System.out.println("Init done:" + initResult);
             }
         });
        ```

    -   设备发送消息。

        设备端连接物联网平台后，向以上定义的Topic发送消息。需将onInitDone函数内容替换为以下内容：

        ``` {#codeblock_itg_4gg_t1v}
        @Override
         public void onInitDone(InitResult initResult) {
             //设置pub消息的topic和内容
             MqttPublishRequest request = new MqttPublishRequest();
             request.topic = "/" + productKey + "/" + deviceName + "/user/devmsg";
             request.qos = 0;
             request.payloadObj = "{\"temperature\":35.0, \"time\":\"sometime\"}";
             //发送消息并设置成功以后的回调
             LinkKit.getInstance().publish(request, new IConnectSendListener() {
                 @Override
                 public void onResponse(ARequest aRequest, AResponse aResponse) {
                     System.out.println("onResponse:" + aResponse.getData());
                 }
        
                 @Override
                 public void onFailure(ARequest aRequest, AError aError) {
                     System.out.println("onFailure:" + aError.getCode() + aError.getMsg());
                 }
             });
         }
        ```

        服务器收到消息如下：

        ``` {#codeblock_7m2_jrb_8w0}
        Message
        {payload={"temperature":35.0, "time":"sometime"},
        topic='/a1uzcH0****/device1/user/devmsg',
        messageId='1131755639450642944',
        qos=0,
        generateTime=1558666546105}
        ```


## 服务器发送消息给设备 {#section_bhe_qsr_tpd .section}

流程图：

![自定义Topic通信](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/341841/156636736448453_zh-CN.png)

-   配置设备端SDK订阅Topic。

    设备认证配置、设置初始化连接参数、和初始化连接，请参见上一章节的设备端SDK配置。

    设备要接收服务器发送的消息，还需订阅消息Topic。

    配置设备端订阅Topic示例如下：

    ``` {#codeblock_s17_y9x_qty}
    //初始化成功以后的回调
    @Override
    public void onInitDone(InitResult initResult) {
        //设置订阅的topic
        MqttSubscribeRequest request = new MqttSubscribeRequest();
        request.topic = "/" + productKey + "/" + deviceName + "/user/cloudmsg";
        request.isSubscribe = true;
        //发出订阅请求并设置订阅成功或者失败的回调函数
        LinkKit.getInstance().subscribe(request, new IConnectSubscribeListener() {
            @Override
            public void onSuccess() {
                System.out.println("");
            }
    
            @Override
            public void onFailure(AError aError) {
    
            }
        });
    
        //设置订阅的下行消息到来时的回调函数
        IConnectNotifyListener notifyListener = new IConnectNotifyListener() {
            //此处定义收到下行消息以后的回调函数。
            @Override
            public void onNotify(String connectId, String topic, AMessage aMessage) {
                System.out.println(
                    "received message from " + topic + ":" + new String((byte[])aMessage.getData()));
            }
    
            @Override
            public boolean shouldHandle(String s, String s1) {
                return false;
            }
    
            @Override
            public void onConnectStateChange(String s, ConnectState connectState) {
    
            }
        };
        LinkKit.getInstance().registerOnNotifyListener(notifyListener);
    }
    ```

-   配置云端SDK调用云端API [Pub](../../../../intl.zh-CN/云端开发指南/云端API参考/消息通信/Pub.md#)发布消息。
    -   设置身份认证信息。

        ``` {#codeblock_wam_f4n_4sh}
         String regionId = "您设备所处区域regionId";
         String accessKey = "您的阿里云账号accessKey";
         String accessSecret = "您的阿里云账号accessSecret";
         final String productKey = "您的产品productKey";
        ```

    -   设置连接参数。

        ``` {#codeblock_1o2_ikh_7gy}
        //设置client的参数
        DefaultProfile profile = DefaultProfile.getProfile(regionId, accessKey, accessSecret);
        IAcsClient client = new DefaultAcsClient(profile);
        ```

    -   设置消息发布参数。

        ``` {#codeblock_c93_fm9_b28}
        PubRequest request = new PubRequest();
        request.setQos(0);
        //设置发布消息的topic
        request.setTopicFullName("/" + productKey + "/" + deviceName + "/user/cloudmsg");
        request.setProductKey(productKey);
        //设置消息的内容，一定要用base64编码，否则乱码
        request.setMessageContent(Base64.encode("{\"accuracy\":0.001,\"time\":now}"));
        ```

    -   发送消息。

        ``` {#codeblock_obb_hue_hkl}
        try {
             PubResponse response = client.getAcsResponse(request);
             System.out.println("pub success?:" + response.getSuccess());
         } catch (Exception e) {
             System.out.println(e);
         }
        ```

        设备端接收到的消息如下：

        ``` {#codeblock_nj8_u17_4rj}
        msg = [{"accuracy":0.001,"time":now}]
        ```


## 附录：Demo {#section_y5z_eyd_h0d .section}

单击[Pub/SubDemo](https://iotx-demo.oss-cn-hangzhou.aliyuncs.com/PubSubDemo.zip)，下载、查看本示例的完整代码Demo。

