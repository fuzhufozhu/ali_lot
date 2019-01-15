# 设备OTA升级 {#concept_sgj_yts_zdb .concept}

OTA（Over-the-Air Technology）即空中下载技术。物联网平台支持通过OTA方式进行设备固件升级。本文以MQTT协议下的固件升级为例，介绍OTA固件升级流程、数据流转使用的Topic和数据格式。

## OTA固件升级流程 {#section_qqd_r5s_zdb .section}

MQTT协议下固件升级流程如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14288/154753555311336_zh-CN.png)

固件升级Topic：

-   设备端上报固件版本给物联网平台

    ```
    /ota/device/inform/${YourProductKey}/${YourDeviceName}
    ```

-   设备端订阅该topic接收物联网平台的固件升级通知

    ```
    /ota/device/upgrade/${YourProductKey}/${YourDeviceName}
    ```

-   设备端上报固件升级进度

    ```
    /ota/device/progress/${YourProductKey}/${YourDeviceName}
    ```


**说明：** 

-   设备固件版本号只需要在系统启动过程中上报一次即可，不需要周期循环上报。
-   从物联网平台控制台发起批量升级，设备升级操作记录状态是待升级。

    实际升级以物联网平台OTA系统接收到设备上报的升级进度开始。设备升级操作记录状态是升级中。

-   根据版本号来判断设备端OTA升级是否成功。
-   设备离线时，不能接收服务端推送的升级消息。

    通过MQTT协议接入物联网平台的设备再次上线后，主动通知服务端上线消息。OTA服务端收到设备上线消息，验证该设备是否需要升级。如果需要升级，再次推送升级消息给设备， 否则，不推送消息。


## 数据格式说明 {#section_nm2_m4c_r2b .section}

设备端OTA开发流程和代码示例，请参见[Link Kit SDK](https://help.aliyun.com/product/93051.html)中，各语言SDK文档中，关于设备OTA开发章节。

1.  设备连接OTA服务，必须上报版本号。

    设备端通过MQTT协议推送当前设备固件版本号到Topic：`/ota/device/inform/${YourProductKey}/${YourDeviceName}`。消息内容格式如下：

    ```
    
    {
      "id": 1,
      "params": {
        "version": "xxxxxxxx"
      }
    }
    ```

    -   id：消息ID号，保留值。
    -   version：设备当前固件版本号。
2.  在物联网平台控制台上，逐步添加固件、验证固件和发起批量升级固件。

    具体操作，请参见[固件升级](../../../../../intl.zh-CN/用户指南/监控运维/固件升级.md#)。

3.  您在控制台触发升级操作之后，设备会收到物联网平台OTA服务推送的固件的URL地址。

    设备端订阅Topic： `/ota/device/upgrade/${YourProductKey}/${YourDeviceName}`。控制台对设备发起固件升级请求后，设备端会通过该Topic收到固件的URL。消息格式如下：

    ```
    
    {
      "code": "1000",
      "data": {
        "size": 432945,
        "version": "2.0.0",
        "url": "https://iotx-ota-pre.oss-cn-shanghai.aliyuncs.com/nopoll_0.4.4.tar.gz?    Expires=1502955804&OSSAccessKeyId=XXXXXXXXXXXXXXXXXXXX&Signature=XfgJu7P6DWWejstKJgXJEH0qAKU%3D&security-  token=CAISuQJ1q6Ft5B2yfSjIpK6MGsyN1Jx5jo6mVnfBglIPTvlvt5D50Tz2IHtIf3NpAusdsv03nWxT7v4flqFyTINVAEvYZJOPKGrGR0DzDbDasumZsJbo4f%2FMQBqEaXPS2MvVfJ%2BzLrf0ceusbFbpjzJ6xaCAGxypQ12iN%2B%2Fr6%2F5gdc9FcQSkL0B8ZrFsKxBltdUROFbIKP%2BpKWSKuGfLC1dysQcO1wEP4K%2BkkMqH8Uic3h%2Boy%2BgJt8H2PpHhd9NhXuV2WMzn2%2FdtJOiTknxR7ARasaBqhelc4zqA%2FPPlWgAKvkXba7aIoo01fV4jN5JXQfAU8KLO8tRjofHWmojNzBJAAPpYSSy3Rvr7m5efQrrybY1lLO6iZy%2BVio2VSZDxshI5Z3McKARWct06MWV9ABA2TTXXOi40BOxuq%2B3JGoABXC54TOlo7%2F1wTLTsCUqzzeIiXVOK8CfNOkfTucMGHkeYeCdFkm%2FkADhXAnrnGf5a4FbmKMQph2cKsr8y8UfWLC6IzvJsClXTnbJBMeuWIqo5zIynS1pm7gf%2F9N3hVc6%2BEeIk0xfl2tycsUpbL2FoaGk6BAF8hWSWYUXsv59d5Uk%3D",
        "md5": "93230c3bde425a9d7984a594ac55ea1e"
      },
      "id": 1507707025,
      "message": "success"
    }
    ```

    -   size：固件文件大小。
    -   md5：固件内容的MD5签名值，32位的hex String。
    -   url：固件的URL。时效为24小时。超过这个时间后，服务端拒绝下载。
    -   version：固件的目的版本号。
4.  设备收到URL之后，通过HTTPS协议根据URL下载固件。

    **说明：** 设备需在固件URL下发后的24小时内下载固件，否则该URL失效。

    下载固件过程中，设备端向服务端推送升级进度到Topic： `/ota/device/progress/${YourProductKey}/${YourDeviceName}`。消息格式如下：

    ```
    
    {
      "id": 1
      "params": {
        "step":"1", 
        "desc":" xxxxxxxx "
      }   
    }
    ```

    -   id：消息ID号，保留值。
    -   step：
        -   \[1, 100\] 下载进度比。
        -   -1：代表升级失败。
        -   -2：代表下载失败。
        -   -3：代表校验失败。
        -   -4：代表烧写失败。
    -   desc: 当前步骤的描述信息。如果发生异常可以用此字段承载错误信息。
5.  设备端完成固件升级后，推送最新的固件版本到Topic：`/ota/device/inform/${YourProductKey}/${YourDeviceName}`。如果上报的版本与OTA服务要求的版本一致就认为升级成功，反之失败。

    **说明：** 升级成功的唯一判断标志是设备上报正确的版本号。即使升级进度上报为100%，如果不上报新固件版本号，也视为升级失败。


## 常见下载固件错误 {#section_egm_k4t_xfb .section}

-   签名错误。如果设备端获取的固件的URL不全或者手动修改了URL内容，就会出现如下错误：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13905/15475355533964_zh-CN.png)

-   拒绝访问。URL过期导致。目前，URL有效期为24小时。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13905/15475355543967_zh-CN.PNG)


