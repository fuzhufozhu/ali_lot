# Tags {#concept_m3c_jnl_vdb .concept}

The IoT Platform tag is the user-defined identifier you set for a product or device. You can use tags to flexibly manage your products and devices.

IoT often involves the management of a huge number of products and devices. How to distinguish various products and devices, and how to achieve centralized management become a challenge. Alibaba Cloud IoT Platform provides tags to address this issue. You can set different tags for your products and devices. The use of tags allows the centralized management of your various products and devices.

Tags include product tags and device tags. The product tag is the template for the device tag. After a product tag is created,  newly created devices under the product will be automatically assigned the product tag. You can also add device tags for each device individually. The structure of the tag is `key: value`.

This document describes how to create a product tag and device tag.

## Product tags {#section_u23_ssb_wdb .section}

Product tags typically describe the information that is common to all devices under a product. For example, a tag can indicate a specific manufacturer, organization, physical size, and operating system. You have to create a product before you can add a product tag.

To add a product tag, follow these steps:

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com/).
2.  On the Products page, find the product to which you want to add a tag, click **View** in the Actions column, and then go to the **Product Details** page.
3.  Click **Edit** in the **Product Tags** section.
4.  In the **Edit Tags** dialog box, enter the Key and  Value for the tag, and then click **OK**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12823/15349044202847_en-US.png)

After a product tag is created, newly created devices under the product will be automatically assigned the product tag.

## Device tags {#section_igy_bvb_wdb .section}

After a device is created under a product, the device will be automatically assigned the product tag. However, the tag derived from a product only defines the information that is common to all devices under the product. Depending on the device features, you can facilitate device management by adding unique tags to the device. For example, for a smart meter in Room 201, you can add a  `room:201`  tag. You can manage device tags either in the console or using the API.

To add a device tag in the console, follow these steps:

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com/).
2.  Click **Devices**.
3.  On the **Devices** page, find the device that you want to add a tag, click **View** under the Actions column, and then go to the **Device Details** page.
4.  Click **Edit** in the **Device Tags** section.
5.  In the **Edit Tags** dialog box, enter the Key and  Value for the tag, and then click **OK**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12823/15349044212849_en-US.png)

The device tag follows the device through the system. In addition, IoT Platform can send device tags to other services in Alibaba Cloud based on the Rule Engine.

