# 自定义Topic {#concept_ppk_rz4_k2b .concept}

本文介绍如何为产品自定义Topic类。自定义Topic类将自动映射到该产品下的所有设备中。

## 操作步骤 {#section_nhd_3ly_w2b .section}

1.  登录[物联网平台控制台](https://iot.console.aliyun.com)。
2.  在左侧导航栏中，单击**产品管理**。
3.  在产品列表页面，找到需要自定义Topic类的产品，并单击对应操作栏中的**查看**按钮。
4.  在产品详情页面，单击**消息通信** \> **定义Topic类**。
5.  定义Topic类。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15450/15393297347118_zh-CN.png)

    -   **Topic类**：根据页面上方的Topic规则设置Topic类。
    -   **设备操作权限**：设备对该Topic的操作权限，可设置为发布、订阅、发布和订阅。
    -   **描述**：对自定义的Topic类进行描述，可以为空。
6.  单击**确认**。

## 创建带通配符的Topic类 {#section_ytf_qjy_w2b .section}

物联网平台支持自定义带通配符的Topic。如果您需要在订阅Topic时设置为带通配符的Topic，则首先需要自定义带通配符的Topic类，再订阅。创建带通配符的Topic类的方法与创建一般Topic类一致。

创建带通配符的Topic类时，需注意：

-   需先将设备操作权限选择为订阅。只有设备操作权限为订阅时，才能支持填写通配符。
-   Topic类：您可以创建带有通配符`#`或者`+`的Topic类。

    **说明：** 通配符`#`只能出现在Topic类的最后面。


对于带通配符的Topic，您不能在设备的Topic列表页面执行**发布消息**的操作。

