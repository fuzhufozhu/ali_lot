# Java SDK使用说明 {#reference_ukn_xwb_zdb .reference}

物联网平台的 Java SDK 让开发人员可以方便地使用 Java 程序操作物联网平台。开发者可以使用Maven依赖添加SDK，也可以下载安装包到本地直接安装。

## 安装 SDK {#section_o4t_hgd_zdb .section}

1.  安装Java开发环境。

    您可以从 [Java 官方网站](http://developers.sun.com/downloads/) 下载，并按说明安装Java开发环境。

2.  安装IoT Java SDK。
    1.  访问 [Apache Maven 官网](http://maven.apache.org/)下载Maven软件。
    2.  添加Maven项目依赖。

        IoT Java SDK的Maven依赖坐标

        ```
        <!-- https://mvnrepository.com/artifact/com.aliyun/aliyun-java-sdk-iot -->
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-iot</artifactId>
            <version>6.6.1</version>
        </dependency>
        ```

        依赖公共包

        ```
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>3.5.1</version>
        </dependency>
        ```


## 初始化SDK {#section_gmk_k4d_zdb .section}

**说明：** 以下示例以华东2地域及其服务接入地址为例。您在设置时，需使用您的物联网平台地域和对应的服务接入地址。

```
String accessKey = "<your accessKey>";
String accessSecret = "<your accessSecret>";
DefaultProfile.addEndpoint("cn-shanghai", "cn-shanghai", "Iot", "iot.cn-shanghai.aliyuncs.com");
IClientProfile profile = DefaultProfile.getProfile("cn-shanghai", accessKey, accessSecret);
DefaultAcsClient client = new DefaultAcsClient(profile); //初始化SDK客户端
```

accessKey即您的账号的AccessKeyId，accessSecret即AccessKeyId对应的AccessKeySecret。您可在[阿里云官网控制台AccessKey管理](https://ak-console.aliyun.com)中创建或查看您的AccessKey。

## 发起调用 {#section_ins_yrd_zdb .section}

以调用Pub接口发布消息到Topic为例。

```
PubRequest request = new PubRequest(); 
request.setProductKey("productKey"); 
request.setMessageContent(Base64.encodeBase64String("hello world".getBytes())); 
request.setTopicFullName("/productKey/deviceName/get"); 
request.setQos(0); //目前支持QoS0和QoS1 
try 
{ 
   PubResponse response = client.getAcsResponse(request); 
   System.out.println(response.getSuccess()); 
   System.out.println(response.getErrorMessage());
} 
catch (ServerException e) 
{
   e.printStackTrace();
}
 catch (ClientException e)
{
   e.printStackTrace();
}
```

