# MQTT-WebSocket连接通信 {#task_rql_zkw_vdb .task}

物联网平台支持基于WebSocket的MQTT协议。您可以首先使用WebSocket建立连接，然后在WebSocket通道上，使用MQTT协议进行通信，即MQTT over WebSocket。

使用WebSocket方式主要有以下优势：

-   使基于浏览器的应用程序可以像普通设备一样，具备与服务端建立MQTT长连接的能力。
-   WebSocket方式使用443端口，消息可以顺利穿过大多数防火墙。

1.  证书准备。 

    WebSocket可以使用ws和wss两种方式，ws就是普通的WebSocket连接，wss就是增加了TLS加密。如果使用wss方式进行安全连接，需要使用和TLS直连一样的[根证书](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt?spm=5176.doc30539.2.4.aalCo6&file=root.crt)。

2.  客户端选择。 

    直接使用[官方客户端](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iotx-sdk-java/iotx-sdk-mqtt-java-20170526.zip?spm=5176.doc42648.2.18.7iyFfe&file=iotx-sdk-mqtt-java-20170526.zip)，只需要替换连接URL即可。其他语言版本客户端或者是自主接入，请参考[开源MQTT客户端](https://github.com/mqtt/mqtt.github.io/wiki/libraries?spm=5176.doc30539.2.5.aalCo6)参考，使用前请阅读相关客户端的说明，是否支持WebSocket方式。

3.  连接说明。 

    使用WebSocket方式进行连接，区别主要在MQTT连接URL的协议和端口号，MQTT连接参数和TCP直接连接方式完全相同，其中要注意securemode参数，使用wss方式连接时securemode=2，使用ws方式连接时securemode=3。

    -   连接域名，华东2节点：$\{YourProductKey\}.iot-as-mqtt.cn-shanghai.aliyuncs.com:443

        $\{YourProductKey\}请替换为您的产品key。

    -   可变报头（variable header）：Keep Alive

        Connect指令中需包含Keep Alive（保活时间）。保活心跳时间取值范围为30至1200秒。如果心跳时间不在此区间内，物联网平台会拒绝连接。建议取值300秒以上。如果网络不稳定，将心跳时间设置高一些。

        设备端在保活时间间隔内，至少需要发送一次报文，包括PING请求。

        如果物联网平台在保活时间内无法收到任何报文，物联网平台会断开连接，设备端需要进行重连。

    -   MQTT的Connect报文参数如下：

        ``` {#codeblock_ikw_2j6_gk3}
        
        mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
        mqttUsername: deviceName+"&"+productKey
        mqttPassword: sign_hmac(deviceSecret,content)sign签名需要把以下参数按字典序排序后，再根据signmethod加签。
        content=提交给服务器的参数（productKey,deviceName,timestamp,clientId）, 按照字母顺序排序, 然后将参数值依次拼接
        ```

        其中，

        -   clientId：表示客户端ID，建议mac或sn，64字符内。
        -   timestamp：表示当前时间毫秒值，可选。
        -   mqttClientId：格式中`||`内为扩展参数。
        -   signmethod：表示签名算法类型。
        -   securemode：表示目前安全模式，可选值有2 （wss协议）和3（ws协议）。
    参考示例，如果预置前提如下：

    ``` {#codeblock_4pm_htl_a0q}
    clientId = 12345，deviceName = device， productKey = pk， timestamp = 789，signmethod=hmacsha1，deviceSecret=secret
    ```

    -   使用ws方式
        -   连接域名

            ``` {#codeblock_rqo_buz_8e7}
            ws://pk.iot-as-mqtt.cn-shanghai.aliyuncs.com:443
            ```

        -   连接参数

            ``` {#codeblock_92c_zvq_oyj}
            
            mqttclientId=12345|securemode=3,signmethod=hmacsha1,timestamp=789|
            mqttUsername=device&pk
            mqttPasswrod=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString(); 
            ```

    -   使用wss方式
        -   连接域名

            ``` {#codeblock_lv4_69d_jba}
            wss://pk.iot-as-mqtt.cn-shanghai.aliyuncs.com:443
            ```

        -   连接参数

            ``` {#codeblock_g7o_oyb_82d}
            
            mqttclientId=12345|securemode=2,signmethod=hmacsha1,timestamp=789|
            mqttUsername=device&pk
            mqttPasswrod=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString();
            ```

    建议您使用设备端SDK接入物联网平台。如果您自行开发接入，可参考[MQTT连接签名示例](intl.zh-CN/设备端开发指南/设备多协议连接/MQTT连接签名示例.md#)。


