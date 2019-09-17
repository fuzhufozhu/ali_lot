# Python SDK使用说明 {#reference_qdh_ywb_zdb .reference}

物联网平台提供Python语言的云端SDK供开发人员使用。本文介绍云端Python SDK的安装和配置，及使用Python SDK调用云端API的示例。

## 安装Python SDK {#section_acm_vtd_zdb .section}

1.  安装Python开发环境。

    访问[Python官网](https://www.python.org/downloads/)下载Python安装包，并完成安装。目前，支持2.6.5及以上版本。

2.  安装Python的包管理工具pip。（如果您已安装pip，请忽略此步骤。）

    访问 [pip 官网](https://pip.pypa.io/en/stable/installing/)下载pip安装包，并完成安装。

3.  安装IoT Python SDK。

    以管理员权限执以下命令，安装IoT Python SDK。请参见最新版[aliyun-python-sdk-iot](https://github.com/aliyun/aliyun-openapi-python-sdk/tree/master/aliyun-python-sdk-iot)信息。

    ``` {#codeblock_uml_jkb_re7}
    sudo pip install aliyun-python-sdk-core
    sudo pip install aliyun-python-sdk-iot
    ```

4.  将IoT Python SDK相关文件引入Python文件。

    ``` {#codeblock_em6_sxj_8wm}
    from aliyunsdkcore import client
    from aliyunsdkiot.request.v20180120 import RegisterDeviceRequest
    from aliyunsdkiot.request.v20180120 import PubRequest
    ...
    ```


## 初始化SDK {#section_mcr_dzd_zdb .section}

``` {#codeblock_w4k_pgo_ith}
accessKeyId = '<your accessKey>'
 accessKeySecret = '<your accessSecret>'
 clt = client.AcsClient(accessKeyId, accessKeySecret, 'cn-shanghai')
```

accessKeyId即您的账号的AccessKeyId，accessKeySecret即AccessKeyId对应的AccessKeySecret。您可在[阿里云官网控制台AccessKey管理](https://ak-console.aliyun.com)中创建或查看您的AccessKey。

## 发起调用 {#section_xdd_5zd_zdb .section}

物联网平台云端API，请参见[API列表](intl.zh-CN/云端开发指南/云端API参考/API列表.md#)。

以调用Pub接口发布消息到设备为例。

``` {#codeblock_mbw_kxk_lvw}
request = PubRequest.PubRequest()
request.set_accept_format('json')  #设置返回数据格式，默认为XML，此例中设置为JSON
request.set_ProductKey('productKey')
request.set_TopicFullName('/productKey/deviceName/get')  #消息发送到的Topic全名
request.set_MessageContent('aGVsbG8gd29ybGQ=')  #hello world Base64 String
request.set_Qos(0)
result = clt.do_action_with_exception(request)
print 'result : ' + result
```

## 附录：Demo {#section_7kz_a5c_eaf .section}

单击下载[云端SDK Demo](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/44229/intl_en/1568085719729/iotx-api-demo.tar.gz)。Demo中包含Java、Python、PHP、.NET版本SDK示例。

另外，阿里云提供API在线调试工具 [OpenAPI Explorer](https://api.aliyun.com)。在OpenAPI Explorer页，您可以快速检索和试验调用API。系统会根据您输入的参数同步生成各语言SDK的Demo代码。各语言SDK Demo显示在页面右侧**示例代码**页签下。在**调试结果**页签下，查看API调用的真实请求URL和JSON格式的返回结果。

