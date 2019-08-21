# 调用自定义Topic {#concept_z4h_ltl_cfb .concept}

RRPC支持调用自定义Topic与云端通信，且相关Topic中包含了完整的您自定义的Topic。

## 自定义Topic {#section_d2t_2xl_cfb .section}

RRPC调用自定义Topic的格式如下：

-   RRPC请求消息Topic：/ext/rrpc/$\{messageId\}/$\{topic\}
-   RRPC响应消息Topic：/ext/rrpc/$\{messageId\}/$\{topic\}
-   RRPC订阅Topic：/ext/rrpc/+/$\{topic\}

其中$\{messageId\}是云端生成的唯一的RRPC消息ID，$\{topic\}是您的自定义Topic。

## RRPC接入 {#section_g1l_mtl_cfb .section}

1.  云端发送RRPC消息。

    服务端调用云端API RRpc接口向设备发送消息。接口调用方法，请参见[RRpc](../../../../intl.zh-CN/云端开发指南/云端API参考/消息通信/RRpc.md#)。

    以使用Java SDK为例，调用方式：

    ``` {#codeblock_ikf_vvn_531}
    RRpcRequest request = new RRpcRequest();
    request.setProductKey("testProductKey");
    request.setDeviceName("testDeviceName");
    request.setRequestBase64Byte(Base64.getEncoder().encodeToString("hello world"));
    request.setTopic("/testProductKey/testDeviceName/user/get");//如果是自定义Topic调用方式，在这里传递自定义Topic
    request.setTimeout(3000);
    RRpcResponse response = client.getAcsResponse(request);
    ```

    使用自定义Topic格式时，您需要确保您的云端Java SDK（aliyun-java-sdk-iot）版本为6.0.0及以上版本。

    ``` {#codeblock_qpm_asm_vd8}
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-iot</artifactId>
        <version>6.0.0</version>
    </dependency>
    ```

2.  设备端接入。

    从云端下发自定义格式Topic的RRPC调用命令到设备端时，设备端必须在进行MQTT CONNECT协议设置时，在clientId中增加`ext=1`参数。设备端通过MQTT协议接入物联网平台操作指导，请参见[MQTT-TCP连接通信](intl.zh-CN/设备端开发指南/使用开放协议自主接入/MQTT-TCP连接通信.md#)。

    例如，原来传递的clientId为：

    ``` {#codeblock_ywl_wtq_at6}
    mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
    ```

    则添加`ext=1`参数后，传递的clientId为：

    ``` {#codeblock_52y_eer_5lb}
    mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232,ext=1|"
    ```

    **说明：** 云端和设备端之间使用自定义Topic进行RRPC通信的条件：

    -   云端传递的Topic字段不为空。
    -   设备端在建立连接（connect）时传递了`ext=1`参数。
3.  设备端返回RRPC响应的Topic。

    RRPC请求Topic和响应Topic格式一样，直接将请求Topic作为响应Topic即可。

    **说明：** 目前，仅支持设备端返回QoS=0的RRPC响应消息。


