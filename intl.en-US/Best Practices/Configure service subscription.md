# Configure service subscription {#concept_hns_154_1fb .concept}

In some IoT scenarios, device status information and device collected data are expected to be directly received by business servers rather than be forwarded by cloud services. In such a case, you can use the [service subscription](../../../../../reseller.en-US/User Guide/Create products and devices/Service Subscription/What is Service Subscription?.md#) function. You can push device messages to the business servers directly through the HTTP/2 channel, so that users can consume data based on their own business scenarios.

This topic describes how to use service subscription for data forwarding by using a hygrothermograph in an example to construct a client.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20197/155316792811292_en-US.png)

## Prerequisites {#section_mch_z54_1fb .section}

Before you begin, create a product and a device in the IoT Platform console and connect the device to IoT Platform. For more information, see [Quick Start](../../../../../reseller.en-US/Quick Start/Create products and devices.md#).

## Configure service subscription {#section_myj_zyt_1fb .section}

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com/product/region/cn-shanghai), and choose **Devices** \> Product from the left-side navigation pane.
2.  In the product list, locate the product for which you want to configure service subscription and click **View**. The Product Details page appears.
3.  Choose **Service Subscription** \> **Set**.
4.  In the Configure Service Subscription dialog box, select **Device Upstream Notification**, and click **Save**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20197/155316792811352_en-US.png)


## Use the service subscription SDK {#section_odb_vv4_1fb .section}

**Note:** IoT Platform supports Java 8 SDK.

1.  Add the Java SDK dependencies as follows:

    ```
    <! -- Aliyun core -->
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-core</artifactId>
        <version>3.7.1</version>
    </dependency>
    
    <! -- iot message client -->
    <dependency>
        <groupId>com.aliyun.openservices</groupId>
        <artifactId>iot-client-message</artifactId>
        <version>1.1.2</version>
    </dependency>
    ```

2.  Connect the SDK to IoT Platform using your Alibaba Cloud account information for identity authentication.

    ```
    import java.net.UnknownHostException;
    import java.util.concurrent.ExecutionException;
    
    import com.aliyun.openservices.iot.api.Profile;
    import com.aliyun.openservices.iot.api.message.MessageClientFactory;
    import com.aliyun.openservices.iot.api.message.api.MessageClient;
    import com.aliyun.openservices.iot.api.message.callback.MessageCallback;
    import com.aliyun.openservices.iot.api.message.entity.Message;
    
    public class H2Client {
    
        public static void main(String[] args) throws UnknownHostException, 
                                    ExecutionException, InterruptedException {
            // Identity information
            String accessKey = "Your AccessKey ID";
            String accessSecret = "Your AccessKey Secret>";
            String regionId = "cn-shanghai";
            String uid = "Your Alibaba Cloud UID";
            String endPoint = "https://" + uid + ".iot-as-http2." + regionId + ".aliyuncs.com";
            //Configure connection parameters
            Profile profile = Profile.getAccessKeyProfile(endPoint, regionId, accessKey, accessSecret);
    
            //Construct a client
            MessageClient client = MessageClientFactory.messageClient(profile);
            //Receive data
            client.connect(messageToken -> {
                Message m = messageToken.getMessage();
                System.out.println("\ntopic="+m.getTopic());
                System.out.println("payload=" + new String(m.getPayload()));
                System.out.println("generateTime=" + m.getGenerateTime());
                // CommitSuccess indicates that the current message has been consumed and will be deleted by IoT Platform. If the message is not marked with CommitSuccess, the message will be retained until it expires.
                return MessageCallback.Action.CommitSuccess;
            });
        }
    }
    ```

3.  The device reports the message to IoT Platform and the client receives the device message.

## Data forwarding example {#section_ztr_215_1fb .section}

-   Sample data of devices without TSL models

    The device comes online, publishes a temperature and humidity message, and goes offline.

    ```
    {
        "temperature": 27,
        "humidity": 26
    }
    ```

    The business server receives the following messages:

    ```
    # Device connection message
    topic=/as/mqtt/status/a1wLOMQOK4K/temperature-Monitor
    payload={
        "lastTime": "2018-09-04 14:59:59.782",
        "utcLastTime": "2018-09-04T06:59:59.782Z",
        "clientIp": "42.120.75.152",
        "utcTime": "2018-09-04T06:59:59.794Z",
        "time": "2018-09-04 14:59:59.794",
        "productKey": "a1wLOMQOK4K",
        "deviceName": "temperature-Monitor",
        "status": "online"
    }
    generateTime=1536044399797
    
    # Data message reported by the device
    topic=/a1wLOMQOK4K/temperature-Monitor/temperatureData
    payload={
        "temperature": 27,
        "humidity": 26
    }
    generateTime=1536044404762
    
    # Device disconnection message
    topic=/as/mqtt/status/a1wLOMQOK4K/temperature-Monitor
    payload={
        "lastTime": "2018-09-04 15:00:09.761",
        "utcLastTime": "2018-09-04T07:00:09.761Z",
        "utcTime": "2018-09-04T07:00:13.337Z",
        "time": "2018-09-04 15:00:13.337",
        "productKey": "a1wLOMQOK4K",
        "deviceName": "temperature-Monitor",
        "status": "offline"
    }
    generateTime=1536044413339
    ```

    **Note:** The service subscription function is based on the UID of your Alibaba Cloud account. All messages of the products that are created by this account will be received by the HTTP/2 client. You can differentiate service consumption among business servers by the ProductKey and DeviceName information that is contained in the topic.

-   Sample data of devices using TSL models

    The device comes online, publishes a temperature and humidity message, and goes offline.

    ```
    {
        "id": 1536045963829,
        "params":{
            "temperature": 20,
            "humidity": 22
        },
        "method": "thing.event.property.post"
    }
    ```

    The business server receives the following messages:

    ```
    # Device connection message
    topic=/as/mqtt/status/a1gMuCoK4m2/nuwr5r9hf6l
    payload={
        "lastTime": "2018-09-04 15:25:53.771",
        "utcLastTime": "2018-09-04T07:25:53.771Z",
        "clientIp": "42.120.75.152",
        "utcTime": "2018-09-04T07:25:53.787Z",
        "time": "2018-09-04 15:25:53.787",
        "productKey": "a1gMuCoK4m2",
        "deviceName": "nuwr5r9hf6l",
        "status": "online"
    }
    generateTime=1536045953787
    
    # Device-reported data message that is parsed based on the TSL model
    topic=/a1gMuCoK4m2/nuwr5r9hf6l/thing/event/property/post
    payload={
        "deviceType": "None",
        "iotId": "hutrRN9iuBvO5QOclH4f0010885100",
        "productKey": "a1gMuCoK4m2",
        "gmtCreate": 1536045958765,
        "deviceName": "nuwr5r9hf6l",
        "items":{
            "temperature":{
                "value": 20,
                "time": 1536045958770
            },
            "humidity":{
                "value": 22,
                "time": 1536045958770
            }
        }
    }
    generateTime=1536045958770
    
    # Device disconnection message
    topic=/as/mqtt/status/a1gMuCoK4m2/nuwr5r9hf6l
    payload={
        "lastTime": "2018-09-04 15:26:03.749",
        "utcLastTime": "2018-09-04T07:26:03.749Z",
        "utcTime": "2018-09-04T07:26:06.586Z",
        "time": "2018-09-04 15:26:06.586",
        "productKey": "a1gMuCoK4m2",
        "deviceName": "nuwr5r9hf6l",
        "status": "offline"
    }
    generateTime=1536045966588
    ```

    **Note:** The messages of devices using TSL are not in the original format. The data will be parsed based on TSL models.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20197/155316792811294_en-US.png)


