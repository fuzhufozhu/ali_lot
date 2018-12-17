# Tags {#concept_m3c_jnl_vdb .concept}

A tag is a custom identifier you set for a product, a device, or a device group. You can use tags to flexibly manage your products, devices and groups.

IoT often involves the management of a huge number of products and devices. How to distinguish various products and devices, and how to achieve centralized management become a challenge. Alibaba Cloud IoT Platform allows you to use tags to address these issues. The use of tags allows the centralized management of your various products and devices.

Therefore, we recommend that you create tags for your products, devices and device groups. The structure of a tag is `key: value`.

This topic describes how to create product tags, device tags, and group tags.

## Product tags {#section_u23_ssb_wdb .section}

Product tags typically describe the information that is common to all devices of a product. For example, a tag can indicate a specific manufacturer, organization, physical size, or operating system. After a product has been created, you can add tags for it.

To add product tags, follow these steps:

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, click **Devices** \> **Product**.
3.  On the Product Details page, find the product for which you want to add tags and click **View**.
4.  Click **Add** under **Tag Information**.
5.  In the dialog box, enter values for Tag Key and Tag Value, and then click **OK**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12823/15450398542847_en-US.png)

## Device tags {#section_igy_bvb_wdb .section}

You can facilitate device management by adding unique tags to devices. For example, you can use the device feature information as tags, such as `PowerMeter:room201` for the electricity meter of room 201. You can manage device tags either in the console or using the APIs.

To add device tags in the console, follow these steps:

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, click **Devices** \> **Device**.
3.  On the **Devices** page, find the device for which you want to add tags, click **View** to go to the **Device Details** page.
4.  Click **Add** under **Tag Information**.
5.  In the dialog box, enter values for Tag Key and Tag Value, and then click **OK**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12823/15450398542849_en-US.png)

Device tags always follow the devices, and can be forwarded to other Alibaba Cloud services by using the rules engine.

## Group tags {#section_szr_31j_wfb .section}

You can manage devices across products by grouping your devices. A group tag typically describe the general information of devices in the group and the sub-groups. For example, you can use region information as a group tag. After you have created a group, you can add tags for it.

To add group tags, follow these steps:

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, click **Devices** \> **Group**.
3.  On the Group Management page, find the group for which you want to add tags and click **View**.
4.  Click **Add** under **Tag Information**.
5.  In the dialog box, enter values for Tag Key and Tag Value, and then click **OK**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12823/154503985432634_en-US.png)

