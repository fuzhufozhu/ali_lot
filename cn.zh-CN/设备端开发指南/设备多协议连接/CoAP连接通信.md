# CoAP连接通信 {#concept_gn3_kr5_wdb .concept}

物联网平台支持CoAP协议连接通信。CoAP协议适用在资源受限的低功耗设备上，尤其是NB-IoT的设备使用。本文介绍了基于CoAP协议进行设备接入的流程，及使用DTLS和对称加密两种认证方式下的自主接入流程。

## 基础流程 {#section_sf1_mr5_wdb .section}

基于CoAP协议将NB-IoT设备接入物联网平台的流程如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7504/15433977283114_zh-CN.png)

基础流程说明如下：

1.  设备端NB-IoT模块中，集成阿里云物联网平台SDK，厂商在物联网平台的控制台申请设备证书（ProductKey、DeviceName和DeviceSecret）并烧录到设备中。
2.  NB-IoT设备通过运营商的蜂窝网络进行入网，需要联系当地运营商，确保设备所属地区已经覆盖NB网络，并已具备NB-IoT入网能力。
3.  设备入网成功后，NB设备产生的流量数据及产生的费用数据，将由运营商的M2M平台管理，此部分平台能力由运营商提供。
4.  设备开发者可通过 CoAP/UDP 协议，将设备采集的实时数据上报到阿里云物联网平台，借助物联网平台，实现海量亿级设备的安全连接和数据管理能力，并可通过规则引擎，与阿里云的各类大数据产品、云数据库和报表系统打通，快速实现从连接到智能的跨越。
5.  物联网平台提供相关的数据开放接口和消息推送服务，可将数据转发到业务服务器中，实现设备资产与实际应用的快速集成。

## 使用DTLS自主接入 {#section_vlc_pdk_b2b .section}

1.  连接CoAP服务器，地址为`endpoint = ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com:${port}`。

    其中：

    -   `${YourProductKey}`：请替换为您申请的产品Key。
    -   `${port}`：端口。使用DTLS时端口为5684。
2.  如果使用DTLS，必须走安全通道，此时需下载[根证书](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt?spm=5176.doc30539.2.1.1MRvV5&file=root.crt)。
3.  设备认证，此接口用于传输数据前获取token，只需要请求一次。

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
        |:---|:---|:-|
        |productKey|是|设备三元组信息中ProductKey的值，是物联网平台为产品颁发的全局唯一标识，从物联网平台的控制台获取。|
        |deviceName|是|设备三元组信息中DeviceName的值，在注册设备时自定义的或自动生成的设备名称，从物联网平台的控制台获取。|
        |ackMode|否|取值为：        -   0或不赋值：request/response 是携带模式。
        -   1：request/response 是分离模式。
|
        |sign|是|签名，格式为`hmacmd5(DeviceSecret,content)`， content为将所有提交给服务器的参数（version、sign、resources和signmethod除外），按照英文字母升序，依次拼接排序（无拼接符号）。|
        |signmethod|否|算法类型，支持hmacmd5或hmacsha1。|
        |clientId|是|表示客户端ID，64字符内。|
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

4.  上行数据\(`${yourendpoint}/topic/${topic}`\)

    发送数据到某个Topic，`${topic}`可以在物联网平台的控制台**产品管理** \> **消息通信**页面进行设置。

    对于Topic`/ProductKey/${YourDeviceName}/pub`，如果当前设备名称为device，那么您可以调用 `${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com:5684/topic/ProductKey/device/pub`地址来上报数据，目前只支持pub权限的Topic用于数据上报。

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
    -   URL： `/topic/${topic}`，`${topic}`替换为当前设备对应的topic。
    -   Accept：接收的数据编码方式，目前只支持 application/json和application/cbor。
    -   Content-Format： 上行数据的编码格式，服务端不做校验。
    -   CustomOptions： 设备认证（Auth）获取到token值，Option Number使用2088。

        **说明：** 每次上报数据都需要携带token信息。如果token失效，需重新认证获取token。


## 使用对称加密自主接入 {#section_j5f_cq3_bfb .section}

1.  连接CoAP服务器，地址为`endpoint = ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com:${port}`。

    其中：

    -   `${YourProductKey}`：请替换为您申请的产品Key。
    -   `${port}`：为端口。使用对称加密时端口为5682。
2.  设备认证

    请求示例：

    ```
    POST /auth
    Host: ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com
    Port: 5682
    Accept: application/json or application/cbor
    Content-Format: application/json or application/cbor
    payload: {"productKey":"a1NUjcVkHZS","deviceName":"ff1a11e7c08d4b3db2b1500d8e0e55","clientId":"a1NUjcVkHZS&ff1a11e7c08d4b3db2b1500d8e0e55","sign":"F9FD53EE0CD010FCA40D14A9FEAB81E0", "seq":"10"}
    ```

    参数及payload内容说明，请参见[参数说明](#)。

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

3.  数据上行

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

**说明：** 每次上报数据都需要携带token信息。如果token失效，需重新认证获取token。

    -   2089：表示seq，比设备认证后返回的seqOffset值更大，且在认证生效周期内不重复的随机值。使用AES加密该值。
|

    option返回如下：

    ```
    number:2090(云端消息id)
    ```

    消息上行成功后，返回成功状态码，同时返回上传后的云端消息ID。


