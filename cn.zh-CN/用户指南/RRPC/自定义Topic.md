# 自定义Topic {#concept_z4h_ltl_cfb .concept}

RRPC支持调用自定义Topic与云端通信，且相关Topic中包含了完整的您自定义的Topic。

## 自定义Topic {#section_d2t_2xl_cfb .section}

RRPC调用的自定义Topic格式如下：

-   RRPC请求消息Topic：/ext/rrpc/$\{messageId\}/$\{topic\}
-   RRPC响应消息Topic：/ext/rrpc/$\{messageId\}/$\{topic\}
-   RRPC订阅Topic：/ext/rrpc/+/$\{topic\}

其中$\{messageId\}是云端生成的唯一的RRPC消息id，$\{topic\}是您的自定义Topic。

## RRPC接入 {#section_g1l_mtl_cfb .section}

1.  云端接入。

    调用RRPC的API，将您的设备接入云端SDK，详细调用方法请见[RRPC](../../../../intl.zh-CN/云端开发指南/云端API参考/消息通信/RRpc.md#)。

    以使用Java SDK为例，调用方式：

    ```
    RRpcRequest request = new RRpcRequest();
    request.setProductKey("testProductKey");
    request.setDeviceName("testDeviceName");
    request.setRequestBase64Byte(Base64.getEncoder().encodeToString("hello world"));
    request.setTopic("/testProductKey/testDeviceName/get");//如果是自定义Topic调用方式，在这里传递自定义Topic
    request.setTimeout(3000);
    RRpcResponse response = client.getAcsResponse(request);
    ```

    使用自定义Topic格式时，您需要确保您的Java SDK（aliyun-java-sdk-iot）版本为6.0.0及以上版本。

    ```
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-iot</artifactId>
        <version>6.0.0</version>
    </dependency>
    ```

2.  设备端接入。

    从云端下发自定义格式Topic的RRPC调用命令到设备端时，设备端必须在进行MQTT CONNECT协议设置时，在clientId中增加ext=1参数。详情请参考[MQTT-TCP连接通信](intl.zh-CN/设备端开发指南/设备多协议连接/MQTT-TCP连接通信.md#)。

    例如，原来传递的clientId为：

    ```
    mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
    ```

    则添加ext=1参数后，传递的clientId为：

    ```
    mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232,ext=1|"
    ```

    **说明：** 云端和设备端之间RRPC调用使用自定义topic的条件：

    -   云端传递的Topic字段不为空
    -   设备端在connect时传递了ext=1参数
3.  回复RRPC响应Topic。

    RRPC请求Topic和响应Topic格式是一样的，不需要提取messageId，直接将请求Topic作为响应Topic即可。


