# Connect to IoT Platform using MQTT.fx {#concept_d3l_fw3_p2b .concept}

This article uses MQTT.fx as an example to introduce the method for using a third-party MQTT client to connect to IoT Platform. MQTT.fx is a MQTT client that is written in Java language and based on Eclipse Paho. It supports subscribing to messages and publishing messages through topics.

## Prerequisites {#section_dhm_qz3_p2b .section}

You have created products and devices in the [IoT Platform console](https://iot.console.aliyun.com), and have got the ProductKey, DeviceName, and DeviceSecrect of the devices. When you set the connection parameters for MQTT.fx, you will use the values of the ProductKey, DeviceName, and DeviceSecrect. For more information about creating products and devices, see [Create a product](../../../../intl.en-US/User Guide/Create products and devices/IoT Platform Basic/Create a product.md#) and [Create a device](../../../../intl.en-US/User Guide/Create products and devices/IoT Platform Basic/Create a device.md#) if you are using IoT Platform Basic, and see [Create a product](../../../../intl.en-US/User Guide/Create products and devices/IoT Platform Pro/Create a product.md#) and [Create a device](../../../../intl.en-US/User Guide/Create products and devices/IoT Platform Pro/Create a device.md#)if you are using IoT Platform Pro.

## Procedure {#section_whq_1bj_p2b .section}

1.  Download and install the MQTT.fx software.

    Download the MQTT.fx software for Windows from: [http://mqtt-fx.software.informer.com/download/](http://mqtt-fx.software.informer.com/download/).

    Download the MQTT.fx software for Mac from: [http://macdownload.informer.com/mqtt-fx/](http://macdownload.informer.com/mqtt-fx/).

2.  Open MQTT.fx, and click the settings icon.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285987694_en-US.png)

3.  Set the connection parameters.

    Currently, two types of connection modes are supported: TCP and TLS.

    -   Set parameters for TCP connection.
        1.  Enter basic information.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285997698_en-US.png)

            -   **Profile Name**: Enter a custom profile name.
            -   **Profile Type**: Select **MQTT Broker**.
            -   **Broker Address**: Enter the connection domain in the format $\{YourProductKey\}.iot-as-mqtt.$\{region\}.aliyuncs.com. In this format, $\{YourProductKey\} and $\{region\} are two variables, whose respective values are your product key and the region ID of your IoT Platform service region. For information about region IDs, see [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm). Example: alPUPCoxxxx.iot-as-mqtt.cn-shanghai.aliyuncs.com.
            -   **Broker Port**: Set the port to 1883.
            -   **Client ID**: Enter the content in the format $\{clientId\}|securemode=3,signmethod=hmacsha1|. $\{clientId\} is your client ID. You can enter any value, and the maximum length is 64 characters. We recommend that you use the MAC address or SN code of your device as the client ID. signmethod is the signature method you want to use. IoT Platform supports hmacmd5 and hmacsha1. Example: 12345|securemode=3,signmethod=hmacsha1|.

                **Note:** Do not click **Generate** after you enter the Client ID information.

            -   You can keep the default parameters for General, or set the values according to your needs.
        2.  Click **User Credentials**, and enter your **User Name** and **Password**.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285997699_en-US.png)

            -   **User Name**: It must be the device name directly followed by the character \(&\) and the product key. Format: $\{YourDeviceName\}&$\{YourPrductKey\}. The values of $\{YourDeviceName\} and $\{YourPrductKey\} respectively are the device name and product key. For example, the user name could be device&fOAt5H5TOWF.
            -   **Password**: You must enter a encrypted value.

                Encryption method:

                Sort and join the parameters \(clientId, deviceName, productKey, and timestamp in a lexicographical order. \(If you have not set a timestamp, do not include timestamp in the string.\) Then, use the deviceSecret of the device as the secret key to encrypt the resulting string by hmacsha1 or hmacmd5.

                Joint string example: `clientId12345deviceNamedeviceproductKeyfOAt5H5TOWF`.

                You can then enter the encrypted result as the password.

                For more information about encryption, see [Direct connection to the MQTT client domain](../../../../intl.en-US/User Guide/Develop devices/Protocols for connecting devices/Establish MQTT over TCP connections.md#section_ivs_b3m_b2b).

                You also can use a password generation tool to generate a password, or to verify the password you encrypted. Click [here](https://files.alicdn.com/tpsservice/471c155376d6a88a29c9ad66784e94f0.zip) to download the tool package.

        3.  Click **OK** after you have entered the required information.
    -   Set parameters for TLS connection

        The method for setting parameters for TLS connection is almost the same as that for TCP, with the following two differences:

        -   In **Client ID**, the securemode of TLS is different from that of TCP. For TLS connection, it must be `securemode=2`. Example: 12345|securemode=2,signmethod=hmacsha1|.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285997732_en-US.png)

            The formats of the **User Name** and **Password** of User Credentials are the same as those of TCP.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285997733_en-US.png)

        -   For TLS connection, you must set SSL/TLS. Check **Enable SSL/TLS** and select **TLSv1** as the protocol.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285997734_en-US.png)

4.  Click **Connect** to connect MQTT.fx and IoT Platform.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285997735_en-US.png)


## Message communication test {#section_rmt_vgk_p2b .section}

Test if MQTT.fx and IoT Platform are successfully connected.

1.  In MQTT.fx, click **Subscribe**.
2.  Enter a topic of the device, and click **Subscribe**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285997736_en-US.png)

    If the topic is successfully subscribed to, it is displayed in the list.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285997737_en-US.png)

3.  Go to the [IoT Platform console](https://iot.console.aliyun.com). On the **Topic List** page of the Device Details, click the **Publish** button of the topic you subscribed to.
4.  Enter message content, and click **OK**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338285997738_en-US.png)

5.  Go back to MQTT.fx to check if the message is received.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338286007739_en-US.png)


## View logs {#section_lnm_nkk_p2b .section}

In MQTT.fx, click **Log** to view the operation logs and error logs.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16788/15338286007740_en-US.png)

