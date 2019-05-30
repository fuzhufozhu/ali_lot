# 使用HTTP/2 SDK\(.NET\)接入 {#concept_xlx_dzz_ggb .concept}

物联网平台提供HTTP/2 SDK\(.NET\) ，用于建立设备端与物联网平台的通信。此处提供了SDK Demo，您可以参考此Demo，开发SDK，将设备端消息上传至物联网平台。

## 使用.NET SDK的前提条件 {#section_z4t_bf1_hgb .section}

安装.NET SDK的开发工具 Visual Studio。

## .NET SDK的开发步骤 {#section_hnx_ch1_hgb .section}

1.  下载[HTTP/2 .NET SDK](https://iot-demos.oss-cn-shanghai.aliyuncs.com/h2/iotx-as-http2-net-sdk.zip)。
2.  单击[iot-http2-net-sdk-demo](https://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/net-http2-sdk-demo/iot-http2-net-sdk-demo.zip)下载.NET SDK Demo。
3.  使用Visual Studio，将该Demo导入到工程里。
4.  从控制台设备详情页获取设备的证书信息（ProductKey、DeviceName和DeviceSecret）。
5.  根据您的设备信息和业务需求修改Demo中的配置信息。
    1.  配置设备信息。

        ```
        //从控制台获取productKey、deviceName、deviceSecret信息
        string productKey = "";
        string deviceName = "";
        string deviceSecret = "";
        
        //用于发送和接收消息的topic，具有发布和订阅权限，具体可在控制台查看
        string topic = "/" + productKey + "/" + deviceName + "/update";
        ```

    2.  连接HTTP/2服务器，并设置数据接收。

        ```
        IPHostEntry host = Dns.GetHostEntry(Dns.GetHostName());
        // 客户端设备唯一标记
        string clientId = host.AddressList.FirstOrDefault(
            ip => ip.AddressFamily == System.Net.Sockets.AddressFamily.InterNetwork).ToString();
        string regionId = "cn-shanghai";
        string domain = ".aliyuncs.com";
        string endpoint = "https://" + productKey + ".iot-as-http2." + regionId + domain;
        
        //连接服务配置项
        Profile profile = new Profile();
        profile.ProductKey = productKey;
        profile.DeviceName = deviceName;
        profile.DeviceSecret = deviceSecret;
        profile.Url = endpoint;
        profile.ClientId = clientId;
        //删除堆积消息
        profile.CleanSession = true;
        //qos>0消息，SDK发生异常时可以设置重
        profile.RetryPubCount = 3;
        //重试间隔时间单位为s(秒)
        profile.RetryIntervalTime = 2;
        profile.GetDeviceAuthParams();
        
        //构造客户端
        IMessageClient client = new MessageClient(profile);
        
        //接收数据
        client.DoConnection(new DefaultHttp2MessageCallback());
        ```

    3.  订阅Topic。

        ```
        //topic订阅
        client.DoSubscribe(topic, msg =>
        {
            Console.WriteLine("subscribe topic: " + msg.Code + "|| topic:" + msg.Message.Topic);
        });
        ```

    4.  配置数据发送。

        ```
        //发消息
        Message message = new Message();
        message.Payload = Encoding.ASCII.GetBytes("hello, iot");
        message.Qos = 1;
        client.DoPublish(topic, message, msg =>
        {
            Console.WriteLine("publish topic message, messageId: " + msg.Message.MessageId 
                              + "|| topic:" + msg.Message.Topic
                      + "|| code: " + msg.Code
                      + "|| body: " + Encoding.ASCII.GetString(msg.Body));
        });
        ```


配置修改完成后即可运行SDK进行调试。

## 接口说明 {#section_bfp_mhb_3gb .section}

-   设备身份认证接口

    设备连接物联网平台时，需要使用Profile配置设备身份及相关参数。配置接口参数如下：

    ```
    //连接服务配置项
    Profile profile = new Profile();
    profile.ProductKey = productKey;
    profile.DeviceName = deviceName;
    profile.DeviceSecret = deviceSecret;
    profile.Url = endpoint;
    profile.ClientId = clientId;
    //删除堆积消息
    profile.CleanSession = true;
    //qos>0消息，SDK发生异常时可以设置重
    profile.RetryPubCount = 3;
    //重试间隔时间单位为s(秒)
    profile.RetryIntervalTime = 2;
    profile.GetDeviceAuthParams();
    
    //构造客户端
    IMessageClient client = new MessageClient(profile);
    
    //接收数据
    client.DoConnection(new DefaultHttp2MessageCallback());
    ```

    |参数|类型|是否必需|描述|
    |:-|:-|:---|:-|
    |ProductKey|String|是|设备所隶属产品的Key。可从控制台设备详情页获取。|
    |DeviceName|String|是|设备的名称。可从控制台设备详情页获取。|
    |DeviceSecret|String|是|设备的密钥。可从控制台设备详情页获取。|
    |Url|String|是|接入点地址。格式为`https://${productKey}.iot-as-http2.${regionId}.aliyuncs.com`。     -   变量$\{productKey\}替换为您的产品Key。
    -   变量$\{regionId\}替换为您的服务地域代码。地域代码表达方法，请参见[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)文档。
 |
    |ClientId|String|是|客户端设备的唯一标识。建议使用设备的MAC地址或序列码作为ClientId。长度限制为64字节。|
    |CleanSession|Bool|否|是否清除缓存的离线消息。可选：     -   true：清除。
    -   false：不清除。
 |
    |RetryIntervalTime|Integer|否|QoS=1消息发送失败后的重试间隔时间。|
    |RetryPubCount|Integer|否|QoS=1消息发送失败后的重试次数。|
    |IsMultiConnection|Bool|否|是否使用多连接。 **说明：** 若使用设备的productKey和deivceName接入时，请将此参数值设置为false。

 |
    |AuthParams|Dictionary|是|自定义认证参数。|

-   建立连接接口

    获取MessageClient之后，设备需要与物联网平台建立连接。设备身份认证成功后才可收发消息。连接建立成功，服务端会立即向SDK推送已订阅的消息，因此建连时需要提供默认消息接收接口，用于处理未设置回调的消息。建连接口如下：

    ```
    void DoConnection(IHttp2MessageCallback callback);
    ```

-   消息订阅接口

    ```
    /// <summary>
    /// 订阅topic
    /// </summary>
    /// <returns>订阅消息</returns>
    /// <param name="topic">Topic.</param>
    void DoSubscribe(string topic, SubscribeDelegate subscribeDelegate);
    
    /// <summary>
    /// 订阅topic,并制定该topic回调 
    /// </summary>
    /// <returns>订阅消息</returns>
    /// <param name="">Topic.</param>
    void DoSubscribe(string topic, IHttp2MessageCallback callback, SubscribeDelegate subscribeDelegate);
    
    /// <summary>
    /// 取消订阅
    /// </summary>
    /// <returns>取消订阅消息</returns>
    /// <param name="topic">Topic.</param>
    void DoUnSubscribe(string topic, UnSubscribeDelegate unSubscribeDelegate);
    ```

-   消息接收接口

    消息接收需要实现IHttp2MessageCallback接口，并在连接或订阅Topic时传入MessageClient。具体接口如下：

    ```
    ConsumeAction Consume(Http2ConsumeMessage http2ConsumeMessage);
    ```

    该方法返回值决定了QoS1及QoS2消息是否回复的ACK。返回值说明如下：

    |返回值|说明|
    |:--|:-|
    |ConsumeAction.CommitSuccess|回执ACK。|
    |ConsumeAction.CommitFailure|不回执ACK。稍后会重新收到消息，需要手动调用MessageClient.DoAck\(\) 方法回复ACK。|

-   消息发送接口

    ```
    /// <summary>
    /// 发送消息到指定topic
    /// </summary>
    /// <returns>发送的消息</returns>
    /// <param name="topic">Topic.</param>
    /// <param name="message">传入参数</param>
    void DoPublish(string topic, Message message, PublishDelegate publishDelegate);
    ```


