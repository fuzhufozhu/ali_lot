# 自定义Topic {#concept_ppk_rz4_k2b .concept}

本文介绍如何为产品自定义Topic类。自定义Topic类将自动映射到该产品下的所有设备中。

## 操作步骤 {#section_nhd_3ly_w2b .section}

1.  登录[物联网平台控制台](https://iot.console.aliyun.com)。
2.  左侧导航栏单击**设备管理** \> **产品**。
3.  在产品管理页面，找到需要自定义Topic类的产品，并单击对应操作栏中的**查看**按钮。
4.  在产品详情页面，单击**Topic类列表** \> **定义Topic类**。
5.  定义Topic类。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15450/15469117387118_zh-CN.png)

    -   **设备操作权限**：设备对该Topic的操作权限，可设置为发布、订阅、发布和订阅。
    -   **Topic类**：根据页面上方的Topic规则设置Topic类的自定义类目名称。
    -   **描述**：对自定义的Topic类进行描述，可以为空。
6.  单击**确认**。

## Topic类中的通配符 {#section_ytf_qjy_w2b .section}

自定义Topic类时，您可以使用通配符。通配符内容请参考[什么是Topic](intl.zh-CN/用户指南/产品与设备/Topic/什么是Topic.md#)。其中：

-   `#`代表本级及下级所有类目。
-   `+`代表本级所有类目。

**说明：** 创建带通配符的Topic类时，需注意：

-   只有设备操作权限为订阅的产品，才支持使用通配符。
-   通配符`#`只能在Topic类的最后一个类目。
-   带通配符的Topic不支持在设备的**Topic列表**页面执行**发布消息**操作。

