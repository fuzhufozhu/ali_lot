# Initialize the SDKs {#task_1732693 .task}

This topic describes the Java SDK demos used on your servers and on the devices. You must prepare a Java development environment, download the SDK demo package, import a project, and then initialize the SDKs.

[Create a gateway and a sub-device](intl.en-US/Best Practices/Connect a sub-device to IoT Platform/Create a gateway and a sub-device.md#)

1.  Click [here](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/44229/intl_en/1565952427116/iotx-api-demo.zip) to download iotx-api-demo, and decompress the package. This package contains the server SDK demo and the device SDK demo.
2.  Open the Java development tool and import the decompressed folder iotx-api-demo.
3.  In the java directory, add the following dependencies to the pom file to import the server SDK and device SDK. 

    ``` {#codeblock_djy_haj_1ir}
    <! -- https://mvnrepository.com/artifact/com.aliyun/aliyun-java-sdk-iot -->
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-iot</artifactId>
        <version>6.5.0</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-core</artifactId>
        <version>3.5.1</version>
    </dependency>
    <dependency>
      <groupId>com.aliyun.alink.linksdk</groupId>
      <artifactId>iot-linkkit-java</artifactId>
      <version>1.2.0.1</version>
      <scope>compile</scope>
    </dependency>
    ```

4.  In the directory java/src/main/resources/, configure the initialization in the config file. 

    ``` {#codeblock_asc_2ob_57w}
    user.accessKeyID = <your accessKey ID>
    user.accessKeySecret = <your accessKey Secret>
    iot.regionId = <regionId>
    iot.productCode = Iot
    iot.domain = iot.<regionId>.aliyuncs.com
    iot.version = 2018-01-20
    ```

    |Parameter|Description|
    |:--------|:----------|
    |accessKeyID|The AccessKey ID of your Alibaba Cloud account. Place the pointer on your profile icon in the console, select **AccessKey** to go to the User Management page. You can create or check your AccessKey in User Management.

 |
    |accessKeySecret|The AccessKey Secret of your Alibaba Cloud account. You can check the AccessKey Secret in the same way as the AccessKey ID.|
    |regionId|The ID of the region where your IoT devices are located. For more information about region ID expressions, see [Regions and zones](../../../../intl.en-US/General Reference/Regions and zones.md#).|


[Connect the gateway to IoT Platform](intl.en-US/Best Practices/Connect a sub-device to IoT Platform/Connect the gateway to IoT Platform.md#)

[Connect the sub-device to IoT Platform](intl.en-US/Best Practices/Connect a sub-device to IoT Platform/Connect the sub-device to IoT Platform.md#)

