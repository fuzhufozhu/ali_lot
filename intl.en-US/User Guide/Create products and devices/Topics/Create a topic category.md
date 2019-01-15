# Create a topic category {#concept_ppk_rz4_k2b .concept}

This article introduces how to create a topic category for a product. Topic categories will be automatically assigned to devices of the product.

## Procedure {#section_nhd_3ly_w2b .section}

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com).
2.  In the left-side navigation pane, click **Devices** \> **Product**
3.  On the Products page, find the product for which you want to create a topic category, and click **View** in the operation column.
4.  On the Product Details page, click **Topic Categories** \> **Create Topic Category**.
5.  Define a topic category.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15450/15475331617118_en-US.png)

    -   **Device Operation Authorizations**: Indicates the operations that devices can perform on the topics of this topic category. You can select from Publish, Subscribe, and Publish and Subscribe.
    -   **Topic Category**: Enter a custom topic category name according to the Topic Rule on the page.
    -   **Description**: Describes the topic category. You can leave this box empty.
6.  Click **OK**.

## Wildcard characters in topic categories {#section_ytf_qjy_w2b .section}

When you create topic categories, you can use wildcards. For more information about wildcards, see [What is a topic?](intl.en-US/User Guide/Create products and devices/Topics/What is a topic?.md#) Supported wildcards:

-   `#`: Includes the category level you enter and all lower levels in topics.
-   `+`: Includes only one category level in topics, and not lower levels.

**Note:** When you want to create topic categories with wildcards, note that:

-   Only topics with Device Operation Authorizations as Subscription support wildcards.
-   `#` can only be at the end of topics.
-   For topics with wildcard characters, you cannot click **Publish** to publish messages on the **Topic List** tab page of devices.

