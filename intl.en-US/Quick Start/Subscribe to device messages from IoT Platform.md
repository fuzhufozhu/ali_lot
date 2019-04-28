# Subscribe to device messages from IoT Platform {#task_xy5_wk2_vdb .task}

After a device is connected to IoT Platform, the device directly reports data to IoT Platform. Then, the data is forwarded to your server over an HTTP/2 connection. This topic describes how to configure the service subscription function. You can connect your server to an HTTP/2 SDK to receive device data.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17309/15564468548965_en-US.png)

1.  Configure the service subscription function for your product in the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot). 

    1.  On the Products page, click **View** next to the target product. 
    2.  On the Product Details page, click **Service Subscription** \> **Set**. 
    3.  Select the types of messages to which you want to subscribe, and click **Save**. 

        |Message type|Description|
        |:-----------|:----------|
        |Device Upstream Notification|Indicates the custom data and TSL model data that are reported by the device. The data includes property data, event data, property setting responses, and service call responses.|
        |Device Status Change Notifications|Indicates the notifications that are sent by the system when the status of a device changes. For example, the connection and disconnection notifications.|
        |Device Changes Throughout Lifecycle|Indicates the notifications about device creation, deletion, disabling, and enabling.|
        |Sub-Device Data Report Detected by Gateway|A gateway reports the information about the discovered sub-devices to IoT Platform. Make sure that the gateway has an application that can discover and report sub-device information.|
        |Device Topological Relation Changes|Indicates notifications about the creation and removal of the topological relationships between a gateway and its sub-devices.|

    After you configure service subscription in the console, it takes approximately 1 minute for the settings to take effect.

2.  Add dependencies. 

    If you use Apache Maven to manage Java projects, you must add the following dependencies to the pom.xml file.

    **Note:** Currently, only Java 8 and .NET SDKs are supported. For more information about SDK configuration, see [Development guide for Java HTTP/2 SDK](../../../../reseller.en-US/User Guide/Create products and devices/Service Subscription/Development guide for Java HTTP__2 SDK.md#) or [Development guide for .NET HTTP/2 SDK](../../../../reseller.en-US/User Guide/Create products and devices/Service Subscription/Development guide for .NET HTTP__2 SDK.md#).

    ```
    <dependency>
        <groupId>com.aliyun.openservices</groupId>
        <artifactId>iot-client-message</artifactId>
        <version>1.1.3</version>
    </dependency>
    
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-core</artifactId>
        <version>3.7.1</version>
    </dependency>
    ```

3.  Use the AccessKey information of your Alibaba Cloud account for identity authentication and connect the HTTP/2 SDK to IoT Platform. 

    ```
    //AccessKey ID of your Alibaba Cloud account
            String accessKey = "xxxxxxxxxxxxxxx";
            //AccessKey Secret of your Alibaba Cloud account
            String accessSecret = "xxxxxxxxxxxxxxx";
            // Region ID of your IoT Platform service
            String regionId = "cn-shanghai";
            //User ID of your Alibaba Cloud account
            String uid = "xxxxxxxxxxxx";
            //Endpoint: https://${uid}.iot-as-http2.${region}.aliyuncs.com 
            String endPoint = "https://" + uid + ".iot-as-http2." + regionId + ".aliyuncs.com";
    
            //Connection configuration
            Profile profile = Profile.getAccessKeyProfile(endPoint, regionId, accessKey, accessSecret);
    
            //Construct the client
            MessageClient client = MessageClientFactory.messageClient(profile);
    
            //Receive data
            client.connect(messageToken -> {
                Message m = messageToken.getMessage();
                System.out.println("receive message from " + m);
                return MessageCallback.Action.CommitSuccess;
            });
    ```

    Parameter description

    |Parameter|Description|
    |:--------|:----------|
    |accessKey|The AccessKey ID of your Alibaba Cloud account.To obtain the AccessKey ID, log on to the Alibaba Cloud console, hover over your account avatar, and click **AccessKey**. You are redirected to the Security Management page of the User Management console.

|
    |accessSecret|The AccessKey Secret of your Alibaba Cloud account. Obtain the AccessKey Secret in the same way you obtain the AccessKey ID.|
    |uid|The account ID.To obtain the account ID, log on to the Alibaba Cloud console by using your Alibaba Cloud account, and click the account avatar. You are redirected to the Security Settings page of the Account Management console.

|
    |regionId|The region ID of your IoT Platform service.In the IoT Platform console, you can view the region in the left corner of the top navigation bar. For more information about regions, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

|

4.  Verify that the HTTP/2 SDK can receive messages from the device. 

    If messages can be received, you can obtain the following data from the message callback of the SDK.

    |Parameter|Description|
    |:--------|:----------|
    |messageId|The message ID generated by IoT Platform .|
    |topic|The source topic of the message.|
    |payload|The payload of the message. For more information, see [Data format](../../../../reseller.en-US/User Guide/Rules/Data Forwarding/Data format.md#).|
    |generateTime|The timestamp when the message was generated, in milliseconds.|
    |qos|     -   0: The message will be delivered only once.
    -   1: The message will be delivered at least once.
 |


