# 系统Topic {#concept_zfd_btl_cfb .concept}

RRPC支持调用系统Topic与云端通信，且相关Topic包含ProductKey和DeviceName。

## 系统Topic {#section_hln_dxl_cfb .section}

RRPC调用的系统Topic格式如下：

-   RRPC请求消息Topic：/sys/$\{YourProductKey\}/$\{YourDeviceName\}/rrpc/request/$\{messageId\}
-   RRPC响应消息Topic：/sys/$\{YourProductKey\}/$\{YourDeviceName\}/rrpc/response/$\{messageId\}
-   RRPC订阅Topic：/sys/$\{YourProductKey\}/$\{YourDeviceName\}/rrpc/request/+

其中，$\{YourProductKey\}和$\{YourDeviceName\}是您设备的三元组信息，$\{messageId\}是云端生成的唯一的RRPC消息id。

## RRPC接入 {#section_kz1_ftl_cfb .section}

1.  云端接入工作。

    调用RRPC的API，将您的设备接入云端SDK，详细调用方法请见[RRPC](../../../../intl.zh-CN/云端开发指南/云端API参考/消息通信/RRpc.md#)。

    以使用Java SDK为例，调用方式：

    ```
    RRpcRequest request = new RRpcRequest();
    request.setProductKey("testProductKey");
    request.setDeviceName("testDeviceName");
    request.setRequestBase64Byte(Base64.getEncoder().encodeToString("hello world"));
    request.setTimeout(3000);
    RRpcResponse response = client.getAcsResponse(request);
    ```

2.  返回RRPC响应Topic。

    设备端收到RRPC请求Topic之后，需要根据RRPC请求Topic的格式，返回对应的RRPC响应Topic消息。

    从收到的Topic中（/sys/$\{YourProductKey\}/$\{YourDeviceName\}/rrpc/request/$\{messageId\}）提取出messageId，然后拼装出对应的RRPC响应Topic的格式，发送给云端。


