# Establish MQTT over TCP connections {#concept_mhv_ghm_b2b .concept}

This topic describes the TCP-based MQTT connection and provides two modes of device authentication.

-   MQTT clients directly connect to the specified domain names without providing additional device credential information. We recommend that you use this authentication mode for devices with limited resources.
-   After HTTPS authentication, connect to the special value-added services of MQTT, such as distributing communication traffic from devices among clusters.

**Note:** When you configure the MQTT CONNECT packet:

-   The keepalive value in the Connect command must be larger than 30 seconds, otherwise, the connection will be denied. We recommend that you set the value in the range of 60 to 300 seconds.
-   If multiple devices are connected using the same set of ProductKey, DeviceName, and DeviceSecret, some devices will be brought offline.
-   The default setting of the MQTT protocol is that open-source SDKs are automatically connected. You can view device behaviors using Log Service.

For more information, see the example of \\sample\\mqtt\\mqtt-example.c in device SDK code package that you downloaded.

## Direct connection to the MQTT client domain {#section_ivs_b3m_b2b .section}

-   Without an existing demo

    If you use the open-source MQTT package for access, see the following procedure:

    1.  We recommend that you use TLS to encrypt, because it provides higher security. If you are using TLS to encrypt, you need to [download a root certificate](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt).
    2.  If you want to access the server using an MQTT client, see [Open-source MQTT client references](https://github.com/mqtt/mqtt.github.io/wiki/libraries). For more information about the MQTT protocol, see [http://mqtt.org](http://mqtt.org/).

        **Note:** Alibaba Cloud does not provide technical support if you are using third-party code.

    3.  Instructions for MQTT connection

        -   Connection domain: `${YourProductKey}.iot-as-mqtt. ${YourRegionId}.aliyuncs.com:1883`

            Replace $\{YourProductKey\} with your product ID. Replace $\{YourRegionId\} with your device region ID. For the expressions of region IDs, see [Regions and zones](https://help.aliyun.com/document_detail/40654.html).

        -   The MQTT Connect packets include the following parameters:

            ```
            mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
            mqttUsername: deviceName+"&"+productKey
            mqttPassword: sign_hmac(deviceSecret,content)
            ```

            Sort the following parameters in alphabetical order and then encrypt the parameters based on the specified sign method.

            The value of content is the parameters sent to the server \(productKey, deviceName, timestamp, and clientId\). Sort these parameters in alphabetical order and then splice the parameters and parameter values.

            -   clientId: Indicates the client ID. We recommend that you use the MAC address or the serial number of the device as the client ID. The length of the client ID must be within 64 characters.
            -   timestamp: The current time in milliseconds.This parameter is optional.
            -   mqttClientId: The expanded parameters are in `||`.
            -   signmethod: Specifies a signature algorithm.
            -   securemode: The current security mode. Values include: 2 \(TLS direct connection\) and 3 \(TCP direct connection\).
        Example: If `clientId = 12345, deviceName = device, productKey = pk, timestamp = 789, signmethod=hmacsha1, deviceSecret=secret`, submit the MQTT parameters over TCP:

        ```
        mqttclientId=12345|securemode=3,signmethod=hmacsha1,timestamp=789|
        username=device&pk
        password=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString(); // The last parameter is a binary-to-hexadecimal string. Its case is insensitive.
        ```

        The result is:

        ```
        FAFD82A3D602B37FB0FA8B7892F24A477F851A14
        ```

        **Note:** The three parameters are: mqttClientId, mqttUsername, and mqttPassword of the MQTT Connect logon packets.

-   With an existing demo
    1.  An MQTT connection over TCP is supported only when you directly connect to a domain. Set the values of FEATURE\_MQTT\_DIRECT, FEATURE\_MQTT\_DIRECT, and FEATURE\_MQTT\_DIRECT\_NOTLS in make.settings to y.

        ```
        FEATURE_MQTT_DIRECT = y
        FEATURE_MQTT_DIRECT = y
        FEATURE_MQTT_DIRECT_NOTLS = y
        ```

    2.  In the SDK, call IOT\_MQTT\_Construct to connect to the cloud.

        ```
        pclient = IOT_MQTT_Construct(&mqtt_params);
        if (NULL == pclient) {
        EXAMPLE_TRACE("MQTT construct failed");
        rc = -1;
        goto do_exit;
        }
        ```

        The function declaration is as follows:

        ```
        /**
        * @brief Construct the MQTT client
        * This function initialize the data structures, establish MQTT connection.
        *
        * @param [in] pInitParams: specify the MQTT client parameter.
        *
        * @retval NULL : Construct failed.
        * @retval NOT_NULL : The handle of MQTT client.
        * @see None.
        */
        void *IOT_MQTT_Construct(iotx_mqtt_param_t *pInitParams);
        ```


## Connect after the HTTPS authentication {#section_brp_33m_b2b .section}

1.  Authenticate devices

    Use HTTPS for device authentication. The authentication URL is `https://iot-auth. ${YourRegionId}.aliyuncs.com/auth/devicename`. Replace $\{YourRegionId\} with your device region ID. For the expressions of region IDs, see [Regions and zones](https://help.aliyun.com/document_detail/40654.html).

    -   The authentication request parameters are as follows:

        |Parameter|Required|Description|
        |---------|--------|-----------|
        |productKey|Yes|ProductKey. You can view the ProductKey in the IoT Platform console.|
        |deviceName|Yes|DeviceName. You can view the DeviceName in the IoT Platform console.|
        |sign|Yes|Signature. The authentication format is HMAC-MD5 for deviceSecret and content. In the content, all parameters submitted to the server \(except for version, sign, resources, and signmethod\) are sorted alphabetically. Then, the parameter values are spliced with the parameter names without using any splice character.|
        |signmethod|No|The algorithm type, such as HMAC-MD5 or HMAC-SHA1. The default type is HMAC-MD5.|
        |clientId|Yes|The client ID. Its length must be within 64 characters.|
        |timestamp|No|The timestamp. Serial port validation is not required.|
        |resources|No|The resource description that you want to obtain, such as MQTT. Multiple resource names are separated by commas.|

    -   Response parameters:

        |Parameter|Required|Description|
        |---------|--------|-----------|
        |iotId|Yes|The connection tag that is issued by the server and used to specify a value for username in the MQTT Connect packets.|
        |iotToken|Yes|The token is valid for seven days. It is used to specify a value for password in the MQTT Connect packets.|
        |resources|No|The resource information. The expanded information includes the MQTT server address and CA certificate information.|

    -   Request example using x-www-form-urlencoded:

        ```
        POST /auth/devicename HTTP/1.1
        Host: iot-auth.cn-shanghai.aliyuncs.com
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 123
        productKey=123&sign=123&timestamp=123&version=default&clientId=123&resouces=mqtt&deviceName=test
        sign = hmac_md5(deviceSecret, clientId123deviceNametestproductKey123timestamp123)
        ```

    -   Response example:

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

2.  Connect to MQTT
    1.  Download the [root.crt](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/30539/cn_zh/1495715052139/root.crt) file of IoT Platform. We recommend that you use TLS protocol version 1.2.
    2.  Connect the device to the Alibaba Cloud MQTT server address and authenticate the returned MQTT address and port.
    3.  TLS is used to establish a connection. The client authenticates the server using CA certificates. The server authenticates the client using the "username=iotId, password=iotToken, clientId=custom device identifier \(use either the MAC address or the device serial number to specify clientId\)" in the MQTT connect packets.

        If the iotId or iotToken is invalid, then MQTT Connect fails. The connect ack flag you receive is 3.

        The error code descriptions are as follows:

        -   401: request auth error. This error code is usually returned when the signature is invalid.
        -   460: param error. Parameter error.
        -   500: unknown error. Unknown error.
        -   5001: meta device not found. The specified device does not exist.
        -   6200: auth type mismatch. An unauthorized authentication type error.

If you use an existing demo, set the value of FEATURE\_MQTT\_DIRECT in make.settings to "n." Then call the IOT\_MQTT\_Construct function to reconnect after the authentication.

```
FEATURE_MQTT_DIRECT = n
```

The HTTPS authentication process is documented in the iotx\_guider\_authenticate of \\src\\system\\iotkit-system\\src\\guider.c.

## SDK APIs {#section_gf5_53m_b2b .section}

-   IOT\_MQTT\_Construct: Call IOT\_MQTT\_Construct to establish an MQTT connection to the cloud.

    The mqtt-example program automatically quits every time the program sends a message. The following are methods that you can use to keep the device online:

    -   When the mqtt-example program is executed, use the `./mqtt-example loop` command to make the device always remain online.
    -   Modify the demo code. The demo code of the mqtt-example program contains IOT\_MQTT\_Destroy. The device goes offline when IOT\_MQTT\_Destroy is called. To keep the device online, remove IOT\_MQTT\_Unregister and IOT\_MQTT\_Destroy. Use while to maintain a persistent connection.
    The code is as follows:

    ```
    while(1)
    {
    IOT_MQTT_Yield(pclient, 200); 
    HAL_SleepMs(100);
    }
    ```

    The response parameter declaration is as follows:

    ```
    /**
    * @brief Construct the MQTT client
    * This function initialize the data structures, establish MQTT connection.
    *
    * @param [in] pInitParams: specify the MQTT client parameter.
    *
    * @retval NULL : Construct failed.
    * @retval NOT_NULL : The handle of MQTT client.
    * @see None.
    */
    void *IOT_MQTT_Construct(iotx_mqtt_param_t *pInitParams);
    ```

-   IOT\_MQTT\_Subscribe: Call IOT\_MQTT\_Subscribe to subscribe to a topic in the cloud.

    To ensure that callback topic\_handle\_func can be successfully delivered, keep memory for topic\_filter.

    The code is as follows:

    ```
    /* Subscribe the specific topic */
    rc = IOT_MQTT_Subscribe(pclient, TOPIC_DATA, IOTX_MQTT_QOS1, _demo_message_arrive, NULL);
    if (rc < 0) {
    IOT_MQTT_Destroy(&pclient);
    EXAMPLE_TRACE("IOT_MQTT_Subscribe() failed, rc = %d", rc);
    rc = -1;
    goto do_exit;
    }
    ```

    The response parameter declaration is as follows:

    ```
    /**
    * @brief Subscribe MQTT topic.
    *
    * @param [in] handle: specify the MQTT client.
    * @param [in] topic_filter: specify the topic filter.
    * @param [in] qos: specify the MQTT Requested QoS.
    * @param [in] topic_handle_func: specify the topic handle callback-function.
    * @param [in] pcontext: specify context. When call 'topic_handle_func', it will be passed back.
    *
    * @retval -1 : Subscribe failed.
    * @retval >=0 : Subscribe successful.
    The value is a unique ID of this request.
    The ID will be passed back when callback 'iotx_mqtt_param_t:handle_event'.
    * @see None.
    */
    int IOT_MQTT_Subscribe(void *handle,
    const char *topic_filter,
    iotx_mqtt_qos_t qos,
    iotx_mqtt_event_handle_func_fpt topic_handle_func,
    void *pcontext);
    ```

-   IOT\_MQTT\_Publish: Call IOT\_MQTT\_Publish to publish messages to the cloud.

    The code is as follows:

    ```
    /* Initialize topic information */
    memset(&topic_msg, 0x0, sizeof(iotx_mqtt_topic_info_t));
    strcpy(msg_pub, "message: hello! start!") ;
    topic_msg.qos = IOTX_MQTT_QOS1;
    topic_msg.retain = 0;
    topic_msg.dup = 0;
    topic_msg.payload = (void *)msg_pub;
    topic_msg.payload_len = strlen(msg_pub);
    rc = IOT_MQTT_Publish(pclient, TOPIC_DATA, &topic_msg);
    EXAMPLE_TRACE("rc = IOT_MQTT_Publish() = %d", rc);
    ```

    The response parameter declaration is as follows:

    ```
    /**
    * @brief Publish message to specific topic.
    *
    * @param [in] handle: specify the MQTT client.
    * @param [in] topic_name: specify the topic name.
    * @param [in] topic_msg: specify the topic message.
    *
    * @retval -1 : Publish failed.
    * @retval 0 : Publish successful, where QoS is 0.
    * @retval >0 : Publish successful, where QoS is >= 0.
    The value is a unique ID of this request.
    The ID will be passed back when callback 'iotx_mqtt_param_t:handle_event'.
    * @see None.
    */
    int IOT_MQTT_Publish(void *handle, const char *topic_name, iotx_mqtt_topic_info_pt topic_msg);
    ```

-   IOT\_MQTT\_Unsubscribe: Call IOT\_MQTT\_Unsubscribe to cancel the subscription in the cloud.

    The code is as follows:

    ```
    IOT_MQTT_Unsubscribe(pclient, TOPIC_DATA);
    ```

    The response parameter declaration is as follows:

    ```
    /**
    * @brief Unsubscribe MQTT topic.
    *
    * @param [in] handle: specify the MQTT client.
    * @param [in] topic_filter: specify the topic filter.
    *
    * @retval -1 : Unsubscribe failed.
    * @retval >=0 : Unsubscribe successful.
    The value is a unique ID of this request.
    The ID will be passed back when callback 'iotx_mqtt_param_t:handle_event'.
    * @see None.
    */
    int IOT_MQTT_Unsubscribe(void *handle, const char *topic_filter);
    ```

-   IOT\_MQTT\_Yield: Call IOT\_MQTT\_Yield to receive data.

    Call this operation wherever data needs to be received. Start a separate thread to perform this operation if the system resources are sufficient.

    The code is as follows:

    ```
    /* handle the MQTT packet received from TCP or SSL connection */
    IOT_MQTT_Yield(pclient, 200);
    ```

    The response parameter declaration is as follows:

    ```
    /**
    * @brief Handle MQTT packet from remote server and process timeout request
    * which include the MQTT subscribe, unsubscribe, publish(QOS >= 1), reconnect, etc..
    *
    * @param [in] handle: specify the MQTT client.
    * @param [in] timeout_ms: specify the timeout in millisecond in this loop.
    *
    * @return status.
    * @see None.
    */
    int IOT_MQTT_Yield(void *handle, int timeout_ms);
    ```

-   IOT\_MQTT\_Destroy: Call IOT\_MQTT\_Destroy to terminate an MQTT connection and release the memory.

    The code is as follows:

    ```
    IOT_MQTT_Destroy(&pclient);
    ```

    The response parameter declaration is as follows:

    ```
    /**
    * @brief Deconstruct the MQTT client
    * This function disconnect MQTT connection and release the related resource.
    *
    * @param [in] phandle: pointer of handle, specify the MQTT client.
    *
    * @retval 0 : Deconstruct success.
    * @retval -1 : Deconstruct failed.
    * @see None.
    */
    int IOT_MQTT_Destroy(void **phandle);
    ```

-   IOT\_MQTT\_CheckStateNormal: Call IOT\_MQTT\_CheckStateNormal to view the current connection status.

    You can use this operation to query the status of an MQTT connection. However, this operation cannot detect a device disconnection in real time. This operation may detect a disconnection only when IoT Platform sends data or performs keepalive checks.

    The response parameter declaration is as follows:

    ```
    /**
    * @brief check whether MQTT connection is established or not.
    *
    * @param [in] handle: specify the MQTT client.
    *
    * @retval true : MQTT in normal state.
    * @retval false : MQTT in abnormal state.
    * @see None.
    */
    int IOT_MQTT_CheckStateNormal(void *handle);
    ```


## MQTT keep alive {#section_x45_pjm_b2b .section}

The device sends packets at least once in the time interval specified in keepalive\_interval\_ms. The packets sent include PING requests.

If the server does not receive any packet in the time interval specified in keepalive\_interval\_ms, IoT Platform will disconnect the device and the device needs to reconnect to the platform.

The value of keepalive\_interval\_ms can be configured in IOT\_MQTT\_Construct. IoT Platform uses this value as the heartbeat. The value range of keepalive\_interval\_ms is 60000-300000.

