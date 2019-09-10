# PHP SDK使用说明 {#task_1545804 .task}

物联网平台提供PHP语言的云端SDK供开发人员使用。本文介绍云端PHP SDK的安装和配置，及使用PHP SDK调用云端API的示例。

## 安装IoT PHP SDK {#section_0v7_co6_ivd .section}

IoT PHP SDK是[Alibaba Cloud SDK for PHP](https://github.com/aliyun/openapi-sdk-php)的一部分。如果您已安装Alibaba Cloud SDK for PHP ，则无需安装IoT PHP SDK。

1.  安装PHP开发环境。 需安装PHP 5.5.0或更高版本。访问[PHP官网](http://www.php.net/)下载PHP安装包，并完成安装。
2.  安装Composer。 目前，IoT PHP SDK通过Composer管理，因此需在系统中安装Composer。
    -   Windows系统用户，请访问[getcomposer.org](https://getcomposer.org/)，下载、安装[Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe)
    -   使用cURL命令安装：

        ``` {#codeblock_atc_xsa_2lp}
        curl -sS https://getcomposer.org/installer | php
        ```

3.  添加以下依赖，安装IoT PHP SDK。 

    ``` {#codeblock_y8d_dt1_n2w}
    composer require alibabacloud/iot
    ```

    PHP SDK详情和使用指导，请参见[openapi-sdk-php-iot](https://github.com/aliyun/openapi-sdk-php/tree/master/src/Iot)和[Alibaba Cloud SDK for PHP](https://github.com/aliyun/openapi-sdk-php)。


## 初始化SDK {#section_48e_nip_ws0 .section}

初始化SDK示例

``` {#codeblock_w3o_bfz_9eu}
<?php
include_once 'aliyun-php-sdk-core/Config.php';
use \Iot\Request\V20180120 as Iot;
//设置您的AccessKeyId/AccessSecret/ProductKey
$accessKeyId = "";
$accessSecret = "";
$iClientProfile = DefaultProfile::getProfile("cn-shanghai", $accessKeyId, $accessSecret);
$client = new DefaultAcsClient($iClientProfile);
```

accessKeyId即您的账号的AccessKeyId，accessSecret即AccessKeyId对应的AccessKeySecret。您可在[阿里云官网控制台AccessKey管理](https://ak-console.aliyun.com)中创建或查看您的AccessKey。

## 发起调用 {#section_8ks_pe5_210 .section}

物联网平台云端API，请参见[API列表](intl.zh-CN/云端开发指南/云端API参考/API列表.md#)。

以调用Pub接口发布数据到设备为例。

``` {#codeblock_kv2_9fg_2qg}
$request = new Iot\PubRequest();
$request->setProductKey("productKey");
$request->setMessageContent("aGVsbG93b3JsZA="); //hello world Base64 String.
$request->setTopicFullName("/productKey/deviceName/get"); //消息发送到的Topic全名.
$response = $client->getAcsResponse($request);
print_r($response);
```

## 附录：Demo {#section_94c_yoc_kl7 .section}

单击下载[云端SDK Demo](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/44229/intl_en/1568085719729/iotx-api-demo.tar.gz)。Demo中包含Java、Python、PHP、.NET版本SDK示例。

另外，阿里云提供API在线调试工具 [OpenAPI Explorer](https://api.aliyun.com)。在OpenAPI Explorer页，您可以快速检索和试验调用API。系统会根据您输入的参数同步生成各语言SDK的Demo代码。各语言SDK Demo显示在页面右侧**示例代码**页签下。在**调试结果**页签下，查看API调用的真实请求URL和JSON格式的返回结果。

