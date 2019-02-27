# CoAP连接通信 {#concept_gn3_kr5_wdb .concept}

物联网平台支持CoAP协议连接通信。CoAP协议适用在资源受限的低功耗设备上，尤其是NB-IoT的设备使用。本文介绍基于CoAP协议进行设备接入的流程，及使用DTLS和对称加密两种认证方式下的自主接入流程。

## 基础流程 {#section_sf1_mr5_wdb .section}

基于CoAP协议将NB-IoT设备接入物联网平台的流程如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7504/15512388793114_zh-CN.png)

基础流程说明如下：

1.  在设备端NB-IoT模块中，集成阿里云物联网平台SDK。厂商在物联网平台的控制台申请设备证书（ProductKey、DeviceName和DeviceSecret）并烧录到设备中。
2.  NB-IoT设备通过运营商的蜂窝网络进行入网。需要联系当地运营商，确保设备所属地区已经覆盖NB网络，并已具备NB-IoT入网能力。
3.  设备入网成功后，NB设备产生的流量数据及产生的费用数据，将由运营商的M2M平台管理。此部分平台能力由运营商提供。
4.  设备开发者可通过 CoAP/UDP 协议，将设备采集的实时数据上报到阿里云物联网平台，借助物联网平台，实现海量亿级设备的安全连接和数据管理能力。并且，可通过规则引擎，将数据转发至阿里云的大数据产品、云数据库、表格存储等服务中进行处理。
5.  物联网平台提供相关的数据开放接口和消息推送服务，可将数据转发到业务服务器中，实现设备资产与实际应用的快速集成。

## 使用对称加密自主接入 {#section_j5f_cq3_bfb .section}

1.  连接CoAP服务器，endpoint地址为`${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com:${port}`。

    其中：

    -   `${YourProductKey}`：请替换为您申请的产品Key。
    -   `${port}`：端口。使用对称加密时端口为5682。
2.  设备认证。

    设备认证请求：

    ```
    POST /auth
    Host: ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com
    Port: 5682
    Accept: application/json or application/cbor
    Content-Format: application/json or application/cbor
    payload: {"productKey":"a1NUjcVkHZS","deviceName":"ff1a11e7c08d4b3db2b1500d8e0e55","clientId":"a1NUjcVkHZS&ff1a11e7c08d4b3db2b1500d8e0e55","sign":"F9FD53EE0CD010FCA40D14A9FEAB81E0", "seq":"10"}
    ```

    |参数|说明|
    |:-|:-|
    |Method|请求方法。只支持POST方法。|
    |URL|URL地址，取值：/auth。|
    |Host|endpoint地址。取值格式：`$\{YourProductKey\}.coap.cn-shanghai.link.aliyuncs.com`。其中，变量$\{YourProductKey\}需替换为您的产品Key。|
    |Port|端口，取值：5682。|
    |Accept|设备接收的数据编码方式。目前，支持两种方式：application/json和application/cbor。|
    |Content-Format|设备发送给物联网平台的上行数据的编码格式，目前，支持两种方式： application/json和application/cbor。|
    |payload|设备认证信息内容，JSON数据格式。具体参数，请参见下表[payload 说明](#)。|

    |字段名称|是否必需|说明|
    |:---|:---|:-|
    |productKey|是|设备三元组信息中ProductKey的值，是物联网平台为产品颁发的全局唯一标识。从物联网平台的控制台获取。|
    |deviceName|是|设备三元组信息中DeviceName的值，在注册设备时自定义的或自动生成的设备名称。从物联网平台的控制台获取。|
    |ackMode|否|通信模式。取值：    -   0：request/response 是携带模式，即客户端发送请求到服务端后，服务端处理完业务，回复业务数据和ACK。
    -   1：request/response 是分离模式，即客户端发送请求到服务端后，服务端先回复一个确认ACK，然后再处理业务后，回复业务数据。
若不传入此参数，则默认为携带模式。

|
    |sign|是|签名。签名计算格式为`hmacmd5(DeviceSecret,content)`。

其中，content为将所有提交给服务器的参数（除version、sign、resources和signmethod外），按照英文字母升序，依次拼接排序（无拼接符号）。

签名计算示例：

    ```
sign= hmac_md5(deviceSecret, clientId123deviceNametestproductKey123seq123timestamp1524448722000)
    ```

 |
    |signmethod|否|算法类型，支持hmacmd5和hmacsha1。|
    |clientId|是|客户端ID，长度需在64字符内。建议使用设备的的MAC地址或SN码作为clientId的值。|
    |timestamp|否|时间戳。目前，时间戳不做时间窗口校验。|

    返回结果示例：

    ```
    {"random":"ad2b3a5eb51d64f7","seqOffset":1,"token":"MZ8m37hp01w1SSqoDFzo0010500d00.ad2b"}
    ```

    |字段名称|说明|
    |----|--|
    |random|用于后续上、下行加密，组成加密Key。|
    |seqOffset|认证seq偏移初始值。|
    |token|设备认证成功后，返回的token值。|

3.  上报数据。

    上报数据请求:

    ```
    POST /topic/${topic}
    Host: ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com
    Port: 5682
    Accept: application/json or application/cbor
    Content-Format: application/json or application/cbor
    payload: ${your_data}
    CustomOptions: number:2088(标识token), 2089(seq)
    ```

    |字段名称|是否必需|说明|
    |----|----|--|
    |Method|是|请求方法。支持POST方法。|
    |URL|是|传入格式：`/topic/${topic}`。其中，变量$\{topic\}需替换为设备数据上行Topic。|
    |Host|是|endpoint地址。传入格式：`${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com`。其中，$\{YourProductKey\}需替换为设备所属产品的Key。|
    |Port|是|端口。取值：5682。|
    |Accept|是|设备接收的数据编码方式。目前，支持两种方式：application/json和application/cbor。|
    |Content-Format|是|上行数据的编码格式，服务端对此不做校验。目前，支持两种方式：application/json和application/cbor。|
    |payload|是|待上传的数据经高级加密标准（AES）加密后的数据。|
    |CustomOptions|是|option值有2088和2089两种类型，说明如下：    -   2088：表示token，取值为设备认证后返回的token值。

**说明：** 每次上报数据都需要携带token信息。如果token失效，需重新认证获取token。

    -   2089：表示seq，取值需比设备认证后返回的seqOffset值更大，且在认证生效周期内不重复的随机值。使用AES加密该值。
option返回示例：

    ```
number:2090(云端消息ID)
    ```

|

    消息上行成功后，返回成功状态码，同时返回物联网平台生成的消息ID。


## 使用DTLS自主接入 {#section_vlc_pdk_b2b .section}

1.  连接CoAP服务器，endpoint 地址为`${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com:${port}`。

    其中：

    -   $\{YourProductKey\}：请替换为您申请的产品Key。
    -   $\{port\}：端口。使用DTLS时，端口为5684。
2.  使用DTLS安全通道，需下载[根证书](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt?spm=5176.doc30539.2.1.1MRvV5&file=root.crt)。
3.  设备认证。使用auth接口认证设备，获取token。上报数据时，需携带token信息。

    设备认证请求：

    ```
    POST /auth
    Host: ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com
    Port: 5684
    Accept: application/json or application/cbor
    Content-Format: application/json or application/cbor
    payload: {"productKey":"ZG1EvTEa7NN","deviceName":"NlwaSPXsCpTQuh8FxBGH","clientId":"mylight1000002","sign":"bccb3d2618afe74b3eab12b94042f87b"}
    ```

    参数（除 Port参数外。使用DTLS时的Port为 5684）及payload内容说明，可参见[参数说明](#)中。

    返回结果示例：

    ```
    response：{"token":"f13102810756432e85dfd351eeb41c04"}
    ```

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

4.  上行数据。

    设备发送数据到某个Topic。

    自定义Topic，在物联网平台的控制台，设备所属产品的产品详情页的Topic类列表栏中进行创建。

    目前，只支持发布权限的Topic用于数据上报，如`/${YourProductKey}/${YourDeviceName}/pub`。假设当前设备名称为device，产品Key为a1GFjLP3xxC，那么您可以调用 `a1GFjLP3xxC.coap.cn-shanghai.link.aliyuncs.com:5684/topic/a1GFjLP3xxC/device/pub`地址来上报数据。

    上报数据请求：

    ```
    POST /topic/${topic}
    Host: ${YourProductKey}.coap.cn-shanghai.link.aliyuncs.com
    Port: 5684
    Accept: application/json or application/cbor
    Content-Format: application/json or application/cbor
    payload: ${your_data}
    CustomOptions: number:2088(标识token)
    ```

    |参数|是否必需|说明|
    |:-|:---|:-|
    |Method|是|请求方法。支持POST方法。|
    |URL|是|`/topic/${topic}`。其中，变量`${topic}`需替换为当前设备对应的Topic。|
    |Host|是|endpoint地址。取值格式：`$\{YourProductKey\}.coap.cn-shanghai.link.aliyuncs.com`。其中，变量$\{YourProductKey\}需替换为您的产品Key。|
    |Port|是|端口，取值：5684。|
    |Accept|是|设备接收的数据编码方式。目前，支持两种方式：application/json和application/cbor。|
    |Content-Format|是|上行数据的编码格式，服务端对此不做校验。目前，支持两种方式：application/json和application/cbor。|
    |CustomOptions|是|     -   number取值：2088。
    -   token为[设备认证（auth）](#)返回的token值。
 **说明：** 每次上报数据都需要携带token信息。如果token失效，需重新进行设备认证获取token。

 |


