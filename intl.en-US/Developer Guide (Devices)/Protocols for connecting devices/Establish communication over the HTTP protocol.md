# Establish communication over the HTTP protocol {#concept_djd_vt5_wdb .concept}

## Descriptions: {#section_knb_xt5_wdb .section}

The description of communication over the HTTP protocol is as follows:

-   The HTTP server endpoint = https://iot-as-http.cn-shanghai.aliyuncs.com.
-   Only the HTTPS protocol is supported.
-   Before transferring data, the device initializes authentication to obtain an access token.
-   Each time a device publishes data to IoT Platform, the access token is required. If the token is invalid, the device must go through the authentication process again in order to obtain a new access token. Tokens can be cached locally for 48 hours.

## Device authentication \($\{endpoint\}/auth\) {#section_akb_155_wdb .section}

You call this operation to obtain an access token before transmitting data. You only need to perform this operation once unless the token becomes invalid.

```

POST /auth HTTP/1.1
Host: iot-as-http.cn-shanghai.aliyuncs.com
Content-Type: application/json
body: {"version":"default","clientId":"mylight1000002","signmethod":"hmacsha1","sign":"4870141D4067227128CBB4377906C3731CAC221C","productKey":"ZG1EvTEa7NN","deviceName":"NlwaSPXsCpTQuh8FxBGH","timestamp":"1501668289957"}
```

Parameters:

-   Method: POST. The POST method is supported.
-   URL: /auth. This is the URL address. This address only supports the HTTPS protocol.
-   Content-Type: Currently only application/json is supported.

The JSON data format has the following properties:

|Field name|Required|Description|
|----------|--------|-----------|
|ProductKey|Required|You can retrieve it in the IoT Platform console.|
|DeviceName|Required|You can retrieve it in the IoT Platform console.|
|ClientId|Required|The Client ID can be up to 64 characters. We recommend that you use either the device MAC address or serial number as the Client ID.|
|TimeStamp|Optional|The timestamp, which is used to verify that the request is valid within 15 minutes.|
|Sign|Required|The signature, the format is hmacmd5 \(deviceSecret,content\), where the content is  all parameters \(except version, sign, and signmethod\) in alphabetical order, and the parameters are in listed together in sequence without splicing symbols.|
|SignMethod|Optional|The algorithm type, set the value to hmacmd5 or hmacsha1. The default value is hmacmd5.|
|Version|Optional|If you do not set the version, the value is set to default.|

The output is as follows:

```

Body:
{
  "code": 0, // the status code
  "message": "success", // the message
  "Info ":{
    "token": "eyJ0eXBlIjoiSldUIiwiYWxnIjoiaG1hY3NoYTEifQ.eyJleHBpcmUiOjE1MDI1MzE1MDc0NzcsInRva2VuIjoiODA0ZmFjYTBiZTE3NGUxNjliZjY0ODVlNWNiNDg3MTkifQ.OjMwu29F0CY2YR_6oOyiOLXz0c8"
  }
}
```

Status codes

|Code|Message|Description|
|----|-------|-----------|
|10000|common error|Unknown errors.|
|10001|param error|An exception occurred while requesting the parameter.|
|20000|auth check error|An error occurred while authorizing the device.|
|20004|update session error|An error occurred while updating the session.|
|40000|request too many|The throttling policy limits the number of requests.|

SDKs use the IOT\_HTTP\_Init function and the IOT\_HTTP\_DeviceNameAuth function to authenticate devices.

```

handle = IOT_HTTP_Init(&http_param);
if (NULL ! = handle) {
  IOT_HTTP_DeviceNameAuth(handle);
  HAL_Printf("IoTx HTTP Message Sent\r\n");
} else {
  HAL_Printf("IoTx HTTP init failed\r\n");
  return 0;
}
```

Function declaration:

```

/**
* @ Initializes the HTTP client 
* This function initializes data.
*
* @ param [in] pInitParams: Specify the init param information.
*
* @ retval NULL: Initialization failed.
* @ Retval not_null: The context of HTTP client.
* @ see None.
*/
Void * IOT_HTTP_Init (iotx_http_param_t * pinitparams );
/**
* @ brief handle Device Name authentication with remote server.
*
* @ param [in] handle: Pointer to context, specify the HTTP client.
*
* @ retval 0: Authentication successful.
* @retval -1 : Authentication failed.
* @see iotx_err_t.
*/
int IOT_HTTP_DeviceNameAuth(void *handle);
```

## Uploaded data \($\{endpoint\}/topic/$\{topic \}\) {#section_wwh_dhk_zdb .section}

Transferring data to a specified topic, you can set $\{topic\} in the IoT Platform console by selecting**Products** \> **Communication.**For example, for $\{topic\} `/productkey/${deviceName}/pub`, if the device name is device123, the device can report data through `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/productkey/device123/pub`.

-   Example:

```

POST /topic/${topic} HTTP/1.1
Host: iot-as-http.cn-shanghai.aliyuncs.com
Password: ${token}
Content-Type: application/octet-stream
Body: ${your_data}
```

    Parameters:

    -   Method: POST. The POST method is supported.
    -   URL: /topic/$\{topic\}. Replace the $\{topic\} placeholder with the name of the specific device topic. This address only supports the HTTPS protocol.
    -   Content-Type: Currently only application/octet-stream is supported.
    -   Password: The $\{token\} access token, which is returned after device authentication. This parameter is placed in the header.
-   Body: The content sent to $\{topic\}, which is in binary byte with UTF-8 encoding.
-   Return value:

    ```
    
    Body:
    {
      "code": 0, // the status code
      "message": "success", // the message
      "Info ":{
        "MessageId": 892687627916247040,
        "Data": byte [] // It is UTF-8 encoding, can be empty.
      }
    }
    ```

    Status codes:

    |Code|Message|Description|
    |----|-------|-----------|
    |10000|common error|Unknown errors.|
    |10001|param error|An exception occurred while requesting the parameter.|
    |20001|token is expired|The access token is invalid. You need to obtain a new token by calling the auth operation for authentication.|
    |20002|token is null|The request header has no tokens.|
    |20003|check token error|An error occurred during authentication. You need to obtain a new token by calling the auth operation for authentication.|
    |30001|publish message error|An error occurred while uploading data.|
    |40000|request too many|The throttling policy limits the number of requests.|


## C SDK {#section_sfs_jkk_zdb .section}

The SDK uses the IOT\_HTTP\_SendMessag function to send and receive data.

```

static int iotx_post_data_to_server(void *handle)
{
  Int ret =-1;
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
* @brief Send a message with specific path to the server.
* Client must be authenticated by the server before sending message.
*
* @param [in] handle: pointer to context, specifies the HTTP client.
* @param [in] msg_param: Specifies the topic path and http payload configuration.
*
* @retval 0 : Successful.
* @retval -1 : Failed.
* @see iotx_err_t.
*/
int IOT_HTTP_SendMessage(void *handle, iotx_http_message_param_t *msg_param);
```

## Additional functions in the C SDK. {#section_ar1_4kk_zdb .section}

The IOT\_HTTP\_Disconnect function close the HTTP connection to clear the cache.

```

/**
* @brief Closes the TCP connection between the client and server.
*
* @param [in] handle: pointer to context, specifies the HTTP client.
* @return None.
* @ see None.
*/
void IOT_HTTP_Disconnect(void *handle);
```

## Restrictions {#section_mq3_rkk_zdb .section}

-   The specifications of topics based on the HTTP protocol and MQTT protocols are same. The operation `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/${topic}` can be reused for all topics based on the HTTP protocol and MQTT protocols. The operation can not set parameters using `? query_String=xxx`.
-   The client caches the token locally. When the token expires,  you need to obtain a new token. The new token will then be cached again.
-   The size of data transferred by the upload operation is limited to 128 KB.
-   Only China \(Shanghai\) region supports HTTP protocol communication.

