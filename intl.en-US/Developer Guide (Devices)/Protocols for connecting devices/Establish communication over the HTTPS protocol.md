# Establish communication over the HTTPS protocol {#concept_djd_vt5_wdb .concept}

IoT Platform supports HTTPS connections. It does not support HTTP connections.

## Description {#section_vbr_bb3_1gb .section}

-   The HTTPS server endpoint is `https://iot-as-http.cn-shanghai.aliyuncs.com`.
-   Currently, only the region cn-shanghai supports HTTPS connections.
-   Only the HTTPS protocol is supported.
-   The standards for HTTPS topics are the same as the standards for MQTT topics in [MQTT standard](reseller.en-US/Developer Guide (Devices)/Protocols for connecting devices/MQTT standard.md#). Devices send data to IoT Platform through `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/${topic}`. The value of $\{topic\} can be the same topic used in MQTT communications. You cannot specify parameters in the format of `? query_String=xxx`.
-   The size of data from devices is limited to 128 KB.

## Procedure {#section_knb_xt5_wdb .section}

1.  Connect to the HTTPS server.

    The endpoint address: `https://iot-as-http.cn-shanghai.aliyuncs.com`

2.  Authenticate the device to get the device token.

    Device authentication request message example:

    ```
    POST /auth HTTP/1.1
    Host: iot-as-http.cn-shanghai.aliyuncs.com
    Content-Type: application/json
    body: {"version":"default","clientId":"mylight1000002","signmethod":"hmacsha1","sign":"4870141D4067227128CBB4377906C3731CAC221C","productKey":"ZG1EvTEa7NN","deviceName":"NlwaSPXsCpTQuh8FxBGH","timestamp":"1501668289957"}
    ```

    |Parameter|Description|
    |:--------|:----------|
    |Method|The request method. The supported method is POST.|
    |URL|/auth URL address. Only HTTPS is supported.|
    |Host|The endpoint address: iot-as-http.cn-shanghai.aliyuncs.com|
    |Content-Type|The format of the data that the device sends to IoT Platform. Only application/json is supported.|
    |body|The device information for authentication, in JSON format. For more information, see the following table [Parameters in body](#).|

    |Parameter|Required?|Description|
    |:--------|:--------|:----------|
    |productKey|Yes|The unique identifier of the product to which the device belongs. You can obtain this information on the device details page in the IoT Platform console.|
    |deviceName|Yes|The device name. You can obtain this information on the device details page in the IoT Platform console.|
    |clientId|Yes|The device client ID. It can be any string up to 64 characters in length. We recommend that you use either the MAC address or the SN code as the clientId.|
    |timestamp|No|Timestamp. The request is valid within 15 minutes after the timestamp is created.|
    |sign|Yes|Signature.The signature algorithm is in the format of `hmacmd5(DeviceSecret,content)`.

The value of content is a string that is built by sorting and concatenating all the parameters \(except version, sign, and signmethod\) that need to be submitted to the server in alphabetical order, without any delimiters.

Signature example:

If `clientId = 12345`, `deviceName = device`, `productKey = pk`, `timestamp = 789`, `signmethod = hmacsha1`, and `deviceSecret = secret`, then the signature algorithm is `hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString();`. In this example, binary data will be converted to hexadecimal data.

 |
    |signmethod|No|The algorithm type. The supported types are hmacmd5 and hmacsha1.The default value is hmacmd5.

|
    |version|No|The version. If you leave this blank, the value is default.|

    Response example:

    ```
    body:
    {
      "code": 0, // the status code
      "message": "success", // the result message
      "info": {
        "token":  "6944e5bfb92e4d4ea3918d1eda3942f6"
      }
    }
    ```

    **Note:** 

    -   The returned token can be cached locally.
    -   Token information is required every time when the device reports data to IoT Platform. If the token is lost or expires, initiate a device authentication request again to obtain a new token.
    |Code|Message|Description|
    |:---|:------|:----------|
    |10000|common error|Unknown error.|
    |10001|param error|A parameter exception occurred during the request.|
    |20000|auth check error|An error occurred while authenticating the device.|
    |20004|update session error|An error occurred while updating the session.|
    |40000|request too many|Too many requests. The throttling policy limits the number of requests.|

3.  The device sends data to IoT Platform.

    The device send data to the specified topic.

    In the IoT Platform console, on the Topic Categories tab page of the product, you can create topic categories.

    For example, a topic category is `/${YourProductKey}/${YourDeviceName}/pub`. If a device name is device123, and its product key is a1GFjLP3xxC, the device sends data through `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/a1GFjLP3xxC/device123/pub`.

    Request message format:

    ```
    POST /topic/${topic} HTTP/1.1
    Host: iot-as-http.cn-shanghai.aliyuncs.com
    password:${token}
    Content-Type: application/octet-stream
    body: ${your_data}
    ```

    |Parameter|Description|
    |:--------|:----------|
    |Method|The request method. The supported request method is POST.|
    |URL|`/topic/${topic}`. Replace `${topic}` with the topic for receiving device data. Only HTTPS is supported.|
    |Host|The endpoint address: iot-as-http.cn-shanghai.aliyuncs.com|
    |password|This parameter is included in the request header. The value of this parameter is the token returned when using the auth interface to authenticate the device.|
    |body|The data content sent to $\{topic\}, which is in binary byte\[\] array format and encoded with UTF-8.|

    Response example:

    ```
    body:
    {
      "code": 0, // the status code
      "message": "success", // the result message
      "info": {
        "messageId": 892687627916247040,
        "Data": byte []/UTF-8 encoded data, and possibly empty
      }
    }
    ```

    |Code|Message|Description|
    |----|-------|-----------|
    |10000|common error|Unknown error.|
    |10001|param error|A parameter exception occurred during the request.|
    |20001|token is expired|The token is invalid. Call auth to authenticate the device again to obtain a new token.|
    |20002|token is null|The request header contains no token information.|
    |20003|check token error|Failed to identify the device based on the token. Call auth to authenticate the device again and obtain a new token.|
    |30001|publish message error|An error occurred while publishing data.|
    |40000|request too many|Too many requests. The throttling policy limits the number of requests.|


