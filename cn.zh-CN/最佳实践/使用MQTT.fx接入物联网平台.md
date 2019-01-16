# 使用MQTT.fx接入物联网平台 {#concept_d3l_fw3_p2b .concept}

本文档以MQTT.fx为例，介绍使用第三方软件以MQTT协议接入物联网平台。MQTT.fx是一款基于Eclipse Paho，使用Java语言编写的MQTT客户端工具。支持通过Topic订阅和发布消息。

## 前提条件 {#section_dhm_qz3_p2b .section}

您已在[物联网平台控制台](https://iot.console.aliyun.com)创建了产品和设备，并获取了设备三元组信息：ProductKey、DeviceName和DeviceSerect。在MQTT.fx中设置连接参数时，将需要使用设备的三元组信息。创建产品和设备时，如需帮助请参见[创建产品\(基础版\)](../../../../../intl.zh-CN/用户指南/产品与设备/创建产品(基础版).md#)、[创建产品\(高级版\)](../../../../../intl.zh-CN/用户指南/产品与设备/创建产品(高级版).md#)、[单个创建设备](../../../../../intl.zh-CN/用户指南/产品与设备/创建设备/单个创建设备.md#)和[批量创建设备](../../../../../intl.zh-CN/用户指南/产品与设备/创建设备/批量创建设备.md#)。

## MQTT.fx接入物联网平台 {#section_whq_1bj_p2b .section}

1.  下载并安装MQTT.fx软件。

    Windows系统： [http://mqtt-fx.software.informer.com/download/](http://mqtt-fx.software.informer.com/download/)

    Mac系统： [http://macdownload.informer.com/mqtt-fx/](http://macdownload.informer.com/mqtt-fx/)

2.  打开MQTT.fx软件，单击设置图标。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034777694_zh-CN.png)

3.  在参数设置页面，设置连接参数。

    目前支持两种连接模式：TCP直连和TLS直连。两种模式设置仅Client ID与SSL/TLS信息设置不同。

    具体设置步骤如下：

    1.  设置基本信息。设置参数说明，请参见下表。

        General栏目下的设置项可保持系统默认，也可根据您的具体需求设置。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034787698_zh-CN.png)

        |参数|说明|
        |:-|:-|
        |Profile Name|输入您的自定义名称。|
        |Profile Type|选择为 **MQTT Broker**。|
        |Broker Address|连接域名。格式：$\{YourProductKey\}.iot-as-mqtt.$\{region\}.aliyuncs.com。其中，$\{region\}需替换为您物联网平台服务所在地域的代码。地域代码，请参见[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)。如：alPUPCoxxxx.iot-as-mqtt.cn-shanghai.aliyuncs.com。|
        |Broker Port|设置为1883。|
        |Client ID|填写mqttClientId，用于MQTT的底层协议报文。格式固定，为：$\{clientId\}|securemode=3,signmethod=hmacsha1|。完整示例如：`12345|securemode=3,signmethod=hmacsha1|`。其中，        -   $\{clientId\}为设备的ID信息，可取任意值，长度在64字符以内。建议使用设备的MAC地址或SN码。
        -   securemode为安全模式，TCP直连模式设置为securemode=3，TLS直连为securemode=2。
        -   signmethod为算法类型，支持hmacmd5和hmacsha1。
**说明：** 输入Client ID信息后，请勿单击**Generate**。

|

    2.  单击**User Credentials**，设置 **User Name** 和 **Password**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034787699_zh-CN.png)

        |参数|说明|
        |:-|:-|
        |User Name|由设备名DeviceName、符号（&）和产品ProductKey组成。格式为：$\{YourDeviceName\}&$\{YourPrductKey\}。完整示例如：device&fOAt5H5TOWF。|
        |Password|密码由参数值拼接加密而成。您可以下载并使用[Password生成小工具](https://files.alicdn.com/tpsservice/88413c66e471bec826257781969d1bc7.zip)自动生成Password，也可以手动生成Password。        -   使用Password生成小工具中参数说明：
            -   productKey：设备所属产品Key。可在控制台设备详情页查看。
            -   deviceName：设备名称。可在控制台设备详情页查看。
            -   deviceSecret：设备密钥。可在控制台设备详情页查看。
            -   timestamp：（可选）时间戳。
            -   clientId：设备的ID信息，与**Client ID**中$\{clientId\}一致。
            -   method：选择签名算法类型，与**Client ID**中signmethod确定的加密方法一致。
        -   手动生成方法如下：
            1.  拼接参数。

提交给服务器的clientId、deviceName、productKey和timestamp（timestamp为非必选参数）参数及参数值依次拼接。本例中拼接结果为：`clientId12345deviceNamedeviceproductKeyfOAt5H5TOWF`

            2.  加密。

通过**Client ID**中确定的加密方法，使用设备deviceSecret，将拼接结果加密。

假设设备的deviceSecret值为abc123，加密计算格式为`hmacsha1(abc123,clientId12345deviceNamedeviceproductKeyfOAt5H5TOWF)`

|

    3.  在TLS直连模式下，需设置SSL/TLS信息。TCP直连无需设置。

        勾选 **Enable SSL/TLS**对应的复选框，并选择 Protocol 为 **TLSv1**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034787734_zh-CN.png)

    4.  填写完成后，单击**OK**。
4.  设置完成后，单击 **Connect**进行连接。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034787735_zh-CN.png)


## 下行通信测试 {#section_rmt_vgk_p2b .section}

从物联网平台发送消息，在MQTT.fx上接收消息，测试MQTT.fx与物联网平台连接是否成功 。

1.  在MQTT.fx上，单击**Subscribe**。
2.  输入一个设备下的Topic，然后单击**Subscribe**，订阅这个Topic。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034787736_zh-CN.png)

    订阅成功后，该Topic将显示在列表中。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034787737_zh-CN.png)

3.  在[物联网平台控制台](https://iot.console.aliyun.com)中，该设备的设备详情页，**Topic列表**下，单击已订阅的Topic对应的**发布消息**操作按钮。
4.  输入消息内容，单击**确认**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034787738_zh-CN.png)

5.  回到MQTT.fx上，查看是否接收到消息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034787739_zh-CN.png)


## 上行通信测试 {#section_ysm_bxt_lgb .section}

在MQTT.fx上发送消息，通过查看设备日志，测试MQTT.fx与物联网平台连接是否成功 。

1.  在MQTT.fx上，单击**Publish**。
2.  输入一个设备的Topic，然后单击**Publish**，向这个Topic推送QoS 1的消息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/154760347837281_zh-CN.png)

3.  在[物联网平台控制台](https://iot.console.aliyun.com)中，该设备的**设备详情** \> **日志服务** \> **上行消息分析**栏下，查看上行消息。

    您还可以复制MessageID，在**消息内容查询**中，选择**原始数据**查看具体消息内容。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/154760347837288_zh-CN.png)


## 查看日志 {#section_lnm_nkk_p2b .section}

在MQTT.fx上，单击**Log**查看操作日志和错误提示日志。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15476034787740_zh-CN.png)

