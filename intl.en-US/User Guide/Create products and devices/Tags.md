# Tags {#concept_m3c_jnl_vdb .concept}

A tag is a custom identifier you set for a product, a device, or a device group. You can use tags to flexibly manage your products, devices and groups.

IoT often involves the management of a huge number of products and devices. How to distinguish various products and devices, and how to achieve centralized management become a challenge. Alibaba Cloud IoT Platform allows you to use tags to address these issues. The use of tags allows the centralized management of your various products, devices, and groups.

Therefore, we recommend that you create tags for your products, devices and device groups. The structure of a tag is `key: value`.

This article describes how to create product tags, device tags, and group tags in the console.

**Note:** Each product, device, or group can have up to 100 tags.

## Product tags {#section_u23_ssb_wdb .section}

Product tags typically describe the information that is common to all devices of a product. For example, a tag can indicate a specific manufacturer, organization, physical size, or operating system. After a product has been created, you can create tags for it.

To create product tags in the console, follow these steps:

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, click **Devices** \> **Product**.
3.  On the Products page, find the product for which you want to create tags and click **View**.
4.  Click **Add** under **Tag Information**.
5.  In the dialog box, enter values for Tag Key and Tag Value, and then click **OK**.

    |Parameter|Description|
    |:--------|:----------|
    |Tag Key|A tag key can contain English letters, digits and dots \(.\), and cannot exceed 30 characters.|
    |Tag Value|A tag value can contain Chinese characters, English letters, digits, underscores \(\_\), hyphens \(-\) and dots \(.\), and cannot exceed 128 characters. A Chinese character is counted as two characters.|


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12823/15550353742847_en-US.png)

## Device tags {#section_igy_bvb_wdb .section}

You can facilitate device management by creating unique tags for devices. For example, you can use the device feature information as tags, such as `PowerMeter:room201` for the electricity meter of room 201.

Device tags always follow the devices. You can include tag information in the messages reported to IoT Platform by devices. When you use the rules engine to forward these messages to other Alibaba Cloud services, the tag information is also forwarded to the targets.

To create device tags in the console, follow these steps:

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, click **Devices** \> **Device**.
3.  On the **Devices** page, find the device for which you want to create tags, click **View** to go to the **Device Details** page.
4.  Click **Add** under **Tag Information**.
5.  In the dialog box, enter values for Tag Key and Tag Value, and then click **OK**.

    |Parameter|Description|
    |:--------|:----------|
    |Tag Key|A tag key can contain English letters, digits, and dots \(.\), and can be 2-30 characters in length.|
    |Tag Value|A tag value can contain Chinese characters, English letters, digits, underscores \(\_\), hyphens \(-\) and dots \(.\), and cannot exceed 128 characters. A Chinese character is counted as 2 characters.|


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12823/15550353742849_en-US.png)

## Group tags {#section_szr_31j_wfb .section}

You can manage devices across products by grouping your devices. A group tag typically describe the general information of devices in the group and the sub-groups. For example, you can use region information as a group tag. After you have created a group, you can create tags for it.

To create group tags, follow these steps:

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, click **Devices** \> **Group**.
3.  On the Group Management page, find the group for which you want to create tags and click **View**.
4.  Click **Add** under **Tag Information**.
5.  In the dialog box, enter values for Tag Key and Tag Value, and then click **OK**.

    |Parameter|Description|
    |:--------|:----------|
    |Tag Key|A tag key can contain English letters, digits, and dots \(.\), and can be 2-30 characters in length.|
    |Tag Value|A tag value can contain Chinese characters, English letters, digits, underscores \(\_\) and hyphens \(-\), and cannot exceed 128 characters. A Chinese character is counted as 2 characters.|


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12823/155503537432634_en-US.png)

## Manage tags in batch {#section_bnf_gmb_ygb .section}

In the console, you only can create, modify, and delete tags one by one. IoT Platform provides APIs for managing tags in batch. In addition, IoT Platform provides APIs for querying products, devices, and groups based on tags. For more information about tag related APIs, see the documents in API reference.

