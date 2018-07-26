# JAVA-SDK使用\(MQTT\) {#task_xbf_h1w_wdb .task}

本章节以JAVA版SDK为例，介绍如何让设备通过MQTT协议连接到阿里云物联网平台。

此Demo为maven工程，请先安装maven。

本Demo并不适合Android特性，如果您是Android，请参考开源库 [https://github.com/eclipse/paho.mqtt.android](https://github.com/eclipse/paho.mqtt.android)。

1.   下载mqttClient SDK，地址为[iotx-sdk-mqtt-java](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/iotx-sdk-java/iotx-sdk-mqtt-java-20170526.zip)。 
2.   使用idea或者eclipse，导入该Demo到工程里面。 
3.   登录阿里云物联网平台的控制台，选择设备管理，单击设备后的**查看**，获取三元组信息，即productKey、deviceName、deviceSecret的值。 
4.   修改SimpleClient4IOT.java配置文件，修改完成后，直接运行。 
    1.   配置参数。 

        ```
        
        /** 从控制台获取productKey、deviceName、deviceSecret信息*/
        private static String productKey = "";
        private static String deviceName = "";
        private static String deviceSecret = "";
        /** 用于测试的topic */
        private static String subTopic = "/"+productKey+"/"+deviceName+"/get";
        private static String pubTopic = "/"+productKey+"/"+deviceName+"/pub";
        ```

    2.   连接MQTT服务器。 

        ```
        
        //客户端设备 自己的一个标记 建议是mac或sn ，不能为空，32字符内
        String clientId = InetAddress.getLocalHost().getHostAddress();
        //设备认证
        Map params = new HashMap();
        params.put("productKey", productKey); //这个是对应用户在控制台注册的 设备productkey
        params.put("deviceName", deviceName); //这个是对应用户在控制台注册的 设备name
        params.put("clientId", clientId);
        String t = System.currentTimeMillis()+"";
        params.put("timestamp", t);
        //mqtt服务器 ，tls的话ssl开头，tcp的话改成tcp开头
        String targetServer = "ssl://"+productKey+".iot-as-mqtt.cn-shanghai.aliyuncs.com:1883";
        //客户端ID格式:
        String mqttclientId = clientId + "|securemode=2,signmethod=hmacsha1,timestamp="+t+"|"; //设备端自定义的标记，字符范围[0-9][a-z][A-Z]，[MQTT-TCP连接通信](https://help.aliyun.com/document_detail/30539.html?spm=a2c4g.11186623.6.592.R3LqNT "MQTT-TCP连接通信")
        String mqttUsername = deviceName+"&"+productKey;//mqtt用户名格式
        String mqttPassword = SignUtil.sign(params, deviceSecret, "hmacsha1");//签名
        //连接mqtt的代码片段
        MqttClient sampleClient = new MqttClient(url, mqttclientId, persistence);
        MqttConnectOptions connOpts = new MqttConnectOptions();
        connOpts.setMqttVersion(4);// MQTT 3.1.1
        connOpts.setSocketFactory(socketFactory);
        //设置是否自动重连
        connOpts.setAutomaticReconnect(true);
        //如果是true 那么清理所有离线消息，即qos1 或者 2的所有未接收内容
        connOpts.setCleanSession(false);
        connOpts.setUserName(mqttUsername);
        connOpts.setPassword(mqttPassword.toCharArray());
        connOpts.setKeepAliveInterval(80);//心跳时间 建议60s以上
        sampleClient.connect(connOpts);
        ```

    3.   数据发送。 

        ```
        
        String content = "要发送的数据内容, 这个内容可以是任意格式";
        MqttMessage message = new MqttMessage(content.getBytes("utf-8"));
        message.setQos(0);//消息qos 0:最多一次，1:至少一次
        sampleClient.publish(topic, message);//发送数据到某个topic
        ```

    4.   数据接收。 

        ```
        
        //订阅某个topic，一旦有数据会回调到这里
        sampleClient.subscribe(topic, new IMqttMessageListener() {
        @Override
        public void messageArrived(String topic, MqttMessage message) throws Exception {
        //设备订阅topic成功后，数据会回调这里
        //重复订阅无影响
        }
        });
        ```

        **说明：** MQTT连接参数详细说明，请参见[MQTT-TCP连接通信](intl.zh-CN/用户指南/设备开发指南/设备多协议连接/MQTT-TCP连接通信.md#)。


