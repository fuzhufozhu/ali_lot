# Connect Android Things to Alibaba Cloud IoT Platform {#concept_ntf_mgq_p2b .concept}

This article uses an indoor air test project as an example to explain how to connect Google Android Things to Alibaba Cloud IoT Platform.

## Hardware {#section_udw_ctq_p2b .section}

-   Hardware list for the project

    The following table lists the hardware required by the indoor air test project.

    |Hardware|Picture|Remarks|
    |--------|-------|-------|
    | NXP Pico i.MX7D

 Development board

 | ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16822/15338290417864_en-US.png)

 |Android Things 1.0**Note:** You can also use Raspberry Pi instead.

|
    | DHT12

 Temperature and humidity sensor

 | ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16822/15338290417865_en-US.png)

 |Supports I2C data communication method.|
    | ZE08-CH2O

 Formaldehyde detection sensor

 | ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16822/15338290417866_en-US.png)

 |Supports UART data communication method.|

-   Layout diagram of the NXP i.MX7D development board pins

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16822/15338290417888_en-US.png)

    For more information about NXP Pico i.MX7D, see [https://developer.android.com/things/hardware/imx7d-pico-io](https://developer.android.com/things/hardware/imx7d-pico-io).

-   Diagram of the hardware connection

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16822/15338290417908_en-US.png)

    -   Connect the SCL \(clock line\) and SDA \(data line\) pins of the temperature and humidity sensor \(DHT12\) with the I2C SCL and SDA pins of the development board.
    -   Connect the TXD \(transmit data\) pin of the formaldehyde detection sensor \(ZE08-CH2O\) with the RXD \(receive data\) pin of the development board, and connect the RXD pin of ZE08-CH2O with the TXD pin of the development board.

## Create a product and device in the Alibaba Cloud IoT Platform console {#section_pqg_qrk_q2b .section}

Prerequisite: You have activated [IoT Platform](https://www.aliyun.com/product/iot) service.

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com).
2.  Create a product in IoT Platform Pro.

    On the Products page, click **Create Product**. Select **Pro Edition** as the version when you are creating the product. For more information, see [Create a product](../../../../intl.en-US/User Guide/Create products and devices/IoT Platform Pro/Create a product.md#).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16822/15338290417969_en-US.png)

3.  Define features for the newly created product.

    On the Product Details page, click **Define Feature** \> **Add**, and then define properties for the product. For more information, see [Define Thing Specification Language models](../../../../intl.en-US/User Guide/Create products and devices/IoT Platform Pro/Define Thing Specification Language models.md#).

    |Property name|Identifier|Data type|Value range|Description |
    |:------------|:---------|:--------|:----------|:-----------|
    |Temperature|temperature|float|-50~100|Detected by DHT12.|
    |Humidity|humidity|float|0~100|Detected by DHT12.|
    |Formaldehyde concentration|ch2o|double|0~3|Detected by ZE08.|

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16822/15338290417991_en-US.png)

4.  Create a device

    On the Devices page, select the name of the newly created product, click **Add Device**, and then create a device. For more information, see [Create a device](../../../../intl.en-US/User Guide/Create products and devices/IoT Platform Pro/Create a device.md#).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16822/15338290417994_en-US.png)


## Develop the Android Things client {#section_cd4_d2l_q2b .section}

1.  Use Android Studio to create an Android Things project, and add the permission for Internet.

    ```
    <uses-permission android:name="android.permission.INTERNET" />
    ```

2.  Add eclipse.paho.mqtt to Gradle file.

    ```
    implementation 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'
    ```

3.  Set that data of DHT12 is read through I2C.

    ```
    private void readDataFromI2C() {
    
            try {
    
                byte[] data = new byte[5];
                i2cDevice.readRegBuffer(0x00, data, data.length);
    
                // check data
                if ((data[0] + data[1] + data[2] + data[3]) % 256 ! = data[4]) {
                    humidity = temperature = 0;
                    return;
                }
                // humidity data
                humidity = Double.valueOf(String.valueOf(data[0]) + "." + String.valueOf(data[1]));
                Log.d(TAG, "humidity: " + humidity);
                // temperature data
                if (data[3] < 128) {
                    temperature = Double.valueOf(String.valueOf(data[2]) + "." + String.valueOf(data[3]));
                } else {
                    temperature = Double.valueOf("-" + String.valueOf(data[2]) + "." + String.valueOf(data[3] - 128));
                }
    
                Log.d(TAG, "temperature: " + temperature);
    
            } catch (IOException e) {
                Log.e(TAG, "readDataFromI2C error " + e.getMessage(), e);
            }
    
        }
    ```

4.  Set that data of Ze08-CH2O is read through UART.

    ```
    try {
                    // data buffer
                    byte[] buffer = new byte[9];
    
                    while (uartDevice.read(buffer, buffer.length) > 0) {
    
                        if (checkSum(buffer)) {
                            ppbCh2o = buffer[4] * 256 + buffer[5];
                            ch2o = ppbCh2o / 66.64 * 0.08;
                        } else {
                            ch2o = ppbCh2o = 0;
                        }
                        Log.d(TAG, "ch2o: " + ch2o);
                    }
    
                } catch (IOException e) {
                    Log.e(TAG, "Ze08CH2O read data error " + e.getMessage(), e);
                }
    ```

5.  Connect Alibaba Cloud IoT Platform and the client, and report data.

    ```
    /*
    Payload format
    {
      "id": 123243,
      "params": {
        "temperature": 25.6,
        "humidity": 60.3,
        "ch2o": 0.048
      },
      "method": "thing.event.property.post"
    }
    */
    MqttMessage message = new MqttMessage(payload.getBytes("utf-8"));
    message.setQos(1);
    
    String pubTopYourPc = "/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post";
    
    mqttClient.publish(pubTopic, message);
    ```


## View real-time data of the device {#section_n34_g5l_q2b .section}

After the device is enabled, you can view the real-time data of the device from the Status column on the Device Details page in IoT Platform console.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16822/15338290418025_en-US.png)

