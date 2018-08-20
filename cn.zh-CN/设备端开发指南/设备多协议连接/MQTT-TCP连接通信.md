# MQTT-TCP连接通信 {#concept_mhv_ghm_b2b .concept}

本篇文档主要介绍基于TCP的MQTT连接，提供了两种设备认证模式。

-   MQTT客户端域名直连，资源受限设备推荐。
-   HTTPS发送授权后，连接MQTT特殊增值服务，比如设备级别的引流。

**说明：** 在进行MQTT CONNECT协议设置时：

-   Connect指令中的KeepAlive不能小于30秒，否则会拒绝连接。建议值为\[60秒,300秒\]之间。
-   如果同一个设备三元组，同时用于多个连接，可能导致客户端互相上下线。
-   MQTT默认开源SDK会自动重连，您可以通过日志服务查看设备行为。

具体使用方式，请参见您下载的设备端SDK代码包中\\sample\\mqtt\\mqtt-example.c的示例。

## MQTT客户端域名直连 {#section_ivs_b3m_b2b .section}

-   不使用已有Demo

    完全使用开源MQTT包自主接入，可以参考以下流程：

    1.  如果使用TLS，需要 [下载根证书](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt)。
    2.  使用MQTT客户端连接服务器，自主接入可以使用[开源MQTT客户端参考](https://github.com/mqtt/mqtt.github.io/wiki/libraries)，如果您对MQTT不了解，请参考 [http://mqtt.org](http://mqtt.org/) 相关文档。

        **说明：** 若使用第三方代码，阿里云不提供技术支持。

    3.  MQTT 连接使用说明。

        -   连接域名：

            -   华东2节点：$\{YourProductKey\}.iot-as-mqtt.cn-shanghai.aliyuncs.com:1883
            -   美西节点：$\{YourProductKey\}.iot-as-mqtt.us-west-1.aliyuncs.com:1883
            -   新加坡节点：$\{YourProductKey\}.iot-as-mqtt.ap-southeast-1.aliyuncs.com:1883
            $\{YourProductKey\}请替换为您的产品key。

        -   mqtt的Connect报文参数：

            ```
            mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
            mqttUsername: deviceName+"&"+productKey
            mqttPassword: sign_hmac(deviceSecret,content)
            ```

            sign签名需要把以下参数按字典排序后，根据signmethod加签。

            content的值为提交给服务器的参数（ProductKey、DeviceName、timestamp和clientId），按照字母顺序排序, 然后将参数值依次拼接。

            -   clientId：表示客户端ID，建议使用设备的MAC地址或SN码，64字符内。
            -   timestamp：表示当前时间毫秒值，可以不传递。
            -   mqttClientId：格式中`||`内为扩展参数。
            -   signmethod：表示签名算法类型。
            -   securemode：表示目前安全模式，可选值有2 （TLS直连模式）和3（TCP直连模式）。
        示例：如果`clientId = 12345，deviceName = device， productKey = pk， timestamp = 789，signmethod=hmacsha1，deviceSecret=secret`，那么使用tcp方式提交给mqtt参数如下：

        ```
        mqttclientId=12345|securemode=3,signmethod=hmacsha1,timestamp=789|
        username=device&pk
        password=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString(); //最后是二进制转16制字符串，大小写不敏感。
        ```

        结果为：

        ```
        FAFD82A3D602B37FB0FA8B7892F24A477F851A14
        ```

        **说明：** 上面3个参数分别是mqtt Connect登录报文的mqttClientId、mqttUsername和mqttPasswrod。

-   使用已有Demo
    1.  在域名直连的情况下，才支持TCP的MQTT连接，将make.settings 中FEATURE\_MQTT\_DIRECT、FEATURE\_MQTT\_DIRECT和FEATURE\_MQTT\_DIRECT\_NOTLS的值设置修改成y。

        ```
        FEATURE_MQTT_DIRECT = y
        FEATURE_MQTT_DIRECT = y
        FEATURE_MQTT_DIRECT_NOTLS = y
        ```

    2.  在SDK中，直接调用IOT\_MQTT\_Construct完成与云端建立连接的过程。

        ```
        pclient = IOT_MQTT_Construct(&mqtt_params);
        if (NULL == pclient) {
        EXAMPLE_TRACE("MQTT construct failed");
        rc = -1;
        goto do_exit;
        }
        ```

        函数声明：

        ```
        /**
        * @brief Construct the MQTT client
        * This function initialize the data structures, establish MQTT connection.
        *
        * @param [in] pInitParams: specify the MQTT client parameter.
        *
        * @retval NULL : Construct failed.
        * @retval NOT_NULL : The handle of MQTT client.
        * @see None.
        */
        void *IOT_MQTT_Construct(iotx_mqtt_param_t *pInitParams);
        ```


## 使用HTTPS认证再连接模式 {#section_brp_33m_b2b .section}

1.  设备认证

    设备认证使用HTTPS，认证域名为：https://iot-auth.cn-shanghai.aliyuncs.com/auth/devicename。

    -   认证请求参数信息：

        |参数|是否可选|获取方式|
        |--|----|----|
        |productKey|必选|ProductKey，从物联网平台的控制台获取。|
        |deviceName|必选|DeviceName，从物联网平台的控制台获取。|
        |sign|必选|签名，格式为hmacmd5\(deviceSecret,content\)，content将所有提交给服务器的参数（version、sign、resources和signmethod除外）, 按照字母顺序排序， 然后将参数值依次拼接，无拼接符号。|
        |signmethod|可选|算法类型，hmacmd5或hmacsha1，默认为hmacmd5。|
        |clientId|必选|表示客户端ID，64字符内。|
        |timestamp|可选|时间戳，不做窗口校验。|
        |resources|可选|希望获取的资源描述，如mqtt。 多个资源名称用逗号隔开。|

    -   返回参数：

        |参数|是否必选|含义|
        |--|----|--|
        |iotId|必选|服务器颁发的一个连接标记，用于赋值给MQTT connect报文中的username。|
        |iotToken|必选|token有效期为7天，赋值给MQTT connect报文中的password。|
        |resources|可选|资源信息，扩展信息比如mqtt服务器地址和CA证书信息等。|

    -   x-www-form-urlencoded请求示例：

        ```
        POST /auth/devicename HTTP/1.1
        Host: iot-auth.cn-shanghai.aliyuncs.com
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 123
        productKey=123&sign=123&timestamp=123&version=default&clientId=123&resouces=mqtt&deviceName=test
        sign = hmac_md5(deviceSecret, clientId123deviceNametestproductKey123timestamp123)
        ```

    -   请求响应：

        ```
        HTTP/1.1 200 OK
        Server: Tengine
        Date: Wed, 29 Mar 2017 13:08:36 GMT
        Content-Type: application/json;charset=utf-8
        Connection: close
        {
             "code" : 200,
             "data" : {
                "iotId" : "42Ze0mk3556498a1AlTP",
                "iotToken" : "0d7fdeb9dc1f4344a2cc0d45edcb0bcb",
                "resources" : {
                    "mqtt" : {
                       "host" : "xxx.iot-as-mqtt.cn-shanghai.aliyuncs.com",
                       "port" : 1883
                }
              }
              },
              "message" : "success"
        }
        ```

2.  连接MQTT
    1.  下载物联网平台 [root.crt](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/30539/cn_zh/1495715052139/root.crt)，建议使用TLS1.2。
    2.  设备端连接阿里云MQTT服务器地址，认证返回的MQTT地址和端口，进行通信。
    3.  采用TLS建立连接，客户端验证服务器通过CA证书完成，服务器验证客户端通过MQTT协议体内connect报文中的username=iotId，password=iotToken，clientId=自定义设备标记（建议MAC或SN）。

        如果iotId或iotToken非法，mqtt connect失败，收到的connect ack为3。

        错误码说明如下：

        -   401: request auth error，在这个场景中，通常在签名匹配不通过时返回。
        -   460: param error，参数异常。
        -   500: unknow error，一些未知异常。
        -   5001: meta device not found，指定的设备不存在。
        -   6200: auth type mismatch，未授权认证类型错误。

如果您使用已有Demo，请将make.settings 中FEATURE\_MQTT\_DIRECT的值设置修改成n，再调用IOT\_MQTT\_Construct接口，完成认证再连接的整个过程。

```
FEATURE_MQTT_DIRECT = n
```

HTTPS认证的过程具体实现在：\\src\\system\\iotkit-system\\src\\guider.c 中 iotx\_guider\_authenticate。

## SDK API介绍 {#section_gf5_53m_b2b .section}

-   IOT\_MQTT\_Construct 与云端建立MQTT连接

    mqtt-example 程序发送一次消息后会自动退出，可以尝试以下任意一种方式实现长期在线。

    -   执行mqtt-example时，使用命令行`./mqtt-example loop`，设备会保持长期在线。
    -   修改demo代码，example的代码调用IOT\_MQTT\_Destroy，设备变成离线状态。如果希望设备一直处于上线状态，请去掉IOT\_MQTT\_Unregister 和IOT\_MQTT\_Destroy的部分，使用while 保持长连接状态。
    代码如下：

    ```
    while(1)
    {
    IOT_MQTT_Yield(pclient, 200); 
    HAL_SleepMs(100);
    }
    ```

    返回参数声明如下：

    ```
    /**
    * @brief Construct the MQTT client
    * This function initialize the data structures, establish MQTT connection.
    *
    * @param [in] pInitParams: specify the MQTT client parameter.
    *
    * @retval NULL : Construct failed.
    * @retval NOT_NULL : The handle of MQTT client.
    * @see None.
    */
    void *IOT_MQTT_Construct(iotx_mqtt_param_t *pInitParams);
    ```

-   IOT\_MQTT\_Subscribe 同云端订阅某个topic

    请保留topic\_filter的memory，callback topic\_handle\_func才能准确送达。

    代码如下：

    ```
    /* Subscribe the specific topic */
    rc = IOT_MQTT_Subscribe(pclient, TOPIC_DATA, IOTX_MQTT_QOS1, _demo_message_arrive, NULL);
    if (rc < 0) {
    IOT_MQTT_Destroy(&pclient);
    EXAMPLE_TRACE("IOT_MQTT_Subscribe() failed, rc = %d", rc);
    rc = -1;
    goto do_exit;
    }
    ```

    返回参数声明如下：

    ```
    /**
    * @brief Subscribe MQTT topic.
    *
    * @param [in] handle: specify the MQTT client.
    * @param [in] topic_filter: specify the topic filter.
    * @param [in] qos: specify the MQTT Requested QoS.
    * @param [in] topic_handle_func: specify the topic handle callback-function.
    * @param [in] pcontext: specify context. When call 'topic_handle_func', it will be passed back.
    *
    * @retval -1 : Subscribe failed.
    * @retval >=0 : Subscribe successful.
    The value is a unique ID of this request.
    The ID will be passed back when callback 'iotx_mqtt_param_t:handle_event'.
    * @see None.
    */
    int IOT_MQTT_Subscribe(void *handle,
    const char *topic_filter,
    iotx_mqtt_qos_t qos,
    iotx_mqtt_event_handle_func_fpt topic_handle_func,
    void *pcontext);
    ```

-   IOT\_MQTT\_Publish 发布信息到云端

    代码如下：

    ```
    /* Initialize topic information */
    memset(&topic_msg, 0x0, sizeof(iotx_mqtt_topic_info_t));
    strcpy(msg_pub, "message: hello! start!");
    topic_msg.qos = IOTX_MQTT_QOS1;
    topic_msg.retain = 0;
    topic_msg.dup = 0;
    topic_msg.payload = (void *)msg_pub;
    topic_msg.payload_len = strlen(msg_pub);
    rc = IOT_MQTT_Publish(pclient, TOPIC_DATA, &topic_msg);
    EXAMPLE_TRACE("rc = IOT_MQTT_Publish() = %d", rc);
    ```

    返回参数声明如下：

    ```
    /**
    * @brief Publish message to specific topic.
    *
    * @param [in] handle: specify the MQTT client.
    * @param [in] topic_name: specify the topic name.
    * @param [in] topic_msg: specify the topic message.
    *
    * @retval -1 : Publish failed.
    * @retval 0 : Publish successful, where QoS is 0.
    * @retval >0 : Publish successful, where QoS is >= 0.
    The value is a unique ID of this request.
    The ID will be passed back when callback 'iotx_mqtt_param_t:handle_event'.
    * @see None.
    */
    int IOT_MQTT_Publish(void *handle, const char *topic_name, iotx_mqtt_topic_info_pt topic_msg);
    ```

-   IOT\_MQTT\_Unsubscribe 取消云端订阅

    代码如下：

    ```
    IOT_MQTT_Unsubscribe(pclient, TOPIC_DATA);
    ```

    返回参数声明如下：

    ```
    /**
    * @brief Unsubscribe MQTT topic.
    *
    * @param [in] handle: specify the MQTT client.
    * @param [in] topic_filter: specify the topic filter.
    *
    * @retval -1 : Unsubscribe failed.
    * @retval >=0 : Unsubscribe successful.
    The value is a unique ID of this request.
    The ID will be passed back when callback 'iotx_mqtt_param_t:handle_event'.
    * @see None.
    */
    int IOT_MQTT_Unsubscribe(void *handle, const char *topic_filter);
    ```

-   IOT\_MQTT\_Yield 数据接收函数

    请在任何需要接收数据的地方调用这个API。如果系统允许，请起一个单独的线程，执行该接口。

    代码如下：

    ```
    /* handle the MQTT packet received from TCP or SSL connection */
    IOT_MQTT_Yield(pclient, 200);
    ```

    返回参数声明如下：

    ```
    /**
    * @brief Handle MQTT packet from remote server and process timeout request
    * which include the MQTT subscribe, unsubscribe, publish(QOS >= 1), reconnect, etc..
    *
    * @param [in] handle: specify the MQTT client.
    * @param [in] timeout_ms: specify the timeout in millisecond in this loop.
    *
    * @return status.
    * @see None.
    */
    int IOT_MQTT_Yield(void *handle, int timeout_ms);
    ```

-   IOT\_MQTT\_Destroy 销毁 MQTT连接，释放内存

    代码如下：

    ```
    IOT_MQTT_Destroy(&pclient);
    ```

    返回参数声明如下：

    ```
    /**
    * @brief Deconstruct the MQTT client
    * This function disconnect MQTT connection and release the related resource.
    *
    * @param [in] phandle: pointer of handle, specify the MQTT client.
    *
    * @retval 0 : Deconstruct success.
    * @retval -1 : Deconstruct failed.
    * @see None.
    */
    int IOT_MQTT_Destroy(void **phandle);
    ```

-   IOT\_MQTT\_CheckStateNormal 查看当前的连接状态

    该接口用于查询MQTT的连接状态。但是，该接口并不能立刻检测到设备断网，只会在有数据发送或是keepalive时才能侦测到disconnect。

    返回参数声明如下：

    ```
    /**
    * @brief check whether MQTT connection is established or not.
    *
    * @param [in] handle: specify the MQTT client.
    *
    * @retval true : MQTT in normal state.
    * @retval false : MQTT in abnormal state.
    * @see None.
    */
    int IOT_MQTT_CheckStateNormal(void *handle);
    ```


## MQTT保活 {#section_x45_pjm_b2b .section}

设备端在keepalive\_interval\_ms时间间隔内，至少需要发送一次报文，包括ping请求。

如果服务端在keepalive\_interval\_ms时间内无法收到任何报文，物联网平台会断开连接，设备端需要进行重连。

在IOT\_MQTT\_Construct函数可以设置keepalive\_interval\_ms的取值，物联网平台通过该取值作为心跳间隔时间。keepalive\_interval\_ms的取值范围是60000~300000。

