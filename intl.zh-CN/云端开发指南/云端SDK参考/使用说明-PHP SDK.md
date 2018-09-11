# 使用说明-PHP SDK {#reference_y3b_zwb_zdb .reference}

使用物联网平台的PHP SDK便捷地对物联网平台进行操作。

## 安装IoT PHP SDK {#section_vxf_kb2_zdb .section}

1.  安装PHP开发环境。

    访问[PHP官网](http://www.php.net/)下载PHP安装包，并完成安装。

2.  下载并解压IoT PHP SDK软件包。

    访问[IoT PHP SDK下载地址](https://github.com/aliyun/aliyun-openapi-php-sdk/tree/master/aliyun-php-sdk-iot)下载PHP SDK 包，然后解压下载的软件包到指定的目录。PHP SDK 是一个软件开发包，不需要进行安装操作。


## 初始化SDK {#section_mp5_jd2_zdb .section}

```
<?php
include_once 'aliyun-php-sdk-core/Config.php';
use \Iot\Request\V20180120 as Iot;
//设置你的AccessKeyId/AccessSecret/ProductKey
$accessKeyId = "";
$accessSecret = "";
$iClientProfile = DefaultProfile::getProfile("cn-shanghai", $accessKeyId, $accessSecret);
$client = new DefaultAcsClient($iClientProfile);
```

accessKeyId即您的账号的AccessKeyId，accessSecret即AccessKeyId对应的AccessKeySecret。您可在阿里云官网控制台[AccessKey管理](https://ak-console.aliyun.com)中创建或查看您的AccessKey。

## 发起调用 {#section_fcq_qd2_zdb .section}

以调用Pub接口发布数据到设备为例。

```
$request = new Iot\PubRequest();
$request->setProductKey("productKey");
$request->setMessageContent("aGVsbG93b3JsZA="); //hello world Base64 String.
$request->setTopicFullName("/productKey/deviceName/get"); //消息发送到的Topic全名.
$response = $client->getAcsResponse($request);
print_r($response);
```

