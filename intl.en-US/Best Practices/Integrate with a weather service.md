# Integrate with a weather service {#task_bzt_jwv_ydb .task}

The user specifies a city name, city code, or the latitude and longitude of a city. IoT Platform uses the rules engine to pass the request to Function Compute for further processing. Function Compute then queries the weather service and retrieves the weather data for the specified city. The query results are eventually sent to the specified topic.

-   In this example, one topic performs weather queries, and the other receives query results.
-   This example uses Function Compute to access third-party services in Alibaba Cloud API Marketplace.

1.  Create a product, add a device, and define a topic. 
    1.  Log on to the IoT Platform console.
    2.  Select Products, and click **Create Product** to create an IoT Platform product. In this example, the product is named weatherProduct.
    3.  Select Devices, select weatherProduct, and then click **Add Device** to specify the device name.
    4.  Select Products, and click **View** next to weatherProduct.
    5.  Select Notifications, and click **Define Topic Category** to define the topics for performing weather queries and receiving query results, as shown in [Figure 1](#fig_yjq_shc_zdb). 

        ![](images/4509_en-US.png "Define topics")

    6.  Select Rules Engine, and click **Create Rule** to create a new JSON rule.
    7.  Click **Management** next to Create Rule. On the Data Processing page, click **Write SQL**, as shown in [Figure 2](#fig_ywk_v3c_zdb). 

        ![](images/4513_en-US.png "Rules engine")

2.  Define the function. 

    Business logic:

    -   Performs weather queries based on city information.
    -   An API is used to query weather information.
    -   The query results are sent to the specified topic of the device using the service API provided by IoT Platform.
    -   Create a new project and import the following JAR files:

``` {#codeblock_201_mcw_t4r}

com.aliyun.fc.runtime
fc-java-core
1.0.0


com.aliyun
aliyun-java-sdk-core
3.2.10


com.aliyun
aliyun-java-sdk-iot
4.0.0


com.alibaba
fastjson
1.2.25


org.apache.httpcomponents
httpclient
4.2.1


org.apache.httpcomponents
httpcore
4.2.1


commons-lang
commons-lang
2.6
									
```

    -   Code logic:

        ``` {#codeblock_0s1_znm_41x}
        
        public class FcToApiDemo implements PojoRequestHandler {
        private static String host = "http://jisutqybmf.market.alicloudapi.com";
        private static String path = "/weather/query";
        private static String method = "GET";
        private static String appcode = "Your appCode";
        private static String accessKey = "Your Alibaba Cloud Access Key ID";
        private static String accessSecret = "Your Alibaba Cloud Access Key Secret";
        static DefaultAcsClient client;
        static {
        IClientProfile profile = DefaultProfile.getProfile("cn-shanghai", accessKey, accessSecret);
        try {
        DefaultProfile.addEndpoint("cn-shanghai", "cn-shanghai", "Iot", "iot.cn-shanghai.aliyuncs.com");
        } catch (ClientException e) {
        e.printStackTrace();
        }
        client = new DefaultAcsClient(profile); // Initialize a client object.
        }
        public String handleRequest(CityModel input, Context context) {
        Map headers = new HashMap();
        // The format of the header is as follows (include a space in the middle): Authorization:APPCODE
        // 83359fd73fe94948385f570e3c139105
        headers.put("Authorization", "APPCODE " + appcode);
        Map querys = new HashMap();
        querys.put("city", input.getCity());
        querys.put("citycode", input.getCityCode());
        querys.put("cityid", input.getCityid());
        querys.put("ip", input.getIp());
        querys.put("location", input.getLocation());
        try {
        /**
        * Note: Download HttpUtils from the following address:
        * https://github.com/aliyun/api-gateway-demo-sign-java/blob/master/src/main/java/com/aliyun/api/gateway/demo/util/HttpUtils.java
        *
        *
        * For more information about the dependencies, see:
        * https://github.com/aliyun/api-gateway-demo-sign-java/blob/master/pom.xml
        */
        HttpResponse response = HttpUtils.doGet(host, path, method, headers, querys);
        // Send the query results to the specified topic.
        Boolean result = pub(EntityUtils.toString(response.getEntity()));
        return result.toString();
        } catch (Exception e) {
        e.printStackTrace();
        }
        return "error";
        }
        public static Boolean pub(String msg) {
        try {
        PubRequest request = new PubRequest();
        request.setProductKey("The productKey of your product");
        request.setTopicFullName("The topic for receiving weather query results");
        request.setMessageContent(Base64.encodeBase64String(msg.getBytes()));
        request.setQos(1);
        PubResponse response = client.getAcsResponse(request);
        return response.getSuccess();
        } catch (ServerException e) {
        e.printStackTrace();
        } catch (ClientException e) {
        e.printStackTrace();
        }
        return false;
        }
        }
        ```

        The custom model is as shown in the following picture.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7644/15687859554521_en-US.png)

    The function implements the PojoRequestHandler interface that is provided by Function Compute. Function Compute also provides the StreamRequestHandler interface. Select an interface based on your needs.

    For more information, see [Java basics](https://www.alibabacloud.com/help/doc-detail/58887.htm).

3.  Configure the function 
    1.  Log on to the Function Compute console.
    2.  Click **Create Function**, select **No Trigger**, and configure the basic information, as shown in the following picture. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7644/15687859554602_en-US.png)

    3.  Click **Next**, **Create**.
    4.  After the configuration is complete, test the function to see whether the function runs properly. 

        The function runs properly if the code logic is correct.

4.  Log on to the IoT Platform console, and select Rules Engine to configure and enable an action rule, as shown in the following picture. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7644/15687859554615_en-US.png)

5.  Test the functionality of weather query. 
    -   Log on to your emulated device and subscribe to the specified topic.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7644/15687859554619_en-US.png)

    -   Send a message from the console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7644/15687859564622_en-US.png)

        You can also use cityid, cityCode, location, or IP information to query weather information.

    -   The device receives the message.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7644/15687859564628_en-US.png)


