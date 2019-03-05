# Establish MQTT over WebSocket connections {#task_rql_zkw_vdb .task}

IoT Platform supports MQTT over WebSocket. You can first use the WebSocket protocol to establish a connection, and then use the MQTT protocol to communicate on the WebSocket channel.

Using WebSocket has the following advantages:

-   Allows browser-based applications to establish persistent connections to the server.
-   Uses port 433, which allows messages to pass through most firewalls.

1.   Certificate preparation 

    The WebSocket protocol includes WebSocket and WebSocket Secure. Websocket and WebSocket Secure are used for unencrypted and encrypted connections, respectively. Transport Layser Security \(TLS\) is used in WebSocket Secure connections. Like a TLS connection, a WebSocket Secure connection requires a [root certificate](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt?spm=5176.doc30539.2.4.aalCo6&file=root.crt).

2.   Client selection 

    IoT Platform provides [Java MQTT SDK](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iotx-sdk-java/iotx-sdk-mqtt-java-20170526.zip?spm=5176.doc42648.2.18.7iyFfe&file=iotx-sdk-mqtt-java-20170526.zip). You can directly use this client SDK by replacing the connect URL with a URL that is used by WebSocket. For clients that use other language versions or connections without using the official SDK, see [Open-source MQTT clients](https://github.com/mqtt/mqtt.github.io/wiki/libraries?spm=5176.doc30539.2.5.aalCo6). Make sure that the client supports WebSocket.

3.   Connections 

    An MQTT over WebSocket connection has a different protocol and port number in the connect URL from an MQTT over TCP connection. MQTT over WebSocket connections have the same parameters as MQTT over TCP connections. The securemode parameter is set to 2 and 3 for WebSocket Secure connections and WebSocket connections, respectively.

    -   Connection domain for Shanghai region: $\{YourProductKey\}.iot-as-mqtt.cn-shanghai.aliyuncs.com:443

        Replace $\{YourProductKey\} with your ProductKey.

    -   Variable header: Keep Alive

        The Keep Alive parameter must be included in the CONNECT packet. The allowed range of Keep Alive value is 30-1200 seconds. If the value of Keep Alive is not in this range, IoT Platform will reject the connection. We recommend that you set a value larger than 300 seconds. If the Internet connection is not stable, set a larger value.

        In a keep alive interval, the device must send at least one packet, including ping requests.

        If IoT Platform does not receive any packets in a keep alive interval, the device is disconnected from IoT Platform and needs to reconnect to the server.

    -   An MQTT Connect packet contains the following parameters:

        ```
        
        mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
        mqttUsername: deviceName+"&"+productKey
        mqttPassword:  sign_hmac(deviceSecret,content). Sort and concatenate the input parameters in alphabetical order and then encrypt the parameters using the specified sign method.
        The value of content is a string that is built by sorting and concatenating the parameters sent to the server (productKey, deviceName, timestamp, and clientId).
        ```

        Where,

        -   clientId: Specifies the client ID up to 64 characters. We recommend that you use the MAC address or SN code.
        -   timestamp: \(Optional\) Specifies the current time in milliseconds.
        -   mqttClientId: Parameters within `||` are extended parameters.
        -   signmethod: Specifies a signature algorithm.  
        -   securemode: Specifies the secure mode. Values include 2 \(WebSocket Secure\) and 3 \(WebSocket\).
    The following are examples of MQTT Connect packets. Suppose the parameter values are:

    ```
    clientId=12345, deviceName=device, productKey=pk, timestamp=789, signmethod=hmacsha1, deviceSecret=secret
    ```

    -   For a WebSocket connection:
        -   Connection domain

            ```
            ws://pk.iot-as-mqtt.cn-shanghai.aliyuncs.com:443
            ```

        -   Connection parameters

            ```
            
            mqttclientId=12345|securemode=3,signmethod=hmacsha1,timestamp=789|
            mqttUsername=device&pk
            mqttPasswrod=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString(); 
            ```

    -   For a WebSocket Secure connection:
        -   Connection domain

            ```
            wss://pk.iot-as-mqtt.cn-shanghai.aliyuncs.com:443
            ```

        -   Connection parameters

            ```
            
            mqttclientId=12345|securemode=2,signmethod=hmacsha1,timestamp=789|
            mqttUsername=device&pk
            mqttPasswrod=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString();
            ```


