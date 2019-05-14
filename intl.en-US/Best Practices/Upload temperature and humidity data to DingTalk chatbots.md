# Upload temperature and humidity data to DingTalk chatbots {#task_z5x_53d_zdb .task}

-   Scenario:

    Upload the data that is collected by the temperature and humidity sensor to DingTalk chatbots.

-   Business logic:

    Connect the temperature and humidity sensor to IoT Platform through MQTT. Configure the rules engine to send the temperature and humidity data to the pushData2DingTalk function in Function Compute. The function processes the data and posts the results to the Webhook address of the specified DingTalk chatbot. The DingTalk group can then receive the temperature and humidity data sent by this DingTalk chatbot.

    The data flow diagram is shown in [Figure 1](#fig_oqy_5md_zdb).

     ![](images/4658_en-US.png "Data flow diagram")


1.  Configure the DingTalk chatbot. 
    1.  Log on to the desktop version of DingTalk.
    2.  In the upper-right corner of the DingTalk group page, click `...`, and then select **ChatBot**.
    3.  Click **Add Robot**, select **Custom**, then click **Add**.
    4.  Enter a name for the robot, click **Next**, and then click **Finish**.
2.  Create a function. 
    1.  Activate Alibaba Cloud Function Compute service first. 

        Function Compute is an event-driven, fully managed computing service. Currently supported languages include Java, Node.js and Python. For more information, see [How to use Function Compute](https://www.alibabacloud.com/help/doc-detail/51733.htm).

    2.  Write the function script code. In this example, Node.js is used. The function obtains the device location, device number, temperature, humidity, and the time of recording from IoT Platform, and splices the data according to the specified DingTalk message format. Data will be sent to the Webhook address of the specified DingTalk chatbot using HTTPS Post methods.
    3.  Log on to the Function Compute console and create a service named IoT\_Service.
    4.  Click **Create Function**, and select the **Empty Function** template.
    5.  Select No Trigger and specify the basic configurations as shown in the following picture. 

        ![](images/4698_en-US.png "Basic configurations")

        The function pushData2DingTalk is declared as follows:

        ```
        
        const https = require('https');
        const accessToken = 'Specify the webhook accessToken of the DingTalk robot';
        module.exports.handler = function(event, context, callback) {
        var eventJson = JSON.parse(event.toString());
        //DingTalk message format
        const postData = JSON.stringify({
        "msgtype": "markdown",
        "markdown": {
        "title": "Temperature and humidity sensor",
        "text": "#### Temperature and humidity details\n" +
        "> Device location: " + eventJson.tag + "\n\n" +
        "> Device number: " + eventJson.isn+ "\n\n" +
        "> Temperature: " + eventJson.temperature + "℃\n\n" +
        "> Humidity: " + eventJson.humidity + "%\n\n" +
        "> ###### " + eventJson.time + " published by [Alibaba Cloud IoT Platform](https://www.aliyun.com/product/iot) \n"
        },
        "at": {
        "isAtAll": false
        }
        });
        const options = {
        hostname: 'oapi.dingtalk.com',
        port: 443,
        path: '/robot/send? access_token=' + accessToken,
        method: 'POST',
        headers: {
        'Content-Type': 'application/json',
        'Content-Length': Buffer.byteLength(postData)
        }
        };
        const req = https.request(options, (res) => {
        res.setEncoding('utf8');
        res.on('data', (chunk) => {});
        res.on('end', () => {
        callback(null, 'success');
        });
        });
        // When an exception returns
        req.on('error', (e) => {
        callback(e);
        });
        // Write the data
        req.write(postData);
        req.end();
        };
        ```

3.  Configure IoT Platform. 
    1.  In the IoT Platform console, select **Products** and then create a product for the temperature and humidity sensor.
    2.  On the Topic Categories tab page of the product details page, create a topic category `/productKey/${deviceName}/user/data` whose Device Operation Authorizations is Publish.
    3.  On the Define Feature tab page, define two properties: temperature and humidity.
    4.  Select **Devices** and then register a device.
    5.  Click **View** next to the device name. On the Device Tag tab, click **Add** to add two device tags. 

        |Tag Key|Tag Value|Description|
        |-------|---------|-----------|
        |tag|Room 007S, 3rd Floor, Building 2, Cloud Town|Device location|
        |deviceISN|T20180102XnbKjmoAnUb|Device number|

    6.  Select Rules to create and enable a rule. A complete rule contains three parts: basic information, data processing SQL, and data forwarding. You can specify multiple forwarding actions for a rule. 
        1.  Configure the data processing script.

            The rules engine supports [SQL statements](../../../../reseller.en-US/User Guide/Rules/Data Forwarding/SQL statements.md#).

            The device name \(deviceName\) and attributes \(tag and deviceISN\) are required.

            This SQL is to obtain the temperature and humidity values from the payload of the messages that are sent from the temperature and humidity sensor.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7645/15578128494798_en-US.png)

            The SQL statements are as follows:

            ```
            
            SELECT
            deviceName() as deviceName,
            attribute('tag') as tag,
            attribute('deviceISN') as isn,
            temperature,
            humidity,
            timestamp('yyyy-MM-dd HH:mm:ss') as time
            FROM
            "/Specify the productKey/+/user/data"
            ```

        2.  Configure the data forwarding action.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7645/15578128494799_en-US.png)

            The complete rule is as follows:

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7645/15578128494805_en-US.png)

    7.  On the rules page, click **Enable** to enable the rule.
4.  Temperature and humidity sensor. 

    To facilitate testing, a Node.js program is used to simulate the temperature and humidity sensor and send the temperature and humidity data. The [aliyun-iot-mqtt library](https://npmjs.org/package/aliyun-iot-mqtt) is used in this example. The complete code is as follows:

    ```
    
    const mqtt = require('aliyun-iot-mqtt');
    const client = mqtt.getAliyunIotMqttClient({
    productKey: "Specify the productKey",
    deviceName: "Specify the deviceName",
    deviceSecret: "Specify the deviceSecret"
    });
    const topic = 'Specify the topic with forwarding actions';
    const data = {
    temperature: 18,
    humidity: 63,
    };
    client.publish(topic, JSON.stringify(data));
    ```

5.  DingTalk robot receives messages. 
    1.  The program sends the temperature and humidity data. 

        ```
        
        $ npm install
        $ node demo.js
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7645/15578128494806_en-US.png)

    2.  DingTalk robot receives the data, and sends a message to the DintTalk group.

