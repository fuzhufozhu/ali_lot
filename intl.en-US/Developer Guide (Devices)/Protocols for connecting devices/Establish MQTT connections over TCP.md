# Establish MQTT connections over TCP {#concept_mhv_ghm_b2b .concept}

This topic describes how to establish MQTT connections over TCP by using two methods: direct connection and connection after HTTPS verification.

**Note:** When you configure MQTT CONNECT packets:

-   Do not use the same device certificate \(ProductKey, DeviceName, and DeviceSecret\) for multiple physical devices for connection authentication. This is because when a new device initiates authentication to IoT Platform, a device that is already connected to IoT Platform using the same device certificate will be brought offline. Later, the device which was brought offline will try to connect again, causing the newly connected device to be brought offline instead.
-   In MQTT connection mode, open-source SDKs automatically reconnect to IoT Platform after they are brought offline. You can check the actions of devices by viewing the device logs.

## Direct MQTT client connection {#section_ivs_b3m_b2b .section}

1.  We recommend that you use the TLS protocol for encryption, because it provides better security. Click [here](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt) to download the TLS root certificate.
2.  Connect devices to the server using the MQTT client. For connection methods, see[Open-source MQTT client references](https://github.com/mqtt/mqtt.github.io/wiki/libraries). For more information about the MQTT protocol, see [http://mqtt.org](http://mqtt.org/).

    **Note:** Alibaba Cloud does not provide technical support for third-party code.

3.  Establish an MQTT connection.

    |Connection domain name|`${YourProductKey}.iot-as-mqtt. ${YourRegionId}.aliyuncs.com:1883`Replace $\{YourProductKey\} with your ProductKey.

Replace $\{YourRegionId\} with the region ID of your device. For information about regions and zones, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

|
    |Variable header: Keep Alive|The Keep Alive parameter must be included in the CONNECT packet. The allowed range of Keep Alive value is 30-1200 seconds. If the value of Keep Alive is not in this range, IoT Platform will reject the connection. We recommend that you set a value larger than 300 seconds. If the Internet connection is not stable, set a larger value.|
    |Parameters in an MQTT CONNECT packet|     ```
mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
mqttUsername: deviceName+"&"+productKey
mqttPassword: sign_hmac(deviceSecret,content)
    ```

 mqttPassword: Sort the parameters to be submitted to the server alphabetically and then encrypt the parameters based on the specified sign method.

 The content value is a string that is built by sorting and concatenating the ProductKey, DeviceName, timestamp \(optional\) and clientId in alphabetical order, without any delimiters.

    -   clientId: The client ID is a device identifier. We recommend that you use the MAC address or the serial number of the device as the client ID. The length of the client ID must be within 64 characters.
    -   timestamp: The 13-digit timestamp of the current time. This parameter is optional.
    -   mqttClientId: Extended parameters are placed between vertical bars \(`|`\).
    -   signmethod: The signature algorithm. Valid values: hmacmd5, hmacsha1, and hmacsha256. Default value: hmacmd5.
    -   securemode: The current security mode. Value options: 2 \(TLS connection\) and 3 \(TCP connection\).
 Example:

 Suppose that `clientId=12345, deviceName=device, productKey=pk, timestamp=789, signmethod=hmacsha1, deviceSecret=secret`. The MQTT CONNECT packet sent over TCP is as follows:

     ```
mqttclientId=12345|securemode=3,signmethod=hmacsha1,timestamp=789|
mqttUsername=device&pk
mqttPassword=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString(); //The toHexString() function converts a binary string to a hexadecimal string. The string is case-insensitive.
    ```

 The encrypted password is as follows:

     ```
FAFD82A3D602B37FB0FA8B7892F24A477F851A14
    ```

 |


## MQTT client connection after HTTPS verification {#section_brp_33m_b2b .section}

1.  Authenticate the device.

    Use HTTPS for device authentication. The authentication URL is `https://iot-auth. ${YourRegionId}.aliyuncs.com/auth/devicename`. Replace $\{YourRegionId\} with the region ID of your device. For more information about regions, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

    -   Request parameters

        |Parameter|Required|Description|
        |---------|--------|-----------|
        |productKey|Yes|The unique identifier of the product. You can view it in the IoT Platform console.|
        |deviceName|Yes|The device name. You can view it in the IoT Platform console.|
        |sign|Yes|The signature. The format is hmacmd5\(deviceSecret, content\). The content value is a string that is built by sorting and concatenating of all the parameters \(except version, sign, resources, and signmethod\) that need to be submitted to the server in alphabetical order.|
        |signmethod|No|The signature algorithm. Valid values: hmacmd5, hmacsha1, and hmacsha256. Default value: hmacmd5.|
        |clientId|Yes|The client ID. The length must be within 64 characters.|
        |timestamp|No|Timestamp. Timestamp verification is not required.|
        |resources|No|The resource that you want to obtain, such as MQTT. Use commas \(,\) to separate multiple resource names.|

    -   Response parameters

        |Parameter|Required|Description|
        |---------|--------|-----------|
        |iotId|Yes|The connection tag that is issued by the server. It is used to specify a value for the user name for the MQTT CONNECT packet.|
        |iotToken|Yes|The token is valid for seven days. It is used as the password for the MQTT CONNECT packet.|
        |resources|No|The resource information. The extended information includes the MQTT server address and CA certificate information.|

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

2.  Establish an MQTT connection.
    1.  Download the [root.crt](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/30539/cn_zh/1495715052139/root.crt) file of IoT Platform. We recommend that you use TLS 1.2.
    2.  Connect the device client to the Alibaba Cloud MQTT server using the returned MQTT host address and port of device authentication.
    3.  Establish a connection over TLS. The device client authenticates the IoT Platform server by CA certificates. The IoT Platform server authenticates the device client by the information in the MQTT CONNECT packet. In the packet, username=iotId, password=iotToken, clientId=custom device identifier \(we recommend that you use the MAC address or the device serial number as the device identifier\).

        If the iotId or iotToken is invalid, then the MQTT connection fails. The connect acknowledgment \(ACK\) flag you receive is 3.

        The error codes are described as follows:

        -   401: request auth error. This error code is returned when the signature is mismatched.
        -   460: param error. Parameter error.
        -   500: unknown error. Unknown error.
        -   5001: meta device not found. The specified device does not exist.
        -   6200: auth type mismatch. The authentication type is invalid.

## MQTT Keep Alive {#section_x45_pjm_b2b .section}

In a keep alive interval, the device must send at least one packet, including ping requests.

If IoT Platform does not receive any packets in a keep alive interval, the device is disconnected from IoT Platform and needs to reconnect to the server.

The keep alive time must be in a range of 30 to 1200 seconds. We recommend that you set a value larger than 300 seconds.

