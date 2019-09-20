# 基于MQTT通道设备动态注册 {#task_1545804 .task}

直连设备可通过MQTT通道进行动态注册，即使用一型一密连接认证方式连接物联网平台。设备需先基于TLS建立与物联网平台的连接，获取DeviceSecret后，再断开连接，并重新建立TCP连接进行通信。

-   在物联网平台上预注册设备，并获取设备所属产品的产品证书（ProductKey和ProductSecret）。预注册设备时，可以使用设备的MAC地址或SN序列号等作为DeviceName。
-   将产品证书烧录至设备固件中。
-   在物联网平台控制台上，设备所属产品的产品详情页，开通该产品的[一型一密](cn.zh-CN/设备端开发指南/设备安全认证/一型一密.md#)动态注册功能。

## 流程 {#section_b4t_u5f_5cl .section}

![](images/57853_zh-CN.jpeg)

1.  设备发送CONNECT报文，报文中包含动态注册参数，请求建立连接。 

    **说明：** 目前，动态注册只支持使用TLS建立连接，不支持TCP直连；动态注册时，云端不会校验MQTT连接的Keep Alive（保活时间），因此可以不用设置Keep Alive时间。

    MQTT连接域名：`${YourProductKey}.iot-as-mqtt.${YourRegionId}.aliyuncs.com:1883`。

    MQTT动态注册的CONNECT报文参数和取值结构如下。

    ``` {#codeblock_3xh_o1w_88f}
    mqttClientId: clientId+"|securemode=2,authType=register,random=xxxx,signmethod=xxx|"
    mqttUserName: deviceName+"&"+productKey
    mqttPassword: sign_hmac(productSecret,content) 
    ```

    -   mqttClientId组成结构：`clientId+"|securemode=2,authType=register,random=xxxx,signmethod=xxx|"` 

        参数取值中包含的详细参数如下表。

        |参数|说明|
        |--|--|
        |clientId|客户端ID。建议使用设备的MAC地址或SN码，长度在64字符内。|
        |securemode|安全模式。动态注册功能仅支持使用TLS通道连接，不支持TCP直连，因此必须设置为2 。|
        |authType|认证类型。动态注册请求中，此参数必须设置为register。|
        |random|随机数。您自定义随机数。|
        |signMethod|签名算法。目前支持hmacmd5、hmacsha1、hmacsha256。|

    -   mqttUserName组成结构：`deviceName+"&"+productKey` 

        示例：device1&al123456789。

    -   mqttPassword计算方法：`sign_hmac(productSecret,content)` 

        将提交给服务器的必需参数（deviceName、productKey、random）按字典排序和拼接后，通过signMethod指定的算法，使用产品的ProductSecret进行加签。

        其中，content的值是提交给服务器的必需参数和值（deviceName、productKey、random）按照字母顺序排序、拼接（无拼接符号）的字符串。

        示例：

        ``` {#codeblock_5g4_l88_c6i}
        hmac_sha1(h1nQFYPZS0mW****, deviceNamedevice1productKeyal123456789random123)
        ```

        将以上方法的计算结果作为Password。

2.  物联网平台返回CONNECT ACK。 

    返回0，则表示建连成功，即动态注册成功。

    建连失败，则需根据 ACK中返回的错误码，确定错误原因。

    设备发送连接请求后，物联网平台返回的结果状态码和说明如下表。

    |结果码|消息|说明|
    |---|--|--|
    |0|CONNECTION\_ACCEPTED|动态注册成功。|
    |2|IDENTIFIER\_REJECTED|参数错误。原因可能是：     -   必填参数缺失或格式错误。
    -   您使用了TCP直连注册。动态注册只能使用TLS通道。
 |
    |3|SERVER\_UNAVAILABLE|云端错误。请稍后再试。|
    |4|BAD\_USERNAME\_OR\_PASSWORD|动态注册失败，鉴权未通过。 请检查传入的mqttUserName和mqttPassword取值是否正确。

 |

3.  建立连接后，物联网平台使用Topic：/ext/register，推送设备证书信息，其中包含DeviceSecret。 

    **说明：** 设备无需订阅推送证书的Topic。

    物联网平台推送的消息payload格式如下：

    ``` {#codeblock_8t7_din_kvx}
    {
      "productKey" : "xxx",
      "deviceName" : "xxx",
      "deviceSecret" : "xxx"
    }
    ```

4.  设备收到证书信息后，保存证书信息，并断开当前MQTT连接。 

    设备可以通过发送DISCONNECT报文或直接断开TCP连接，断开当前连接。

    如果设备未断开此连接，15秒之后，物联网平台会主动断开连接。

    如果您使用Eclipse Paho MQTT客户端，设置`MqttConnectOptions.setAutomaticReconnect(false)` 关闭自动重连。否则，注册成功并TCP断连后，重连逻辑会发起新的动态注册请求。

5.  设备使用证书信息，再次发起MQTT连接请求，建立设备与物联网平台的连接，进行消息通信。详情请参见[MQTT-TCP连接通信](cn.zh-CN/设备端开发指南/使用开放协议自主接入/MQTT-TCP连接通信.md#)。

