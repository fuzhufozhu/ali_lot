# Connect NodeMCU \(ESP8266\) to IoT Platform {#concept_xcr_2yx_ngb .concept}

You can use NodeMCU to develop IoT applications and connect your NodeMCU to Alibaba Cloud IoT Platform. In this way, you can control your NodeMCU application server from IoT Platform. This topic describes how to connect a NodeMCU application server to IoT Platform by using C programming code in an example.

## Background information {#section_dcg_cdy_ngb .section}

NodeMCU is a development board based on the ESP8266 chip. The ESP8266 chip integrates the Wi-Fi function. Therefore, the NodeMCU application server can be used to control its connected devices in the same LAN. However, if you cannot directly operate the NodeMCU application server, you cannot control the devices. To resolve this issue, connect your NodeMCU application server to Alibaba Cloud IoT Platform so that you can control the NodeMCU application server from IoT Platform.

The following section illustrates how to use NodeMCU to control a light bulb device.

**Note:** Connect the data cable of the sensor to the D7 pin, which corresponds to GPIO 13, on the NodeMCU development board.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156204753438110_en-US.png)

## Prerequisites {#section_lzp_ddy_ngb .section}

Before you begin, you must see the official NodeMCU IDE Tutorial for development software preparation . This example uses Arduino IDE as development software.

## Connect NodeMCU to IoT Platform {#section_z4w_2dy_ngb .section}

After the NodeMCU development environment is ready, you can connect NodeMCU to IoT Platform.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  Create a product, as shown in [Create a product](../../../../reseller.en-US/User Guide/Create products and devices/Create a product.md#).
3.  Define features for the product, as shown in [Define features](../../../../reseller.en-US/User Guide/Create products and devices/TSL/Define features.md#).

    This example uses the following properties:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156204753437986_en-US.png)

4.  Create a device, and obtain the device certificate information, including ProductKey, DeviceName, and DeviceSecret. For more information, see [Create a device](../../../../reseller.en-US/User Guide/Create products and devices/Create devices/Create a device.md#).
5.  Write code.

    1.  Connect your computer to the NodeMCU development board by using a USB cable.
    2.  Open the Arduino IDE and write esp8266.ino code.
    3.  Upload code.
    The sample code is as follows:

    **Note:** You can generate the value of MQTT\_PASSWDby using the mqttPassword encryption method described in [Establish MQTT connections over TCP](../../../../reseller.en-US/Developer Guide (Devices)/Protocols for connecting devices/Establish MQTT connections over TCP.md#).

    ``` {#codeblock_gxh_5q2_onv}
    #include <ESP8266WiFi.h>
    /*The PubSubClient 2.4.0 dependency */
    #include <PubSubClient.h>
    /* The ArduinoJson 5.13.4 dependency */
    #include <ArduinoJson.h>
    
    #define SENSOR_PIN    13
    
    /* The SSID and password for Wi-Fi connection */
    #define WIFI_SSID         "SSID"
    #define WIFI_PASSWD       "Password"
    
    
    /* Device certificate information*/
    #define PRODUCT_KEY       "The ProductKey of the device"
    #define DEVICE_NAME       "The DeviceName of the device"
    #define DEVICE_SECRET     "The DeviceSecret of the device"
    #define REGION_ID         "The region ID, such as cn-shanghai"
    
    /* The domain name and port number that do not require any modifications. */
    #define MQTT_SERVER       PRODUCT_KEY ".iot-as-mqtt." REGION_ID ".aliyuncs.com"
    #define MQTT_PORT         1883
    #define MQTT_USRNAME      DEVICE_NAME "&" PRODUCT_KEY
    
    #define CLIENT_ID         "esp8266|securemode=3,timestamp=1234567890,signmethod=hmacsha1|"
    //See "Establish MQTT connections over TCP" (https://www.alibabacloud.com/help/doc-detail/73742.html) to generate a password.
    //The plain text for encryption is a combination of parameter-value pairs in lexicographical order. For example, clientIdesp8266deviceName${deviceName}productKey${productKey}timestamp1234567890.
    //The encryption key is the DeviceSecret.
    #define MQTT_PASSWD       "f1ef80ee8add322c519a5a6bec326dc499f854b8"
    
    #define ALINK_BODY_FORMAT         "{\"id\":\"123\",\"version\":\"1.0\",\"method\":\"thing.event.property.post\",\"params\":%s}"
    #define ALINK_TOPIC_PROP_POST     "/sys/" PRODUCT_KEY "/" DEVICE_NAME "/thing/event/property/post"
    
    unsigned long lastMs = 0;
    WiFiClient espClient;
    PubSubClient  client(espClient);
    
    
    void callback(char *topic, byte *payload, unsigned int length)
    {
        Serial.print("Message arrived [");
        Serial.print(topic);
        Serial.print("] ");
        payload[length] = '\0';
        Serial.println((char *)payload);
    
    }
    
    
    void wifiInit()
    {
        WiFi.mode(WIFI_STA);
        WiFi.begin(WIFI_SSID, WIFI_PASSWD);
        while (WiFi.status() ! = WL_CONNECTED)
        {
            delay(1000);
            Serial.println("WiFi not Connect");
        }
    
        Serial.println("Connected to AP");
        Serial.println("IP address: ");
        Serial.println(WiFi.localIP());
    
    
    Serial.print("espClient [");
    
    
        client.setServer(MQTT_SERVER, MQTT_PORT);   /* Connect to the MQTT server after connecting to the Wi-Fi */
        client.setCallback(callback);
    }
    
    
    void mqttCheckConnect()
    {
        while (! client.connected())
        {
            Serial.println("Connecting to MQTT Server ...");
            if (client.connect(CLIENT_ID, MQTT_USRNAME, MQTT_PASSWD))
    
            {
    
                Serial.println("MQTT Connected!") ;
    
            }
            else
            {
                Serial.print("MQTT Connect err:");
                Serial.println(client.state());
                delay(5000);
            }
        }
    }
    
    
    void mqttIntervalPost()
    {
        char param[32];
        char jsonBuf[128];
    
        sprintf(param, "{\"idle\":%d}", digitalRead(13));
        sprintf(jsonBuf, ALINK_BODY_FORMAT, param);
        Serial.println(jsonBuf);
        boolean d = client.publish(ALINK_TOPIC_PROP_POST, jsonBuf);
        Serial.print("publish:0 Failure;1 Success");
        Serial.println(d);
    }
    
    
    void setup() 
    {
    
        pinMode(SENSOR_PIN,  INPUT);
        /* initialize serial for debugging */
        Serial.begin(115200);
        Serial.println("Demo Start");
    
        wifiInit();
    }
    
    
    // the loop function runs over and over again forever
    void loop()
    {
        if (millis() - lastMs >= 5000)
        {
            lastMs = millis();
            mqttCheckConnect(); 
    
            /*The heartbeat interval */
            mqttIntervalPost();
        }
    
        client.loop();
        if (digitalRead(SENSOR_PIN) == HIGH){
        Serial.println("Motion detected!") ;
        delay(2000);
          }
        else {
        Serial.println("Motion absent!") ;
        delay(2000);
      }
    
    }
    ```

6.  Modify the values of MQTT\_MAX\_PACKET\_SIZE and MQTT\_KEEPALIVE on the MQTT client as needed.

    1.  Locate PubSubClient in the library manager of Arduino IDE.
    2.  Open the PubSubClient.h file under src.
    3.  Modify the MQTT\_MAX\_PACKET\_SIZE and MQTT\_KEEPALIVE parameters. To set the MQTT\_MAX\_PACKET\_SIZE and MQTT\_KEEPALIVE parameters, see [Limits](../../../../reseller.en-US/Product Introduction/Limits.md#).
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156204753538000_en-US.png)


## Perform a test {#section_mwz_pj1_4gb .section}

After the development is complete, you can use the device to report properties to test whether the NodeMCU application server and IOT Platform can receive reported properties from the device.

After the device reports properties, you can view the running status of the device on the development software and in the IOT Platform console.

-   On the development software, click the serial port listener icon in the upper-right corner of the code editing page.

    You can view the running status of the program, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156204753538009_en-US.png)

-   In the IoT Platform console, go to the Device Details page of the corresponding device. On the Status tab page , view the reported properties and property history, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156204753538010_en-US.png)


