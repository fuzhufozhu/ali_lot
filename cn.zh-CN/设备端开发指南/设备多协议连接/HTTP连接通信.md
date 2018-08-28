# HTTP连接通信 {#concept_djd_vt5_wdb .concept}

## 接入说明 {#section_knb_xt5_wdb .section}

设备使用HTTP接入说明：

-   HTTP服务器地址endpoint = [https://iot-as-http.cn-shanghai.aliyuncs.com](https://iot-as-http.cn-shanghai.aliyuncs.com/)。
-   只支持HTTPS协议。
-   设备在发送数据前，首先发起认证，获取设备的token。
-   每次上报数据时，都需要携带token信息，如果token失效，需要重新认证获取token，token可以到缓存本地，有效期48小时。

## 设备认证\($\{endpoint\}/auth\) {#section_akb_155_wdb .section}

传输数据前，使用此接口获取token，只需要请求一次。

```

POST /auth HTTP/1.1
Host: iot-as-http.cn-shanghai.aliyuncs.com
Content-Type: application/json
body: {"version":"default","clientId":"mylight1000002","signmethod":"hmacsha1","sign":"4870141D4067227128CBB4377906C3731CAC221C","productKey":"ZG1EvTEa7NN","deviceName":"NlwaSPXsCpTQuh8FxBGH","timestamp":"1501668289957"}
```

参数说明如下：

-   Method: POST，支持POST方法。
-   URL: /auth，URL地址，只支持HTTPS。
-   Content-Type：目前只支持 application/json。

JSON数据格式，具体属性值如下：

|字段名称|是否必选|说明|
|----|----|--|
|productKey|必选|从物联网平台的控制台获取。|
|deviceName|必选|从物联网平台的控制台获取。|
|clientId|必选|客户端自表示Id,64字符内，建议是MAC或SN。|
|timestamp|可选|时间戳，校验时间戳15分钟内请求有效。|
|sign|必选|签名，格式为hmacmd5\(DeviceSecret,content\), content = 将所有提交给服务器的参数（version、sign和signmethod除外）, 按照字母顺序排序, 然后将参数值依次拼接，无拼接符号。|
|signmethod|可选|算法类型，hmacmd5或hmacsha1，不填默认hmacmd5。|
|version|可选|版本，不填默认default。|

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

业务状态码说明如下：

|code|message|备注|
|----|-------|--|
|10000|common error|未知错误。|
|10001|param error|请求的参数异常。|
|20000|auth check error|设备鉴权失败。|
|20004|update session error|更新失败。|
|40000|request too many|请求次数过多，流控限制。|

SDK使用IOT\_HTTP\_Init和IOT\_HTTP\_DeviceNameAuth来进行认证。

```

handle = IOT_HTTP_Init(&http_param);
if (NULL != handle) {
  IOT_HTTP_DeviceNameAuth(handle);
  HAL_Printf("IoTx HTTP Message Sent\r\n");
} else {
  HAL_Printf("IoTx HTTP init failed\r\n");
  return 0;
}
```

函数声明：

```

/**
* @brief Initialize the HTTP client
* This function initialize the data.
*
* @param [in] pInitParams: Specify the init param infomation.
*
* @retval NULL : Initialize failed.
* @retval NOT_NULL : The contex of HTTP client.
* @see None.
*/
void *IOT_HTTP_Init(iotx_http_param_t *pInitParams);
/**
* @brief Handle device name authentication with remote server.
*
* @param [in] handle: Pointer of context, specify the HTTP client.
*
* @retval 0 : Authenticate success.
* @retval -1 : Authenticate failed.
* @see iotx_err_t.
*/
int IOT_HTTP_DeviceNameAuth(void *handle);
```

## 上行数据\($\{endpoint\}/topic/$\{topic\}\) {#section_wwh_dhk_zdb .section}

发送数据到某个topic，$\{topic\}可以在物联网平台的控制台，选择**产品管理** \> **消息通信**进行设置，比如对于$\{topic\} `/ProductKey/${DeviceName}/pub`，如果设备名称为device123，该设备可以通过 [https://iot-as-http.cn-shanghai.aliyuncs.com/topic/productkey/device123/pub](https://iot-as-http.cn-shanghai.aliyuncs.com/topic/productkey/device123/pub) 地址来上报数据。

-   示例：

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
-   返回值：

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


## C版本SDK {#section_sfs_jkk_zdb .section}

SDK使用接口IOT\_HTTP\_SendMessage来发送和接收数据。

```

static int iotx_post_data_to_server(void *handle)
{
  int ret = -1;
  char path[IOTX_URI_MAX_LEN + 1] = {0};
  char rsp_buf[1024];
  iotx_http_t *iotx_http_context = (iotx_http_t *)handle;
  iotx_device_info_t *p_devinfo = iotx_http_context->p_devinfo;
  iotx_http_message_param_t msg_param;
  msg_param.request_payload = (char *)"{\"name\":\"hello world\"}";
  msg_param.response_payload = rsp_buf;
  msg_param.timeout_ms = iotx_http_context->timeout_ms;
  msg_param.request_payload_len = strlen(msg_param.request_payload) + 1;
  msg_param.response_payload_len = 1024;
  msg_param.topic_path = path;
  HAL_Snprintf(msg_param.topic_path, IOTX_URI_MAX_LEN, "/topic/%s/%s/update",
  p_devinfo->product_key, p_devinfo->device_name);
  if (0 == (ret = IOT_HTTP_SendMessage(iotx_http_context, &msg_param))) {
    HAL_Printf("message response is %s\r\n", msg_param.response_payload);
  } else {
    HAL_Printf("error\r\n");
  }
  return ret;
}
```

```

/**
* @brief Send a message with specific path to server.
* Client must authentication with server before send message.
*
* @param [in] handle: Pointer of contex, specify the HTTP client.
* @param [in] msg_param: Specify the topic path and http payload configuration.
*
* @retval 0 : Success.
* @retval -1 : Failed.
* @see iotx_err_t.
*/
int IOT_HTTP_SendMessage(void *handle, iotx_http_message_param_t *msg_param);
```

## C版本SDK其他接口 {#section_ar1_4kk_zdb .section}

IOT\_HTTP\_Disconnect关闭http连接，释放内存。

```

/**
* @brief close tcp connection from client to server.
*
* @param [in] handle: Pointer of contex, specify the HTTP client.
* @return None.
* @see None.
*/
void IOT_HTTP_Disconnect(void *handle);
```

## 限制条件及注意事项 {#section_mq3_rkk_zdb .section}

-   TOPIC规范和MQTT的TOPIC一致，HTTP协议内 [https://iot-as-http.cn-shanghai.aliyuncs.com/topic/$\{topic\}](https://iot-as-http.cn-shanghai.aliyuncs.com/topic/$%7Btopic%7D,%E8%BF%99%E4%B8%AA%E6%8E%A5%E5%8F%A3%E5%AF%B9%E4%BA%8E%E6%89%80%E6%9C%89$%7Btopic%7D%E5%92%8CMQTT) ，该接口对于所有$\{topic\}和MQTTtopic是可以复用的，不支持`?query_String=xxx`形式传参。
-   客户端缓存认证返回的Token，待Token失效进行重新认证时，刷新缓存。
-   上行接口传输的数据大小限制为128k。
-   HTTP通信仅支持华东2（上海）地域。

