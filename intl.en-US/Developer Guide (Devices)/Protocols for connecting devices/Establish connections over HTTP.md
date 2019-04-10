# Establish connections over HTTP {#concept_djd_vt5_wdb .concept}

IoT Platform supports HTTP connections, and only the HTTPS protocol is supported. This topic describes how to connect devices to IoT Platform over HTTP.

## Restrictions {#section_vbr_bb3_1gb .section}

-   HTTP communications are applicable to simple data report scenarios.

-   The HTTP server endpoint is `https://iot-as-http.cn-shanghai.aliyuncs.com`.

-   Only the China \(Shanghai\) region supports HTTP communication.

-   Only the HTTPS protocol is supported.

-   The standards for HTTPS-based topics are the same as the standards for MQTT-based topics in [MQTT standards](reseller.en-US/Developer Guide (Devices)/Protocols for connecting devices/MQTT standard.md#). Devices connect to IoT Platform over HTTP and send data to IoT Platform by using `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/${topic}`. The value of `${topic}` can be the same topics used for MQTT communications. You cannot specify parameters in the format of `? query_String=xxx`.

-   The size of data from devices is limited to 128 KB.

-   Only POST method is supported.

-   The value of Content-Type in the HTTP header of an authentication request must be application/json.

-   The value of Content-Type in the HTTP header of an upstream data request must be application/octet-stream.

-   The token returned for the device authentication will expire after a certain period of time. Currently, the token is valid for seven days. Make sure that you understand any negative impact that token expiration will have on your business.


## Procedure {#section_knb_xt5_wdb .section}

The communication process includes performing device authentication to obtain a device token and using the obtained token for data reporting.

1.  Authenticate the device to obtain the device token.

    Endpoint: `https://iot-as-http.cn-shanghai.aliyuncs.com`

    Authentication request:

    ```
    POST /auth HTTP/1.1
    Host: iot-as-http.cn-shanghai.aliyuncs.com
    Content-Type application/json
    body: {"version":"default","clientId":"mylight1000002","signmethod":"hmacsha1","sign":"4870141D4067227128CBB4377906C3731CAC221C","productKey":"ZG1EvTEa7NN","deviceName":"NlwaSPXsCpTQuh8FxBGH","timestamp":"1501668289957"}
    ```

    |Parameter|Description|
    |:--------|:----------|
    |Method|The request method. The supported method is POST.|
    |URL|The URL of the /auth request. Only HTTPS is supported.|
    |Host|The endpoint: iot-as-http.cn-shanghai.aliyuncs.com.|
    |Content-Type|The encoding format of the upstream data that the device sends to IoT Platform. Only application/json is supported. If another encoding format is used, a parameter error is returned.|
    |body|The device information for authentication, in JSON format. For more information, see the following table [Fields in body](#).|

    |Field|Required?|Description|
    |:----|:--------|:----------|
    |productKey|Yes|The unique identifier of the product to which the device belongs. You can obtain this information from the Device Details page of the IoT platform console.|
    |deviceName|Yes|The device name. You can obtain this information from the Device Details page of the IoT platform console.|
    |clientId|Yes|The client ID, a string of up to 64 characters. We recommend that you use the MAC address or SN code as the client ID.|
    |timestamp|No|The timestamp. A request is valid within 15 minutes after the timestamp is created. The timestamp is in the format of numbers. The value is the number of milliseconds that have elapsed since 00:00, January 1, 1970 \(GMT\).|
    |sign|Yes|The signature value.The signature algorithm is in the format of `hmacmd5(deviceSecret,content)`.

The value of content is a string that contains all the parameters to be reported to IoT Platform except version, sign, and signmethod. These parameters are sorted in alphabetical order and spliced without any separators.

Signature example:

If `clientId=12345`, `deviceName=device`, `productKey=pk`, `timestamp=789`, `signmethod=hmacsha1`, and `deviceSecret=secret`, then the signature algorithm is `hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString();`. In this example, binary data will be converted to a case-insensitive hexadecimal string.

 |
    |signmethod|No|The algorithm type. The type can be hmacmd5 or hmacsha1.If you do not specify this parameter, the default value is hmacmd5.

|
    |version|No|The version number. If you do not specify this parameter, the value is "default".|

    Sample response:

    ```
    body:
    {
      "code": 0,// The status code
      "message": "success", // The message
      "Info": {
        "token":  "6944e5bfb92e4d4ea3918d1eda3942f6"
      }
    }
    ```

    **Note:** 

    -   Cache the returned token value locally.
    -   Token information is required each time when the device reports data to IoT Platform. If the token expires, you must re-authenticate the device to obtain a new token.
    |Code|Message|Description|
    |:---|:------|:----------|
    |10000|common error|Unknown error.|
    |10001|param error|A parameter error occurred.|
    |20000|auth check error|An error occurred while authenticating the device.|
    |20004|update session error|An error occurred while updating the session.|
    |40000|request too many|Too many requests. The throttling policy limits the number of requests.|

2.  Send data to IoT Platform.

    The device sends data to a specific topic.

    To send data to a custom topic, you must create a topic category on the Topic Categories tab page of the corresponding product in the IoT Platform console. For more information, see [Create a topic category](../../../../../reseller.en-US/User Guide/Create products and devices/Topics/Create a topic category.md#).

    For example, a topic category is `/${YourProductKey}/${YourDeviceName}/user/pub`. If the device name is device123, and its ProductKey is a1GFjLPXXXX, the device can send data through `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/a1GFjLPXXXX/device123/user/pub`.

    Upstream data request:

    ```
    POST /topic/${topic} HTTP/1.1
    Host: iot-as-http.cn-shanghai.aliyuncs.com
    password:${token}
    Content-Type: application/octet-stream
    body: ${your_data}
    ```

    |Parameter|Description|
    |:--------|:----------|
    |Method|The request method. The supported method is POST.|
    |URL|`/topic/${topic}`. Replace `${topic}` with the topic to which data is sent. Only HTTPS is supported.|
    |Host|The endpoint: iot-as-http.cn-shanghai.aliyuncs.com.|
    |password|This parameter is included in the request header. The value of this parameter is the token returned after calling auth to authenticate the device.|
    |Content-Type|The encoding format of the upstream data that the device sends to IoT Platform. Only application/octet-stream is supported. If another encoding format is used, a parameter error is returned.|
    |body|The data content sent to the target topic.|

    Sample response:

    ```
    body:
    {
      "code": 0, // The status code
      "message": "success", // The message
      "Info": {
        "messageId": 892687627916247040,
      }
    }
    ```

    |Code|Message|Description|
    |:---|:------|:----------|
    |10000|common error|Unknown error.|
    |10001|param error|A parameter error occurred.|
    |20001|token is expired|The token has expired. You must call auth to re-authenticate the device and obtain a new token.|
    |20002|token is null|The request header does not contain any token information.|
    |20003|check token error|An error occurred while obtaining identity information according to the token. You must call auth to re-authenticate the device and obtain a new token.|
    |30001|publish message error|An error occurred while reporting data.|
    |40000|request too many|Too many requests. The throttling policy limits the number of requests.|


