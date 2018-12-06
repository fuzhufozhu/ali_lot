# MQTT-TCP连接通信 {#concept_mhv_ghm_b2b .concept}

本文档主要介绍基于TCP的MQTT连接，并提供了两种设备认证模式：MQTT客户端域名直连、使用HTTPS认证再连接模式。您可以使用参考本文自主接入设备。

**说明：** 在进行MQTT CONNECT协议设置时：

-   Connect指令中的KeepAlive时间为30至1200秒。心跳时间不在此区间内会拒绝连接。建议取值300秒以上。
-   如果同一个设备三元组，同时用于多个连接，可能导致客户端互相上下线。
-   MQTT默认开源SDK会自动重连，您可以通过日志服务查看设备行为。

## MQTT客户端域名直连 {#section_ivs_b3m_b2b .section}

1.  推荐使用TLS加密。如果使用，需要[下载根证书](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt)。
2.  使用MQTT客户端连接服务器。自主接入可以使用[开源MQTT客户端参考](https://github.com/mqtt/mqtt.github.io/wiki/libraries)；如果您对MQTT不了解，请参考 [http://mqtt.org](http://mqtt.org/) 相关文档。

    **说明：** 若使用第三方代码，阿里云不提供技术支持。

3.  MQTT 连接。

    |连接域名|`${YourProductKey}.iot-as-mqtt.${YourRegionId}.aliyuncs.com:1883`$\{YourProductKey\}请替换为您的产品key。

$\{YourRegionId\}请参考[地域和可用区](https://help.aliyun.com/document_detail/40654.html)替换为您的Region ID。

|
    |MQTT的Connect报文参数|     ```
mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
mqttUsername: deviceName+"&"+productKey
mqttPassword: sign_hmac(deviceSecret,content)
    ```

 sign签名需要把提交给服务器的参数按字典排序后，根据signmethod加签。

 content的值为提交给服务器的参数（ProductKey、DeviceName、timestamp和clientId），按照字母顺序排序, 然后将参数值依次拼接。

    -   clientId：表示客户端ID，建议使用设备的MAC地址或SN码，64字符内。
    -   timestamp：表示当前时间毫秒值，可以不传递。
    -   mqttClientId：格式中`||`内为扩展参数。
    -   signmethod：表示签名算法类型。支持hmacmd5，hmacsha1和hmacsha256，默认为hmacmd5。
    -   securemode：表示目前安全模式，可选值有2 （TLS直连模式）和3（TCP直连模式）。
 |
    |示例| 如果`clientId = 12345，deviceName = device， productKey = pk， timestamp = 789，signmethod=hmacsha1，deviceSecret=secret`，那么使用TCP方式提交给MQTT的参数如下：

     ```
mqttclientId=12345|securemode=3,signmethod=hmacsha1,timestamp=789|
mqttUsername=device&pk
mqttPassword=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString(); //最后是二进制转16制字符串，大小写不敏感。
    ```

 加密后的Password结果为：

     ```
FAFD82A3D602B37FB0FA8B7892F24A477F851A14
    ```

 |


## 使用HTTPS认证再连接模式 {#section_brp_33m_b2b .section}

1.  设备认证

    设备认证使用HTTPS，认证域名为：`https://iot-auth.${YourRegionId}.aliyuncs.com/auth/devicename`。其中，$\{YourRegionId\}请参考[地域和可用区](https://help.aliyun.com/document_detail/40654.html)替换为您的Region ID。

    -   认证请求参数信息：

        |参数|是否可选|获取方式|
        |--|----|----|
        |productKey|必选|ProductKey，从物联网平台的控制台获取。|
        |deviceName|必选|DeviceName，从物联网平台的控制台获取。|
        |sign|必选|签名，格式为hmacmd5\(deviceSecret,content\)，content将所有提交给服务器的参数（version、sign、resources和signmethod除外），按照字母顺序排序， 然后将参数值依次拼接，无拼接符号。|
        |signmethod|可选|算法类型。支持hmacmd5，hmacsha1和hmacsha256，默认为hmacmd5。|
        |clientId|必选|表示客户端ID，64字符内。|
        |timestamp|可选|时间戳，不做窗口校验。|
        |resources|可选|希望获取的资源描述，如MQTT。 多个资源名称用逗号隔开。|

    -   返回参数：

        |参数|是否必选|含义|
        |--|----|--|
        |iotId|必选|服务器颁发的一个连接标记，用于赋值给MQTT connect报文中的username。|
        |iotToken|必选|token有效期为7天，赋值给MQTT connect报文中的password。|
        |resources|可选|资源信息，扩展信息比如MQTT服务器地址和CA证书信息等。|

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

        -   401: request auth error：在这个场景中，通常在签名匹配不通过时返回。
        -   460: param error：参数异常。
        -   500: unknow error：一些未知异常。
        -   5001: meta device not found：指定的设备不存在。
        -   6200: auth type mismatch：未授权认证类型错误。

## MQTT保活 {#section_x45_pjm_b2b .section}

设备端在保活时间间隔内，至少需要发送一次报文，包括ping请求。

如果服务端在保活时间内无法收到任何报文，物联网平台会断开连接，设备端需要进行重连。

设置保活时间的取值，取值范围为30至1200秒，建议取值300秒以上。

