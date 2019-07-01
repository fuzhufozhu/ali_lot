# 下载设备端SDK {#concept_jlk_mjl_vdb .concept}

物联网平台提供各类设备端SDK，简化开发过程，使设备快速上云。

## 前提条件 {#section_j13_sxr_52b .section}

在设备端开发之前，您需要首先完成控制台所需操作，以获取设备开发阶段的必要信息，包括设备信息、Topic信息等。具体内容请参考用户指南。

## 基于设备端SDK开发 {#section_e4x_yxr_52b .section}

您可以在设备中集成物联网平台提供的SDK，实现物联网平台接入。

SDK下载连接：

-   [C SDK](https://www.alibabacloud.com/help/doc-detail/96623.htm)

如果以上SDK不满足您的需求，可以按照以下模板发送邮件至`linkkitSDK-query@list.alibaba-inc.com`联系我们。

-   邮件主题：`SDK开发语言/平台咨询`
-   邮件内容：

    ``` {#codeblock_mue_9kq_goo}
    公司名称：
    联系人：
    联系电话：
    设备开发语言/平台：
    需求描述：
    贵司的产品规模及开发计划：
    ```


## 基于Alink协议开发 {#section_uyv_bm1_mgb .section}

如果提供的设备端SDK无法满足您的需求，可以参考[Alink协议文档](intl.zh-CN/设备端开发指南/基于Alink协议开发/Alink协议.md#)自行开发。

## 泛化协议SDK {#section_czd_vn1_mgb .section}

您可以使用[泛化协议SDK](../../../../intl.zh-CN/用户指南/泛化协议/概览.md#)，快速构建桥接服务，搭建设备或平台与阿里云物联网平台的双向数据通道。

## HTTP/2 SDK {#section_fd3_rvj_wgb .section}

您可以使用HTTP/2 SDK，建立设备端与阿里云物联网平台的数据通道。例如，将HTTP/2 SDK嵌入您的服务器，直接接受设备上报的消息。

-   [HTTP/2 SDK \(Java\)](intl.zh-CN/设备端开发指南/C-SDK/使用HTTP__2 SDK(Java)建连.md#)
-   [HTTP/2 SDK \(.NET\)](intl.zh-CN/设备端开发指南/C-SDK/使用HTTP__2 SDK(.NET)接入.md#)

