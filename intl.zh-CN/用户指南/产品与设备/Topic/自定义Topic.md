# 自定义Topic {#concept_ppk_rz4_k2b .concept}

本文介绍如何为产品自定义Topic类。自定义Topic类将自动映射到该产品下的所有设备中。

## 操作步骤 {#section_nhd_3ly_w2b .section}

1.  登录[物联网平台控制台](https://iot.console.aliyun.com)。
2.  左侧导航栏单击**设备管理** \> **产品**。
3.  在产品管理页面，找到需要自定义Topic类的产品，并单击对应操作栏中的**查看**按钮。
4.  在产品详情页面，单击**Topic类列表** \> **定义Topic类**。
5.  定义Topic类。

    ![自定义Topic](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15450/15687046117118_zh-CN.png)

    |参数|描述|
    |--|--|
    |设备操作权限|设备对该Topic的操作权限，可设置为**发布**、**订阅**、**发布和订阅**。|
    |Topic类|将Topic类填充完整。 **说明：** 只有设备操作权限为**订阅**时，才可以使用通配符+和\#自定义Topic类。

+代表本级所有类目。

\#代表本级及下级所有类目。它只能出现在Topic类的最后一个类目。

 |
    |描述|写一些话描述该Topic类，可以为空。|

6.  单击**确认**。

## 带通配符的自定义Topic {#section_ayy_1us_99o .section}

带通配符的Topic不支持在设备的**Topic列表**页面执行**发布消息**操作，仅支持订阅操作。

例如，某产品有一个自定义Topic类：`/a1aycMA****/${deviceName}/user/#`。DeviceName为Light的设备订阅`/a1aycMA****/Light/user/#`表示批量订阅了以/a1aycMA\*\*\*\*/Light/user/为开头的全部Topic，包含`/a1aycMA****/Light/user/get`，`/a1aycMA****/Light/user/data`等。

例如，某产品有一个自定义Topic类：`/a1aycMA****/${deviceName}/user/+/error`。DeviceName为Robot的设备订阅`/a1aycMA****/Robot/user/+/error`，表示批量订阅了 `/a1aycMA****/Robot/user/get/error`、`/a1aycMA****/Robot/user/update/error`等Topic。

## 自定义Topic通信 {#section_qwx_k6f_sli .section}

服务端调用[Pub](../../../../intl.zh-CN/云端开发指南/云端API参考/消息通信/Pub.md#)，可向指定的自定义Topic发布消息；设备通过订阅该Topic，接收来自服务端的消息。

使用自定义Topic通信的示例，请参见[使用自定义Topic进行通信](../../../../intl.zh-CN/最佳实践/使用自定义Topic进行通信.md#)。

