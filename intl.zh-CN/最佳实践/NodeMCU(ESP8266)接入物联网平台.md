# NodeMCU\(ESP8266\)接入物联网平台 {#concept_xcr_2yx_ngb .concept}

您可以使用NodeMCU开发物联网应用，并将您的NodeMCU接入阿里云物联网平台，便可通过阿里云物联网平台远程控制您的NodeMCU应用服务器。本文档以C语言代码为例，介绍将NodeMCU应用服务器接入物联网平台的配置过程。

## 背景信息 {#section_dcg_cdy_ngb .section}

NodeMCU是基于ESP8266芯片的开发板。ESP8266芯片集成了Wifi功能，因此使用NodeMCU应用服务器，可在同一局域网内对设备进行控制。但是，如果您无法直接控制操作NodeMCU服务器的情况下，便不能控制设备。因此，我们提供的解决方案是：将您的NodeMCU服务器接入阿里云物联网平台，这样便可通过物联网平台远程控制NodeMCU服务器。

本文档中以NodeMCU控制一个灯泡设备为例。

**说明：** 将传感器数据线连接NodeMCU开发板上的D7引脚（对应GPIO 13）。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156291751038110_zh-CN.png)

## 前提条件 {#section_lzp_ddy_ngb .section}

使用NodeMCU开发应用并接入物联网平台之前，您需参见NodeMCU官方教程集成开发环境。本文示例中使用的开发软件是Arduino IDE。

## 接入物联网平台 {#section_z4w_2dy_ngb .section}

NodeMCU开发环境准备好后，您便可以开始接入物联网平台的操作和配置。

1.  登录[物联网平台控制台](https://iot.console.aliyun.com/product/region/cn-shanghai)。
2.  创建产品。请参见[创建产品](../../../../intl.zh-CN/用户指南/产品与设备/创建产品.md#)。
3.  为产品定义功能。请参见[新增物模型](../../../../intl.zh-CN/用户指南/产品与设备/物模型/新增物模型.md#)。

    本文以如下属性为例：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156291751137986_zh-CN.png)

4.  创建设备，并获取设备证书信息（ProductKey、DeviceName和DeviceSecret）。请参见[单个创建设备](../../../../intl.zh-CN/用户指南/产品与设备/创建设备/单个创建设备.md#)。
5.  编写代码。

    1.  用USB线连接开发电脑和NodeMCU开发板。
    2.  打开Arduino IDE，编写esp8266.ino 代码。
    3.  上传代码。
    esp8266.ino代码示例：

    **说明：** MQTT连接参数，可参见[MQTT接入——MQTT-TCP连接通信](../../../../intl.zh-CN/设备端开发指南/设备多协议连接/MQTT接入——MQTT-TCP连接通信.md#)。

    ``` {#codeblock_rxk_emm_re3}
    #include <ESP8266WiFi.h>
    /* 依赖 PubSubClient 2.4.0 */
    #include <PubSubClient.h>
    /* 依赖 ArduinoJson 5.13.4 */
    #include <ArduinoJson.h>
    
    #define SENSOR_PIN    13
    
    /* 连接您的WIFI SSID和密码 */
    #define WIFI_SSID         "路由器SSID"
    #define WIFI_PASSWD       "密码"
    
    
    /* 设备证书信息*/
    #define PRODUCT_KEY       "替换为设备的ProductKey"
    #define DEVICE_NAME       "替换为设备的DeviceName"
    #define DEVICE_SECRET     "替换为设备的DeviceSecret"
    #define REGION_ID         "替换为设备所在地域ID如cn-shanghai"
    
    /* 线上环境域名和端口号，不需要改 */
    #define MQTT_SERVER       PRODUCT_KEY ".iot-as-mqtt." REGION_ID ".aliyuncs.com"
    #define MQTT_PORT         1883
    #define MQTT_USRNAME      DEVICE_NAME "&" PRODUCT_KEY
    
    #define CLIENT_ID         "esp8266|securemode=3,timestamp=1234567890,signmethod=hmacsha1|"
    // MQTT连接报文参数,请参见MQTT-TCP连接通信文档，文档地址：https://www.alibabacloud.com/help/doc-detail/73742.html
    // 加密明文是参数和对应的值（clientIdesp8266deviceName${deviceName}productKey${productKey}timestamp1234567890）按字典顺序拼接
    // 密钥是设备的DeviceSecret
    #define MQTT_PASSWD       "f1ef80ee8add322c519a5a6bec326dc499f8****"
    
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
        while (WiFi.status() != WL_CONNECTED)
        {
            delay(1000);
            Serial.println("WiFi not Connect");
        }
    
        Serial.println("Connected to AP");
        Serial.println("IP address: ");
        Serial.println(WiFi.localIP());
    
    
    Serial.print("espClient [");
    
    
        client.setServer(MQTT_SERVER, MQTT_PORT);   /* 连接WiFi之后，连接MQTT服务器 */
        client.setCallback(callback);
    }
    
    
    void mqttCheckConnect()
    {
        while (!client.connected())
        {
            Serial.println("Connecting to MQTT Server ...");
            if (client.connect(CLIENT_ID, MQTT_USRNAME, MQTT_PASSWD))
    
            {
    
                Serial.println("MQTT Connected!");
    
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
        Serial.print("publish:0 失败;1成功");
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
    
            /* 上报消息心跳周期 */
            mqttIntervalPost();
        }
    
        client.loop();
        if (digitalRead(SENSOR_PIN) == HIGH){
        Serial.println("Motion detected!");
        delay(2000);
          }
        else {
        Serial.println("Motion absent!");
        delay(2000);
      }
    
    }
    ```

6.  修改MQTT客户端上MQTT\_MAX\_PACKET\_SIZE值为1024，修改MQTT\_KEEPALIVE值为60。您可以根据业务需要修改为合适的值。

    1.  在Arduino IDE的库管理器中找到PubSubClient。
    2.  打开src下的PubSubClient.h文件。
    3.  修改消息长度限制和MQTT连接保活时长。设置消息长度限制和MQTT连接保活时长，请参见[使用限制](../../../../intl.zh-CN/产品简介/使用限制.md#)。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156291751138000_zh-CN.png)


## 测试 {#section_mwz_pj1_4gb .section}

开发完成后，可使用设备上报属性，测试NodeMCU应用服务器和物联网平台是否能接收到设备上报的属性值。

设备上报属性数据后，您可在开发软件上和物联网平台控制台查看设备运行状态。

-   在开发软件上，单击代码编辑页面右上角的串口监听图标，查看程序运行情况。

    运行效果如下图：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156291751138009_zh-CN.png)

-   在物联网平台控制台，对应的设备详情页运行状态页签栏下查看设备上报的属性值和历史属性数据。如下图：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/117120/156291751138010_zh-CN.png)


