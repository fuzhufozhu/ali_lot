# Create a topic category {#concept_ppk_rz4_k2b .concept}

This article introduces how to create a custom topic category for a product. Custom topic categories will be automatically assigned to devices under the product.

## Procedure {#section_nhd_3ly_w2b .section}

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left navigation pane, click **Products**.
3.  On the Product List page, find the product you want to create a topic category for, and click **View** in the operation column.
4.  On the Product Details page, click **Notifications** \> **Create Topic Category**.
5.  Define a topic category.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15450/15404604407118_en-US.png)

    -   **Topic Category**: Enter a topic category name according to the Topic Rule on the page.
    -   **Device Operation Authorization**: Indicates the operations that devices can perform on the topics of this topic category. You can select from Publish, Subscribe, and Publish and Subscribe.
    -   **Description**: Describes the topic category. You can leave this box empty.
6.  Click **OK**.

## Create a topic category with a wildcard character {#section_ytf_qjy_w2b .section}

IoT Platform supports custom topic categories with wildcard characters. When you configure a topic subscription and you want to set topics with wildcard characters, you must first create topic categories with wildcard characters. The procedures of creating a topic category with a wildcard character is almost the same as that of creating a general topic category.

When you are creating a topic category with a wildcard character, pay attention to the following:

-   You must firstly select Subscribe as the Device Operation Authorizations. Only when the device operation authorization is set as Subscribe can you enter wildcard characters in topic category name field.
-   Topic Category: You can use wildcard characters `#` and `+` in the topic category name.

    **Note:** `#` can only be located at the end of the topic category name.


For topics with wildcard characters, you cannot click **Publish** to publish messages on the Topic List page of devices.

