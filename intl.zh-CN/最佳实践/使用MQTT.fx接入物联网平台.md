# 使用MQTT.fx接入物联网平台 {#concept_d3l_fw3_p2b .concept}

本文档以MQTT.fx为例，介绍使用第三方软件以MQTT协议接入物联网平台。MQTT.fx是一款基于Eclipse Paho，使用Java语言编写的MQTT客户端工具。支持通过Topic订阅和发布消息。

## 前提条件 {#section_dhm_qz3_p2b .section}

您已在[物联网平台控制台](https://iot.console.aliyun.com)创建了产品和设备，并获取了设备三元组信息：ProductKey、DeviceName和DeviceSerect。在MQTT.fx中设置链接参数时，将需要使用设备的三元组信息。创建产品和设备时，如需帮助请参见基础版[创建产品](../../../../intl.zh-CN/用户指南/创建产品与设备/基础版/创建产品.md#)和[创建设备](../../../../intl.zh-CN/用户指南/创建产品与设备/基础版/创建设备.md#)，高级版[创建产品](../../../../intl.zh-CN/用户指南/创建产品与设备/高级版/创建产品.md#)和[创建设备](../../../../intl.zh-CN/用户指南/创建产品与设备/高级版/创建设备.md#)。

## MQTT.fx接入物联网平台 {#section_whq_1bj_p2b .section}

1.  下载、安装MQTT.fx软件。

    适用Windows 系统的MQTT.fx软件下载地址： [http://mqtt-fx.software.informer.com/download/](http://mqtt-fx.software.informer.com/download/)

    适用Mac系统的MQTT.fx软件下载地址： [http://macdownload.informer.com/mqtt-fx/](http://macdownload.informer.com/mqtt-fx/)

2.  打开MQTT.fx软件，再单击设置图标。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017694_zh-CN.png)

3.  在参数设置页面，设置连接参数。

    目前支持两种连接模式：TCP直连模式和TLS直连模式。

    -   设置TCP直连模式
        1.  设置基本信息。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017698_zh-CN.png)

            -   **Profile Name**：输入您的自定义名称。
            -   **Profile Type**：选择为 **MQTT Broker**。
            -   **Broker Address**：连接域名。格式：$\{YourProductKey\}.iot-as-mqtt.$\{region\}.aliyuncs.com。其中，$\{YourProductKey\}和$\{region\}是变量，需分别替换为您的产品 ProductKey和您的物联网平台服务地域代码。地域代码，请参见[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)。如：alPUPCoxxxx.iot-as-mqtt.cn-shanghai.aliyuncs.com
            -   **Broker Port**：设置为1883。
            -   **Client ID**：填写的内容需为固定格式，格式如：$\{clientId\}|securemode=3,signmethod=hmacsha1|。其中，$\{clientId\}为客户端ID，可取任意值，建议使用设备的mac地址或sn码，长度在64字符以内；signmethod参数为您使用的算法类型，支持hmacmd5和hmacsha1。填写示例：12345|securemode=3,signmethod=hmacsha1|。

                **说明：** 输入Client ID信息后，请勿单击**Generate**。

            -   General栏目下的设置项可保持系统默认，也可根据您的具体需求设置。
        2.  单击**User Credentials**，设置 **User Name** 和 **Password**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017699_zh-CN.png)

            -   **User Name**：由设备名DeviceName、符号（&）和产品ProductKey组成。格式：$\{YourDeviceName\}&$\{YourPrductKey\}。其中，变量$\{YourDeviceName\}和$\{YourPrductKey\}需替换为您的设备名和产品Key。如：device&fOAt5H5TOWF。
            -   **Password**：签名。需输入加密后的值。

                加密方法：

                将提交给服务器的参数（clientId、deviceName、productKey和timestamp）和对应的参数值进行拼接（timestamp若未设置，则不包括），然后将拼接结果和设备的deviceSecret，通过hmacsha1或hmacmd5进行加密。

                本示例中拼接结果：`clientId12345deviceNamedeviceproductKeyfOAt5H5TOWF`

                加密后得到的结果作为Password。

                如需更多帮助，请参见[MQTT客户端域名直连](../../../../intl.zh-CN/用户指南/设备开发指南/设备多协议连接/MQTT-TCP连接通信.md#section_ivs_b3m_b2b)中签名部分。

        3.  填写完成后，单击**OK**。
    -   设置TLS直连模式

        TLS模式的设置与TCP模式的设置方法大致相似，但有以下不同点：

        -   **Client ID**设置中，安全模式securemode与TCP直连的安全模式不同。TLS 模式`securemode=2`。如：12345|securemode=2,signmethod=hmacsha1|。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017732_zh-CN.png)

            User Credentials栏目下的 **User Name** 和 **Password**填写方法与TCP模式的相同。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017733_zh-CN.png)

        -   TLS模式，需设置SSL/TLS信息：勾选 **Enable SSL/TLS**对应的复选框，并选择 Protocol 为 **TLSv1**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017734_zh-CN.png)

4.  设置完成后，单击 **Connect**进行连接。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017735_zh-CN.png)


## 通信测试 {#section_rmt_vgk_p2b .section}

测试MQTT.fx与物联网平台连接是否成功 。

1.  在MQTT.fx上，单击**Subscribe**。
2.  输入一个设备下的Topic，然后单击**Subscribe**，订阅这个Topic。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017736_zh-CN.png)

    订阅成功后，该Topic将显示在列表中。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017737_zh-CN.png)

3.  在[物联网平台控制台](https://iot.console.aliyun.com)中，该设备的设备详情页，**Topic列表**下，单击已订阅的Topic对应的**发布消息**操作按钮。
4.  输入消息内容，单击**确认**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017738_zh-CN.png)

5.  回到MQTT.fx上，查看是否接收到消息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017739_zh-CN.png)


## 查看日志 {#section_lnm_nkk_p2b .section}

在MQTT.fx上，单击**Log**查看操作日志和错误提示日志。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15330225017740_zh-CN.png)

