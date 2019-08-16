# Java SDK使用说明 {#reference_ukn_xwb_zdb .reference}

物联网平台的 Java SDK 让开发人员可以方便地使用 Java 程序操作物联网平台。开发者可以使用Maven依赖添加SDK，也可以下载安装包到本地直接安装。

## 安装 SDK {#section_o4t_hgd_zdb .section}

1.  安装Java开发环境。

    您可以从 [Java 官方网站](http://developers.sun.com/downloads/) 下载，并按说明安装Java开发环境。

2.  安装IoT Java SDK。
    1.  访问 [Apache Maven 官网](http://maven.apache.org/)下载Maven软件。
    2.  添加Maven项目依赖。

        IoT Java SDK的Maven依赖坐标

        ``` {#codeblock_bhn_y6h_hcb}
        <!-- https://mvnrepository.com/artifact/com.aliyun/aliyun-java-sdk-iot -->
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-iot</artifactId>
            <version>6.11.0</version>
        </dependency>
        ```

        依赖公共包

        ``` {#codeblock_45w_ys4_opv}
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>3.5.1</version>
        </dependency>
        ```


## 初始化SDK {#section_gmk_k4d_zdb .section}

**说明：** 以下示例以华东2地域及其服务接入地址为例。您在设置时，需使用您的物联网平台地域和对应的服务接入地址。

``` {#codeblock_36w_2ug_tu7}
String accessKey = "<your accessKey>";
String accessSecret = "<your accessSecret>";
DefaultProfile.addEndpoint("cn-shanghai", "cn-shanghai", "Iot", "iot.cn-shanghai.aliyuncs.com");
IClientProfile profile = DefaultProfile.getProfile("cn-shanghai", accessKey, accessSecret);
DefaultAcsClient client = new DefaultAcsClient(profile); //初始化SDK客户端
```

accessKey即您的账号的AccessKeyId，accessSecret即AccessKeyId对应的AccessKeySecret。您可在[阿里云官网控制台AccessKey管理](https://ak-console.aliyun.com)中创建或查看您的AccessKey。

## 发起调用 {#section_ins_yrd_zdb .section}

物联网平台云端API，请参见[API列表](cn.zh-CN/云端开发指南/云端API参考/API列表.md#)。

以调用Pub接口发布消息到Topic为例。

``` {#codeblock_zp2_3dy_7kv}
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

## 附录：Demo {#section_tlh_nst_wkh .section}

点击下载[SDK Demo](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/98270/cn_zh/1552272193375/iotx-api-demo.zip)。Demo中包含Java、Python、PHP、.NET版本SDK示例。

另外，阿里云提供API在线调试工具 [OpenAPI Explorer](https://api.aliyun.com)。在OpenAPI Explorer页，您可以快速检索和试验调用API。系统会根据您输入的参数同步生成各语言SDK的Demo代码。各语言SDK Demo显示在页面右侧**示例代码**页签下供您参考。在**调试结果**页签下，查看API调用的真实请求URL和JSON格式的返回结果。

