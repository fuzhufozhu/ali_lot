# Servers subscribe to device messages {#task_xy5_wk2_vdb .task}

When devices are connected to IoT Platform, they report data to the platform. Data in the platform can be pushed to your server through a HTTP/2 channel. In this step, we set the service subscription through HTTP/2 and configure for HTTP/2 SDKs. You can then connect your server to an HTTP/2 SDK and the server can receive device data.![](images/8965_en-US.png)

1.  In the [IoT Platform console](https://iot.console.aliyun.com/product/region/cn-shanghai), you can set the service subscription for a product. 

    1.  On the Products page, find the product for which you want to set the service subscription and click **View**. 
    2.  On the product details page, click **Service Subscription**, and then click **Set Now**. 
    3.  Select the types of notifications which you want to push to your server \(HTTP/2 SDK\) and click **Save**. 
        -   Device Upstream Notification: Indicates messages of the topics in the device topic list, whose **Device Authorizations** are**Publish**.
        -   Device Status Change Notification: Indicates the notifications that are sent by the system when the statuses of devices change. For example, the notifications upon devices going online or going offline.
    The subscription configuration in the console takes effect about one minute after the configuration is completed.

2.  Connect to the HTTP/2 SDK. If you use Apache Maven to manage Java projects, you add the following dependency content to the pom.xml file.

    ```
    <dependency>
        <groupId>com.aliyun.openservices</groupId>
        <artifactId>iot-client-message</artifactId>
        <version>1.1.0</version>
    </dependency>
    
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-core</artifactId>
        <version>3.7.1</version>
    </dependency>
    ```

3.  Connect the SDK and IoT Platform using the AccessKey information of your Alibaba Cloud account for identity authentication. 

    ```
    String accessKey = "xxxxxxxxxxxxxxxxx";
    String accessSecret = "xxxxxxxxxxxxxxx";
    String uid = "xxxxxxxxxxxxxxxxx";
    String region = "cn-shanghai";
    String endPoint = "https://${uid}.iot-as-http2.${region}.aliyuncs.com"
    Profile profile = new Profile(endPoint, region, accessKey, accessSecret);
    MessageClient client = MessageClientFactory.messageClient(profile);
    client.connect(messageToken -> {
        Message m = messageToken.getMessage();
        System.out.println("receive message from " + m);
        return MessageCallback.Action.CommitSuccess;
    });
    ```

    The parameters are introduced as follows:

    -   accessKey and accessSecret: Log on to the Alibaba Cloud console, move the pointer to your account image, and click **AccessKey**. You are directed to the User Management page and you can create a new AccessKey or view the AccessKey ID and AccessKey Secret of an existing AccessKey on this page.
    -   uid: Log on to the Alibaba Cloud console, move the pointer to your account image, and click **Security Settings**. You are directed to the Account Management page and you can view your account ID on this page.
    -   region: The region of your IoT Platform service. For region IDs, see [Regions and zones](https://help.aliyun.com/document_detail/40654.html).
4.  Test if the HTTP/2 SDK can successfully receive device messages by simulating a device to push messages using the following sample codes. 
    1.  Push a test message `hello world!` from a simulated device to the topic`/a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/data`. Compile the message in `iotkit-embedded/sample/mqtt/mqtt-example.c` of the C SDK on a Linux system. The code example is as follows: 

        ```
        #define TOPIC_DATA "/"PRODUCT_KEY"/"DEVICE_NAME"/data"
        #define PRODUCT_KEY "a1wmrZPO8o9"
        #define DEVICE_NAME "cbgiotkJ4O4WW59ivysa"
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

    2.  Execute the make command using the path `iotkit-embedded` to compile the device SDK. The statement format is as follows: 

        ```
        make distclean
        ```

        ```
        make
        ```

    3.  Runs the updated sample program using the path:`iotkit-embedded/output/release/bin`. 
    4.  Check if the cloud applications can successfully receive the message. If the message is successfully received, you can obtain the following data from the message callback of the SDK. 

        |Parameter|Description|
        |---------|-----------|
        |messageId|A 19-bit message ID generated by IoT Platform .|
        |topic|The topic from which the message is sent. In this example, the topic is `/a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/data`. If the message is a device status change notification, the topic format is `/as/mqtt/status/${productKey}/${deviceName}`.|
        |payload|         -   The binary data that a Pro Edition device publishes to a topic.
        -   If the message is a device status change notification, the format is as follows:

            ```
{ 
"status":"online" (or offline), //the device status 
"productKey":"xxxxxxxxxxx", //In the above example, the ProductKey is a1wmrZPO8o9 
"deviceName":"xxxxxxxxxx", //In the above example, the DeviceName is cbgiotkJ4O4WW59ivysa 
"time":"2018-08-31 15:32:28.205", //The time when the notification is sent 
"utcTime":"2018-08-31T07:32:28.205Z",//The UTC time when the notification is sent 
"lastTime":"2018-08-31 15:32:28.195", //The time when the last message communication occurred before the status change 
"utcLastTime":"2018-08-31T07:32:28.195Z",//The UTC time when the last message communication occurred before the status change
"clientIp":"xxx.xxx.xxx.xxx" //the public IP address of the device. 
}
            ```

 |
        |generateTime|The timestamp of the message generation, in millisecond.|
        |qos|         -   0: The message is only pushed one time.
        -   1: The message is pushed at least one time.
 |


