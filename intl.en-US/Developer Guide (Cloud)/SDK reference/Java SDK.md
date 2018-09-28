# Java SDK {#reference_ukn_xwb_zdb .reference}

The IoT Java SDK allows you to create Java applications to interact with IoT platform easily.   

## Install the IoT Java SDK {#section_o4t_hgd_zdb .section}

1.  Install a Java development environment.

    You can download a Java IDE from [Java official website](http://developers.sun.com/downloads/), and install the IDE by following the instructions.

2.  Install the IoT Java SDK.
    1.  Visit [Apache Maven official website](http://maven.apache.org/) to download a Maven installation package.
    2.  Add a Maven dependency.

          Maven coordinates of the IoT Java SDK dependency

        ```
        <! -- https://mvnrepository.com/artifact/com.aliyun/aliyun-java-sdk-iot -->
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-iot</artifactId>
            <version>6.2.0</version>
        </dependency>
        ```

        Public libraries

        ```
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>3.5.1</version>
        </dependency>
        ```


## Initialize the SDK {#section_gmk_k4d_zdb .section}

**Note:** The following example uses the China \(Shanghai\) region and the corresponding endpoint. You must specify the region of your IoT Platform and the endpoint of your service to use the SDK.

```
String accessKey = "<your accessKey>";
String accessSecret = "<your accessSecret>";
DefaultProfile.addEndpoint("cn-shanghai", "cn-shanghai", "Iot", "iot.cn-shanghai.aliyuncs.com");
IClientProfile profile = DefaultProfile.getProfile("cn-shanghai", accessKey, accessSecret);
DefaultAcsClient client = new DefaultAcsClient(profile); //Initialize the SDK
```

accessKeyId is the AccessKeyId of your account, andaccessSecret is the AccessKeySecret paired with the AccessKeyId. You can go to [AccessKeys](https://ak-console.aliyun.com) page in the Alibaba Cloud console to create or view your AccessKey.

## Initiate a call {#section_ins_yrd_zdb .section}

The following example demonstrates how to call the Pub operation to publish messages to a topic.

```
PubRequest request = new PubRequest();
request.setProductKey("productKey");
request.setMessageContent(Base64.encodeBase64String("hello world".getBytes()));
request.setTopicFullName("/productKey/deviceName/get");
request.setQos(0); //Currently, QoS0 and QoS1 are supported.
PubResponse response = client.getAcsResponse(request);
System.out.println(response.getSuccess());
System.out.println(response.getErrorMessage());
```

