# Cloud services receive data from devices {#task_xy5_wk2_vdb .task}

In this step,  your devices can report data to the cloud, and cloud applications can get messages from devices by monitoring the MNS queues.

1.   In the console, configure the service subscriptions for products in order to enable automatic message forwarding from the platform to the MNS queues. 

    1.   Click Products, and then click **View** to view the product that you created. 
    2.   On the left-side navigation pane, click Subscriptions, click **Configure**, and then select the **Device Upstream Notification** and **Device Status Update Notification** check boxes. 
        -   The Device Upstream Notification option indicates that the platform automatically forwards the data reported by devices to the MNS service.
        -   The Device Status Update Notification option indicates that the platform will automatically push a notification to the MNS service when the device status \(online or offline\) changes.
    Once you subscribe to the MNS service, the MNS service will automatically create a message queue in the China \(Shanghai\) region. For example, the name is `aliyun-iot-a1wmrZPO8o9`, where `a1wmrZPO8o9` is the productKey of the device.

    The configuration will take effect after one minute.

2.   The cloud applications receive device data by monitoring the MNS queues. In this example, assuming that your cloud applications access the MNS service using the Java SDK, and subscribe to topics in queue mode. The process involves the following information: 

    1.   A project file pom.xml has the following dependencies:  

        ```
        <dependency>
                  <groupId>com.aliyun.mns</groupId>
                  <artifactId>aliyun-sdk-mns</artifactId>
                  <version>1.1.5</version>
        </dependency>
        
        ```

    2.   You need to specify the following information when receiving a message: 

        ```
        CloudAccount account = new CloudAccount ($AccessKeyId, $AccessKeySecret, $AccountEndpoint);
        ```

        -   The AccessKeyId and AccessKeySecret are required for you to access APIs. You can find them in your profile by clicking the Alibaba Cloud account avatar.
        -   The AccountEndpoint can be found in the MNS console. Select the **China \(Shanghai\)** region, locate the message queue that IoT Platform has created, and click **Get Endpoint**.
    3.   You will also need to specify information such as the IoT Message Queue, for example: 

        ```
        MNSClient client = account.getMNSClient();
        CloudQueue queue = client.getQueueRef("aliyun-iot-a1wmrZPO8o9"); //Input the queue that is automatically created as the parameter.
        
        
        while (true) {
        //Get messages
        Message popMsg = queue.popMessage(10);  //The timeout for long polling is 10 senconds
        if (popMsg ! = null) {
        System.out.println("PopMessage Body: "
        + popMsg.getMessageBodyAsRawString()); //Get the original messages.
        queue.deleteMessage(popMsg.getReceiptHandle()); //Delete messages from the queue.
        } else {
        System.out.println("Continuing");
        }
        }
        ```

    4.   Start to monitor the MNS queues. 
    **Note:** For more information about how to monitor the MNS queues, see [MNS](https://help.aliyun.com/product/27412.html).

3.   Upload test messages and run sample programs to verify that the cloud services can receive messages by monitoring the MNS queues. 
    1.   Upload a test message from a simulated device to the topic  `/a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/data`. For example, send a `hello world!` message. You need to edit the message to be uploaded using the path:  `iotkit-embedded/sample/mqtt/mqtt-example.c`  with the C SDK on a Linux device. The code example is as follows: 

        ```
        #define TOPIC_DATA "/"PRODUCT_KEY"/"DEVICE_NAME"/data"
        /* Initialize topic information */
        memset(&topic_msg, 0x0, sizeof(iotx_mqtt_topic_info_t));
        strcpy(msg_pub, "message: hello world!") ;
        topic_msg.qos = IOTX_MQTT_QOS1;
        topic_msg.retain = 0;
        topic_msg.dup = 0;
        topic_msg.payload = (void *)msg_pub;
        topic_msg.payload_len = strlen(msg_pub);
        EXAMPLE_TRACE("\n publish message: \n topic: %s\n payload: \%s\n", TOPIC_DATA, topic_msg.payload);
        rc = IOT_MQTT_Publish(pclient, TOPIC_DATA, &topic_msg);
        EXAMPLE_TRACE("rc = IOT_MQTT_Publish() = %d", rc);
        ```

    2.   Execute the make command using the path `iotkit-embedded` to compile the device SDKs. The command is as follows: 

        ```
        make distclean
        ```

        ```
        make
        ```

    3.   Runs the updated sample program using the path: `iotkit-embedded/output/release/bin`. 

        The output log of sent data is as follows:

        ```
        mqtt_client|246:: 
        publish message:
        topic: /a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/data
        payload: message: hello world!
        ```

    4.   Check if the cloud applications can successfully monitor the above message. If the monitoring is successful, 
        -   the cloud applications will receive a message as follows:

            ```
            {
            "messageid":" ", //Message ID
            "messagetype":"status",
            "topic":"/a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/data",
            "payload": {hello world},
            "timestamp": //the timestamp
            }
            ```

            -   messageid: The message ID generated by IoT Platform, which is 64-bit size.
            -   messagetype: the message types, including notification, status and upload.
            -   Topic: the topic the monitored messages are from. The type of topic is identified by messagetype. When the messagetype = status, it is an empty topic. When the messagetype = upload, it is a specific topic. In this example, the topic is `/a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/data`.
            -   payload: **Base64 Encode data**. The type of data is identified by messagetype. When the messagetype = status, it indicates that IoT Platform sends a notification. When the messagetype = upload, it indicates that the device publishes data to IoT Platform.
            -   Timestamp: The timestamp, expressed in Epoch time.
        -   The following is a message example:

            ```
            data=
            {
            "status":"online" (or offline), //the device status
            "productKey":"xxxxxxxxxxx", //In this document, the ProductKey is: a1wmrZPO8o9
            "deviceName":"xxxxxxxxxx", //In this document, the DeviceName: cbgiotkJ4O4WW59ivysa
            "time":"2017-10-11 10:11:12.234", //the time that the notification is sent
            "lastTime":"2017-10-11 10:11:12.123" //the point in time that the device communicates with IoT Platform before the status change.
            "clientIp":"xxx.xxx.xxx.xxx" //the public IP address of the device.
            }
            ```


