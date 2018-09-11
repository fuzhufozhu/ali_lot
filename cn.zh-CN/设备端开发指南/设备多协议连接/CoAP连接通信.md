# CoAP连接通信 {#concept_gn3_kr5_wdb .concept}

## 基础流程 {#section_sf1_mr5_wdb .section}

CoAP协议适用在资源受限的低功耗设备上，尤其是NB-IoT的设备使用，基于CoAP协议将NB-IoT设备接入物联网平台的流程如下图所示。￼

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7504/15366706093114_zh-CN.png)

基础流程说明如下：

1.  设备端NB-IoT模块中，集成阿里云物联网平台SDK，厂商在物联网平台的控制台申请设备证书（ProductKey/DeviceName/DeviceSecret）并烧录到设备中。
2.  NB-IoT设备通过运营商的蜂窝网络进行入网，需要联系当地运营商，确保设备所属地区已经覆盖NB网络，并已具备NB-IoT入网能力。
3.  设备入网成功后，NB设备产生的流量数据及产生的费用数据，将由运营商的M2M平台管理，此部分平台能力由运营商提供。
4.  设备开发者可通过 CoAP/UDP 协议，将设备采集的实时数据上报到阿里云物联网平台，借助物联网平台，实现海量亿级设备的安全连接和数据管理能力，并可通过规则引擎，与阿里云的各类大数据产品、云数据库和报表系统打通，快速实现从连接到智能的跨越。
5.  物联网平台提供相关的数据开放接口和消息推送服务，可将数据转发到业务服务器中，实现设备资产与实际应用的快速集成。

## 使用C-SDK建立CoAP链接 {#section_uxd_vr5_wdb .section}

IoT提供了C版本SDK示例，可以参考[DEMO](https://help.aliyun.com/document_detail/42648.html)。

1.  SDK中使用IOT\_CoAP\_Init 和 IOT\_CoAP\_DeviceNameAuth与云端建立CoAP认证。

    示例代码：

    ```
    
    iotx_coap_context_t *p_ctx = NULL;
    p_ctx = IOT_CoAP_Init(&config);
    if (NULL != p_ctx) {
      IOT_CoAP_DeviceNameAuth(p_ctx);
      do {
        count ++;
        if (count == 11) {
          count = 1;
        }
        IOT_CoAP_Yield(p_ctx);
      } while (m_coap_client_running);
      IOT_CoAP_Deinit(&p_ctx);
    } else {
      HAL_Printf("IoTx CoAP init failed\r\n");
    }
    ```

    函数声明：

    ```
    
    /**
    * @brief Initialize the CoAP client.
    * This function initialize the data structures and network,
    * and create the DTLS session.
    *
    * @param [in] p_config: Specify the CoAP client parameter.
    *
    * @retval NULL : Initialize failed.
    * @retval NOT_NULL : The contex of CoAP client.
    * @see None.
    */
    iotx_coap_context_t *IOT_CoAP_Init(iotx_coap_config_t *p_config);
    
    /**
    * @brief Handle device name authentication with remote server.
    *
    * @param [in] p_context: Pointer of contex, specify the CoAP client.
    *
    * @retval IOTX_SUCCESS : Authenticate success.
    * @retval IOTX_ERR_SEND_MSG_FAILED : Send authentication message failed.
    * @retval IOTX_ERR_AUTH_FAILED : Authenticate failed or timeout.
    * @see iotx_ret_code_t.
    */
    int IOT_CoAP_DeviceNameAuth(iotx_coap_context_t *p_context);
    ```

2.  SDK使用接口IOT\_CoAP\_SendMessage发送数据，使用IOT\_CoAP\_GetMessagePayload和IOT\_CoAP\_GetMessageCode接收数据。

    示例代码：

    ```
    
    /* send data */
    static void iotx_post_data_to_server(void *param)
    {
      char path[IOTX_URI_MAX_LEN + 1] = {0};
      iotx_message_t message;
      iotx_deviceinfo_t devinfo;
      message.p_payload = (unsigned char *)"{\"name\":\"hello world\"}";
      message.payload_len = strlen("{\"name\":\"hello world\"}");
      message.resp_callback = iotx_response_handler;
      message.msg_type = IOTX_MESSAGE_CON;
      message.content_type = IOTX_CONTENT_TYPE_JSON;
      iotx_coap_context_t *p_ctx = (iotx_coap_context_t *)param;
      iotx_set_devinfo(&devinfo);
      snprintf(path, IOTX_URI_MAX_LEN, "/topic/%s/%s/update/", (char *)devinfo.product_key,
      (char *)devinfo.device_name);
      IOT_CoAP_SendMessage(p_ctx, path, &message);
    }
    
    /* receive data */
    static void iotx_response_handler(void *arg, void *p_response)
    {
      int len = 0;
      unsigned char *p_payload = NULL;
      iotx_coap_resp_code_t resp_code;
      IOT_CoAP_GetMessageCode(p_response, &resp_code);
      IOT_CoAP_GetMessagePayload(p_response, &p_payload, &len);
      HAL_Printf("[APPL]: Message response code: 0x%x\r\n", resp_code);
      HAL_Printf("[APPL]: Len: %d, Payload: %s, \r\n", len, p_payload);
    }
    ```

    函数声明：

    ```
    
    /** 
    * @brief Send a message with specific path to server.
    * Client must authentication with server before send message.
    *
    * @param [in] p_context : Pointer of contex, specify the CoAP client.
    * @param [in] p_path: Specify the path name.
    * @param [in] p_message: Message to be sent.
    *
    * @retval IOTX_SUCCESS : Send the message success.
    * @retval IOTX_ERR_MSG_TOO_LOOG : The message length is too long.
    * @retval IOTX_ERR_NOT_AUTHED : The client hasn't authenticated with server
    * @see iotx_ret_code_t.
    */
    int IOT_CoAP_SendMessage(iotx_coap_context_t *p_context, char *p_path, iotx_message_t *p_message);
    
    /**
    * @brief Retrieves the length and payload pointer of specified message.
    *
    * @param [in] p_message: Pointer to the message to get the payload. Should not be NULL.
    * @param [out] pp_payload: Pointer to the payload.
    * @param [out] p_len: Size of the payload.
    *
    * @retval IOTX_SUCCESS : Get the payload success.
    * @retval IOTX_ERR_INVALID_PARAM : Can't get the payload due to invalid parameter.
    * @see iotx_ret_code_t.
    **/
    int IOT_CoAP_GetMessagePayload(void *p_message, unsigned char **pp_payload, int *p_len);
    
    /**
    * @brief Get the response code from a CoAP message.
    *
    * @param [in] p_message: Pointer to the message to add the address information to.
    * Should not be NULL.
    * @param [out] p_resp_code: The response code.
    *
    * @retval IOTX_SUCCESS : When get the response code to message success.
    * @retval IOTX_ERR_INVALID_PARAM : Pointer to the message is NULL.
    * @see iotx_ret_code_t.
    **/
    int IOT_CoAP_GetMessageCode(void *p_message, iotx_coap_resp_code_t *p_resp_code);
    ```

3.  您还可以使用其他C-SDK接口：
    -   IOT\_CoAP\_Yield 接口接收数据。

        请在任何需要接收数据的地方调用这个API，如果系统允许，请起一个单独的线程，执行该接口。

        ```
        
        /**
        * @brief Handle CoAP response packet from remote server,
        * and process timeout request etc..
        *
        * @param [in] p_context : Pointer of contex, specify the CoAP client.
        *
        * @return status.
        * @see iotx_ret_code_t.
        */
        int IOT_CoAP_Yield(iotx_coap_context_t *p_context);
        ```

    -   IOT\_CoAP\_Deinit 接口释放内存。

        ```
        
        /**
        * @brief De-initialize the CoAP client.
        * This function release CoAP DTLS session.
        * and release the related resource. *
        * @param [in] p_context: Pointer of contex, specify the CoAP client.
        *
        * @return None.
        * @see None.
        */
        void IOT_CoAP_Deinit(iotx_coap_context_t **p_context);
        ```


具体使用请参考C-SDK中\\sample\\coap\\coap-example.c的实现。

## CoAP协议自主接入流程 {#section_owj_wr5_wdb .section}

1.  CoAP服务器地址`endpoint = ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com:${port}`。

    其中：

    -   $\{YourProductKey\}：请替换为您申请的产品Key；
    -   $\{port\}：为端口，使用对称加密时端口为5682，使用DTLS时端口为5684。
2.  目前支持DTLS和对称加密。
    -   如果使用DTLS，必须走安全通道，此时需下载[根证书](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt?spm=5176.doc30539.2.1.1MRvV5&file=root.crt)。
    -   如果使用对称加密，无需使用证书。
3.  设备在发送数据前，首先发起认证，获取设备的token。
4.  每次上报数据时，需要携带token信息，如果token失效，需要重新认证获取token。

## 使用DTLS自主接入 {#section_vlc_pdk_b2b .section}

1.  设备认证，此接口用于传输数据前获取token，只需要请求一次。

    ```
    POST /auth
    Host: ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com
    Port: 5684
    Accept: application/json or application/cbor
    Content-Format: application/json or application/cbor
    payload: {"productKey":"ZG1EvTEa7NN","deviceName":"NlwaSPXsCpTQuh8FxBGH","clientId":"mylight1000002","sign":"bccb3d2618afe74b3eab12b94042f87b"}
    ```

    参数说明如下：

    -   Method: POST，只支持POST方法。
    -   URL: /auth，URL地址。
    -   Accept：接收的数据编码方式，目前支持 application/json和application/cbor。
    -   Content-Format： 上行数据的编码格式，目前支持 application/json和application/cbor。
    -   payload内容，JSON数据格式，具体属性值如下：

        |字段名称|是否必需|说明|
        |----|----|--|
        |productKey|是|设备三元组信息中ProductKey的值，是物联网平台为产品颁发的全局唯一标识，从物联网平台的控制台获取。|
        |deviceName|是|设备三元组信息中DeviceName的值，在注册设备时自定义的或自动生成的设备名称，从物联网平台的控制台获取。|
        |ackMode|否|取值为：        -   0或不赋值：request/response 是携带模式。
        -   1：request/response 是分离模式。
|
        |sign|是|签名，格式为`hmacmd5(DeviceSecret,content)`， content为将所有提交给服务器的参数（version、sign、resources和signmethod除外）, 按照英文字母升序，依次拼接排序（无拼接符号）。|
        |signmethod|否|算法类型，支持hmacmd5或hmacsha1。|
        |clientId|是|表示客户端Id，64字符内。|
        |timestamp|否|时间戳，目前时间戳并不做窗口校验。|

    返回如下：

    ```
    response：{"token":"f13102810756432e85dfd351eeb41c04"}
    ```

    返回码说明如下：

    |Code|描述|Payload|备注|
    |----|--|-------|--|
    |2.05|Content|认证通过：Token对象|正确请求。|
    |4.00|Bad Request|no payload|请求发送的Payload非法。|
    |4.01|Unauthorized|no payload|未授权的请求。|
    |4.03|Forbidden|no payload|禁止的请求。|
    |4.04|Not Found|no payload|请求的路径不存在。|
    |4.05|Method Not Allowed|no payload|请求方法不是指定值。|
    |4.06|Not Acceptable|no payload|Accept不是指定的类型。|
    |4.15|Unsupported Content-Format|no payload|请求的content不是指定类型。|
    |5.00|Internal Server Error|no payload|auth服务器超时或错误。|

2.  上行数据\($\{yourendpoint\}/topic/$\{topic\}\)

    发送数据到某个Topic，$\{topic\}可以在物联网平台的控制台，**产品管理** \> **消息通信**页面进行设置。

    对于topic`/ProductKey/${YourDeviceName}/pub`，如果当前设备名称为device，那么您可以调用 `${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com:5684/topic/ProductKey/device/pub`地址来上报数据，目前只支持pub权限的topic用于数据上报。

    请求示例：

    ```
    POST /topic/${topic}
    Host: ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com
    Port: 5684
    Accept: application/json or application/cbor
    Content-Format: application/json or application/cbor
    payload: ${your_data}
    CustomOptions: number:2088(标识token)
    ```

    参数说明如下：

    -   Method： POST，支持POST方法。
    -   URL： /topic/$\{topic\}，$\{topic\}替换为当前设备对应的topic。
    -   Accept：接收的数据编码方式，目前只支持 application/json和application/cbor。
    -   Content-Format： 上行数据的编码格式，服务端不做校验。
    -   CustomOptions： 设备认证（Auth）获取到token值，Option Number使用2088。

## 使用对称加密自主接入 {#section_j5f_cq3_bfb .section}

1.  设备认证

    请求示例：

    ```
    POST /auth
    Host: ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com
    Port: 5682
    Accept: application/json or application/cbor
    Content-Format: application/json or application/cbor
    Option: 无
    payload: {"productKey":"a1NUjcVkHZS","deviceName":"ff1a11e7c08d4b3db2b1500d8e0e55","clientId":"a1NUjcVkHZS&ff1a11e7c08d4b3db2b1500d8e0e55","sign":"F9FD53EE0CD010FCA40D14A9FEAB81E0", "seq":"10"}
    ```

    其中，payload内容为JSON数据格式，具体属性值请见[payload参数说明](#)。

    返回如下：

    ```
    {"random":"ad2b3a5eb51d64f7","seqOffset":1,"token":"MZ8m37hp01w1SSqoDFzo0010500d00.ad2b"}
    ```

    返回参数说明如下：

    |字段名称|说明|
    |----|--|
    |random|用于后续上、下行加密，组成加密Key。|
    |seqOffset|认证seq偏移初始值。|
    |token|设备认证成功后返回的token值。|

2.  数据上行

    请求示例:

    ```
    POST /topic/${topic}
    Host: ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com
    Port: 5682
    Accept: application/json or application/cbor
    Content-Format: application/json or application/cbor
    payload: ${your_data}
    CustomOptions: number:2088(标识token), 2089(seq)
    ```

    参数说明如下：

    |字段名称|是否必需|说明|
    |----|----|--|
    |$\{topic\}|是|物联网平台数据上行topic。|
    |$\{YourProductKey\}|是|设备三元组信息中ProductKey的值。|
    |$\{your\_data\}|是|需要上传的数据，AES加密后的数据。AES加密说明如下：

    -   加密key， SHA256\(设备密钥+","+认证后返回随机数\)，截取中间32个字符（16个字节）。
    -   AES参数， 向量：543yhjy97ae7fyfg，模式：AES/CBC/PKCS5Padding，字符集UTF-8。
|
    |option|是|option值有2088和2089两种类型，说明如下：    -   2088：表示token，设备认证后返回的token值。
    -   2089：表示seq，比设备认证后返回的seqOffset值更大，且在认证生效周期内不重复的随机值。使用AES加密该值。
|

    option返回如下：

    ```
    number:2090(云端消息id)
    ```

    消息上行成功后，返回成功状态码，同时返回上传后的云端消息id。


## 限制条件及注意事项 {#section_ut1_xqj_zdb .section}

-   topic规范和MQTT topic一致，CoAP协议内 `coap://host:port/topic/${topic}`接口对于所有$\{topic\}和MQTT topic是可以复用的，不支持``?query_String=xxx``形式传参。
-   客户端缓存认证返回的token是请求的令牌。
-   传输的数据大小依赖于MTU的大小，最好在1K以内。
-   仅华东2（上海）地域支持CoAP通信。

