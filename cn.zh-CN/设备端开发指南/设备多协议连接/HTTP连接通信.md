# HTTP连接通信 {#concept_djd_vt5_wdb .concept}

物联网平台支持使用HTTP接入，目前仅支持HTTPS协议。

## 接入流程 {#section_knb_xt5_wdb .section}

1.  连接HTTP服务器。

    `endpoint=https://iot-as-http.cn-shanghai.aliyuncs.com`

2.  设备在发送数据前，参考下文设备认证，发起认证，获取设备的token。
3.  参考下文上报数据。每次上报数据时，都需要携带token信息，如果token失效，需要重新认证获取token。

    **说明：** token可以缓存到本地，有效期48小时。


## 设备认证\($\{endpoint\}/auth\) {#section_akb_155_wdb .section}

传输数据前，使用此接口获取token，只需要请求一次。

```
POST /auth HTTP/1.1
Host: iot-as-http.cn-shanghai.aliyuncs.com
Content-Type: application/json
body: {"version":"default","clientId":"mylight1000002","signmethod":"hmacsha1","sign":"4870141D4067227128CBB4377906C3731CAC221C","productKey":"ZG1EvTEa7NN","deviceName":"NlwaSPXsCpTQuh8FxBGH","timestamp":"1501668289957"}
```

参数说明：

-   Method: POST，支持POST方法。
-   URL: /auth，URL地址，只支持HTTPS。
-   Content-Type：目前只支持 application/json。
-   body：JSON数据格式，具体属性值如下：

|字段名称|是否必需|说明|
|:---|:---|:-|
|productKey|是|从物联网平台的控制台获取。|
|deviceName|是|从物联网平台的控制台获取。|
|clientId|是|客户端自表示Id,64字符内，建议是MAC或SN。|
|timestamp|否|时间戳，校验时间戳15分钟内请求有效。|
|sign|是|签名，格式为hmacmd5\(DeviceSecret,content\), content = 将所有提交给服务器的参数（version、sign和signmethod除外）, 按照字母顺序排序, 然后将参数值依次拼接，无拼接符号。|
|signmethod|否|算法类型，hmacmd5或hmacsha1，不填默认hmacmd5。|
|version|否|版本，不填默认default。|

    签名示例：

    如果`clientId = 12345`，`deviceName = device`，`productKey = pk`，`timestamp = 789`，`signmethod=hmacsha1`，`deviceSecret=secret`，那么`sign=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString();`，最后二进制转十六进制字符串，大小写不敏感。


返回如下：

```
body:
{
  "code": 0,//业务状态码
  "message": "success",//业务信息
  "info": {
    "token":  "eyJ0eXBlIjoiSldUIiwiYWxnIjoiaG1hY3NoYTEifQ.eyJleHBpcmUiOjE1MDI1MzE1MDc0NzcsInRva2VuIjoiODA0ZmFjYTBiZTE3NGUxNjliZjY0ODVlNWNiNDg3MTkifQ.OjMwu29F0CY2YR_6oOyiOLXz0c8"
  }
}
```

业务状态码说明：

|code|message|备注|
|:---|:------|:-|
|10000|common error|未知错误。|
|10001|param error|请求的参数异常。|
|20000|auth check error|设备鉴权失败。|
|20004|update session error|更新失败。|
|40000|request too many|请求次数过多，流控限制。|

## 上行数据\($\{endpoint\}/topic/$\{topic\}\) {#section_wwh_dhk_zdb .section}

发送数据到某个Topic。

$\{topic\}可以在物联网平台的控制台，选择**产品管理** \> **消息通信**进行设置。

比如对于$\{topic\} `/ProductKey/${DeviceName}/pub`，如果设备名称为device123，该设备可以通过 `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/productkey/device123/pub` 地址来上报数据。

示例：

```
POST /topic/${topic} HTTP/1.1
Host: iot-as-http.cn-shanghai.aliyuncs.com
password:${token}
Content-Type: application/octet-stream
body: ${your_data}
```

参数说明：

-   Method： POST，支持POST方法。
-   URL： /topic/$\{topic\}，$\{topic\}替换为设备对应的topic，只支持HTTPS。
-   Content-Type：目前只支持 application/octet-stream。
-   password： 放在Header中的参数，$\{token\}为设备认证auth接口返回的token。
-   body内容：发往$\{topic\}的内容，二进制byte\[\]，utf-8编码。

    返回如下：

    ```
    body:
    {
      "code": 0,//业务状态码
      "message": "success",//业务信息
      "info": {
        "messageId": 892687627916247040,
        "data": byte[]//可能为空，utf-8编码
      }
    }
    ```

    业务状态码说明：

    |code|message|备注|
    |----|-------|--|
    |10000|common error|未知错误。|
    |10001|param error|请求的参数异常。|
    |20001|token is expired|token失效，需要重新调用auth，进行，鉴权，获取token。|
    |20002|token is null|请求header无token信息。|
    |20003|check token error|根据token获取identify信息失败，需要重新调用auth，进行鉴权获取token。|
    |30001|publish message error|数据上行失败。|
    |40000|request too many|请求次数过多，流控限制。|


## 限制条件及注意事项 {#section_mq3_rkk_zdb .section}

-   TOPIC规范和MQTT的TOPIC一致，HTTP协议内 `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/${topic}` ，该接口对于所有$\{topic\}和MQTT Topic是可以复用的，不支持`?query_String=xxx`形式传参。
-   客户端缓存认证返回的token，待token失效进行重新认证时，刷新缓存。
-   上行接口传输的数据大小限制为128k。

