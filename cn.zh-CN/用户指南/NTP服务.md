# NTP服务 {#concept_s2r_tx3_kgb .concept}

物联网平台提供NTP服务，解决嵌入式设备资源受限，系统不包含NTP服务，端上没有精确时间戳的问题。

## 原理介绍 {#section_jls_xkj_kgb .section}

物联网平台借鉴NTP协议原理，将云端作为NTP服务器。设备端发送一个特定Topic给云端，payload中带上发送时间。云端回复时在payload中加上云端的接收时间和发送时间。设备端收到回复后，再结合自己本地当前时间，得出一共4个时间。一起计算出设备端与云端的时间差，从而得出端上当前的精确时间。

**说明：** 只有设备端与云端成功建立连接之后，才能通过NTP服务进行校准。

举个例子，嵌入式设备上电后没有准确时间，TLS建连过程中证书时间校验失败的问题，无法通过NTP服务解决，因为此时设备与云端尚未成功建立连接。

## 接入流程 {#section_btf_by3_kgb .section}

请求Topic：`/ext/ntp/${YourProductKey}/${YourDeviceName}/request`

响应Topic：`/ext/ntp/${YourProductKey}/${YourDeviceName}/response`

**说明：** ProductKey和DeviceName是设备证书的一部分，可以从控制台获取。

1.  设备端订阅`/ext/ntp/${YourProductKey}/${YourDeviceName}/response`Topic。
2.  设备端向`/ext/ntp/${YourProductKey}/${YourDeviceName}/request`Topic发送一条QoS=0的消息，payload中带上设备当前的时间戳，单位为毫秒。示例如下：

    ``` {#codeblock_mgp_e6y_yjy}
    {
        "deviceSendTime":"100"
    }
    ```

    **说明：** 

    -   时间戳数字的格式，支持Long和String。
    -   NTP服务目前仅支持QoS=0的消息。
3.  设备端收到服务端回复的消息，payload中包含以下信息：

    ``` {#codeblock_d55_eov_bch}
    {
        "deviceSendTime":"100",
        "serverRecvTime":"1010",
        "serverSendTime":"1015",
    }
    ```

4.  设备端计算出当前精确的unix时间。

    设备端收到服务端的时间记为$\{deviceRecvTime\}，则设备上的精确时间为：`(${serverRecvTime} + ${serverSendTime} + ${deviceRecvTime} - ${deviceSendTime}) / 2`


## 使用示例 {#section_iwy_nmj_kgb .section}

例如，设备上时间是100，服务端时间是1000，链路延时是10，服务端从接收到发送经过了5。

|-|设备端时间|服务端时间|
|:-|:----|-----|
|设备发送|100（deviceSendTime）|1000|
|服务端接收|110|1010（serverRecvTime）|
|服务端发送|115|1015（serverSendTime）|
|设备端接收|125（deviceRecvTime）|1025|

则设备端计算出的当前准确时间为（1010 + 1015 + 125 - 100）/ 2 = 1025。

与云端当前时间相同对比下，如果直接采用云端返回的时间戳，只能得到1015，端上的时间会有一个链路延时的误差。

