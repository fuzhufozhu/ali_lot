# Use the Java SDK over MQTT {#task_xbf_h1w_wdb .task}

This topic describes how to connect devices to Alibaba Cloud IoT Platform over the MQTT protocol. The Java SDK is used as an example.

In this demo, a Maven project is used. Install Maven first.

This demo is not made for the Android operating system. If you are using Android, see open-source library [https://github.com/eclipse/paho.mqtt.android](https://github.com/eclipse/paho.mqtt.android).

1.   Download the mqttClient SDK at [iotx-sdk-mqtt-java](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iotx-sdk-java/iotx-sdk-mqtt-java-20170526.zip). 
2.   Use IntelliJ IDEA or Eclipse to import the demo into a Maven project. 
3.   Log on to the Alibaba Cloud IoT Platform console, and select Devices. Click **View** next to the device to obtain the ProductKey, DeviceName, and DeviceSecret. 
4.   Modify and run the SimpleClient4IOT.java configuration file. 
    1.   Configure the parameters. 

        ```
        
        /** Obtain ProductKey, DeviceName, and DeviceSecret from the console */
        private static String productKey = "";
        private static String deviceName = "";
        private static String deviceSecret = "";
        /** The topics used for testing */
        private static String subTopic = "/"+productKey+"/"+deviceName+"/get";
        private static String pubTopic = "/"+productKey+"/"+deviceName+"/pub";
        ```

    2.   Connect to MQTT server. 

        ```
        
        // The client device ID. It can be specified using either MAC address or device serial number. It cannot be empty and must contain no more than 32 characters
        String clientId = InetAddress.getLocalHost().getHostAddress();
        // Authenticate the device
        Map params = new HashMap();
        params.put("productKey", productKey); // Specifies the product key that the user registered in the console
        params.put("deviceName", deviceName); // Specifies the device name that the user registered in the console
        params.put("clientId", clientId);
        String t = System.currentTimeMillis()+"";
        params.put("timestamp", t);
        // Specifies the MQTT server. If using the TLS protocol, begin the URL with SSL. If using the TCP protocol, begin the URL with TCP
        String targetServer = "ssl://"+productKey+".iot-as-mqtt.cn-shanghai.aliyuncs.com:1883";
        // Client ID format:
        String mqttclientId = clientId + "|securemode=2,signmethod=hmacsha1,timestamp="+t+"|"; // Specifies the custom device identifier. Valid characters include letters and numbers. For more information, see Establish MQTT over TCP connections (https://help.aliyun.com/document_detail/30539.html?spm=a2c4g. 11186623.6.592. R3LqNT)
        String mqttUsername = deviceName+"&"+productKey;// Specifies username format
        String mqttPassword = SignUtil.sign(params, deviceSecret, "hmacsha1");// Signature
        // Code excerpt for connecting over MQTT
        MqttClient sampleClient = new MqttClient(url, mqttclientId, persistence);
        MqttConnectOptions connOpts = new MqttConnectOptions();
        connOpts.setMqttVersion(4);// MQTT 3.1.1
        connOpts.setSocketFactory(socketFactory);
        // Configure automatic reconnection
        connOpts.setAutomaticReconnect(true);
        // If set to true, then all offline messages are cleared. These messages include all QoS 1 or QoS 2 messages that are not received
        connOpts.setCleanSession(false);
        connOpts.setUserName(mqttUsername);
        connOpts.setPassword(mqttPassword.toCharArray());
        connOpts.setKeepAliveInterval(80);// Specifies the heartbeat interval. We recommend that you set it to 60 seconds or longer
        sampleClient.connect(connOpts);
        ```

    3.   Send data. 

        ```
        
        String content = "The content of the data to be sent. It can be in any format";
        MqttMessage message = new MqttMessage(content.getBytes("utf-8"));
        message.setQos(0);// Message QoS. 0: At most once. 1: At least once
        sampleClient.publish(topic, message);// Send data to a specified topic
        ```

    4.   Receive data. 

        ```
        
        // Subscribe to a specified topic. When new data is sent to the topic, the specified callback method is called.
        sampleClient.subscribe(topic, new IMqttMessageListener() {
        @Override
        public void messageArrived(String topic, MqttMessage message) throws Exception {
        // After the device successfully subscribes to a topic, when new data is sent to the topic, the specified callback method is called.
        // If you subscribe to the same topic again, only the initial subscription takes effect.
        }
        });
        ```

        **Note:** For more information about MQTT connection parameters, see [Establish MQTT over TCP connections](intl.en-US/User Guide/Develop devices/Protocols for connecting devices/Establish MQTT over TCP connections.md#).


