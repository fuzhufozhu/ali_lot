# HTTP连接通信 {#concept_djd_vt5_wdb .concept}

物联网平台支持使用HTTP接入，目前仅支持HTTPS协议。本文介绍使用HTTP连接通信的接入流程。

## 限制说明 {#section_vbr_bb3_1gb .section}

-   HTTP通信方式适合单纯的数据上报的场景。

-   HTTP服务器地址为`https://iot-as-http.cn-shanghai.aliyuncs.com`。

-   目前，仅中国（上海）地域支持HTTP通信。

-   只支持HTTPS协议。

-   Topic规范和[MQTT的Topic规范](intl.zh-CN/设备端开发指南/设备多协议连接/MQTT接入——MQTT协议规范.md#)一致。使用HTTP协议连接，上报数据请求：`https://iot-as-http.cn-shanghai.aliyuncs.com/topic/${topic}`。其中，Topic变量`${topic}`的值可以与MQTT连接通信的Topic 相复用。不支持以`?query_String=xxx`格式传参。

-   数据上行接口传输的数据大小限制为128 KB。

-   HTTP请求只支持POST方式。

-   设备认证请求的HTTP header中的Content-Type必须为application/json。

-   数据上报请求的HTTP header中的Content-Type必须为application/octet-stream。

-   设备认证返回的token会在一定周期后失效（目前token有效期是7天），请务必考虑token失效逻辑的处理。


## 接入流程 {#section_knb_xt5_wdb .section}

接入流程主要包含进行设备认证以获取设备token和采用获取的token进行持续地数据上报。

1.  认证设备，获取设备的token。

    endpoint地址：`https://iot-as-http.cn-shanghai.aliyuncs.com`

    认证设备请求：

    ``` {#codeblock_ves_08s_v86}
    POST /auth HTTP/1.1
    Host: iot-as-http.cn-shanghai.aliyuncs.com
    Content-Type: application/json
    body: {"version":"default","clientId":"mylight1000002","signmethod":"hmacsha1","sign":"4870141D4067227128CBB4377906C3731CAC221C","productKey":"ZG1EvTEa7NN","deviceName":"NlwaSPXsCpTQuh8FxBGH","timestamp":"1501668289957"}
    ```

    |参数|说明|
    |:-|:-|
    |Method|请求方法。支持POST方法。|
    |URL|/auth，URL地址，只支持HTTPS。|
    |Host|endpoint地址：iot-as-http.cn-shanghai.aliyuncs.com|
    |Content-Type|设备发送给物联网平台的上行数据的编码格式。目前只支持 application/json。若使用其他编码格式，会返回参数错误。|
    |body|设备认证信息。JSON数据格式。具体信息，请参见下表[body参数](#)。|

    |字段名称|是否必需|说明|
    |:---|:---|:-|
    |productKey|是|设备所属产品的Key。可从物联网平台的控制台设备详情页获取。|
    |deviceName|是|设备名称。从物联网平台的控制台设备详情页获取。|
    |clientId|是|客户端ID。长度为64字符内，建议以MAC地址或SN码作为clientId。|
    |timestamp|否|时间戳。校验时间戳15分钟内的请求有效。时间戳格式为数值，值为自GMT 1970年1月1日0时0分到当前时间点所经过的毫秒数。|
    |sign|是|签名。 签名计算格式为`hmacmd5(DeviceSecret,content)`。

 其中，content为将所有提交给服务器的参数（除version、sign和signmethod外），按照英文字母升序，依次拼接排序（无拼接符号）的结果。

 签名示例：

 假设`clientId = 12345`，`deviceName = device`，`productKey = pk`，`timestamp = 789`，`signmethod=hmacsha1`，`deviceSecret=secret`，那么签名计算为`hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString();`。最后二进制数据转十六进制字符串，大小写不敏感。

 |
    |signmethod|否|算法类型，支持hmacmd5和hmacsha1。 若不传入此参数，则默认为hmacmd5。

 |
    |version|否|版本号。若不传入此参数，则默认default。|

    设备认证返回结果示例：

    ``` {#codeblock_pdv_qbb_e2f}
    body:
    {
      "code": 0,//业务状态码
      "message": "success",//业务信息
      "info": {
        "token":  "6944e5bfb92e4d4ea3918d1eda3942f6"
      }
    }
    ```

    **说明：** 

    -   请将返回的token值缓存到本地。
    -   每次上报数据时，都需要携带token信息。如果token失效，需要重新认证设备获取token。
    |code|message|备注|
    |:---|:------|:-|
    |10000|common error|未知错误。|
    |10001|param error|请求的参数异常。|
    |20000|auth check error|设备鉴权失败。|
    |20004|update session error|更新失败。|
    |40000|request too many|请求次数过多，流控限制。|

2.  上报数据。

    发送数据到某个Topic。

    若要发送数据到自定义Topic，需先在物联网平台的控制台，设备所属产品的产品详情页，Topic类列表栏中创建Topic类。请参见[自定义Topic](../../../../intl.zh-CN/用户指南/产品与设备/Topic/自定义Topic.md#)。

    如Topic为`/${YourProductKey}/${YourDeviceName}/pub`。假设当前设备名称为device123，产品Key为a1GFjLPXXXX，那么您可以调用 `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/a1GFjLPXXXX/device123/pub`地址来上报数据。

    上报数据请求：

    ``` {#codeblock_ssu_qy8_o7k}
    POST /topic/${topic} HTTP/1.1
    Host: iot-as-http.cn-shanghai.aliyuncs.com
    password:${token}
    Content-Type: application/octet-stream
    body: ${your_data}
    ```

    |参数|说明|
    |:-|:-|
    |Method|请求方法。支持POST方法。|
    |URL|`/topic/${topic}`。其中，变量`${topic}`需替换为数据发往的目标Topic。只支持HTTPS。|
    |Host|endpoint地址：iot-as-http.cn-shanghai.aliyuncs.com。|
    |password|放在Header中的参数，取值为调用设备认证接口auth返回的token值。|
    |Content-Type|设备发送给物联网平台的上行数据的编码格式。目前仅支持application/octet-stream。若使用其他编码格式，会返回参数错误。|
    |body|发往$\{topic\}的数据内容。|

    返回结果示例：

    ``` {#codeblock_dra_elx_pl0}
    body:
    {
      "code": 0,//业务状态码
      "message": "success",//业务信息
      "info": {
        "messageId": 892687627916247040,
      }
    }
    ```

    |code|message|备注|
    |:---|:------|:-|
    |10000|common error|未知错误。|
    |10001|param error|请求的参数异常。|
    |20001|token is expired|token失效。需重新调用auth进行鉴权，获取token。|
    |20002|token is null|请求header中无token信息。|
    |20003|check token error|根据token获取identify信息失败。需重新调用auth进行鉴权，获取token。|
    |30001|publish message error|数据上行失败。|
    |40000|request too many|请求次数过多，流控限制。|


