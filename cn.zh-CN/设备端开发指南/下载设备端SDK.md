# 下载设备端SDK {#concept_jlk_mjl_vdb .concept}

物联网平台提供各类设备端SDK，简化开发过程，使设备快速上云。若您不使用SDK，请参考[Alink协议](intl.zh-CN/设备端开发指南/基于Alink协议开发/Alink协议.md#)文档自行开发。

## 前提条件 {#section_j13_sxr_52b .section}

在设备端开发之前，您需要首先完成控制台所需操作，以获取设备开发阶段的必要信息，包括设备信息、Topic信息等。具体内容请参考用户指南。

## 基于设备端SDK开发 {#section_e4x_yxr_52b .section}

您可以根据实际环境的协议、功能需求，参考[Link Kit SDK汇总](https://help.aliyun.com/document_detail/100576.html)选择合适的设备端SDK进行开发。Link Kit SDK包含：

-   [C SDK](https://help.aliyun.com/document_detail/96623.html)
-   [Android SDK](https://help.aliyun.com/document_detail/96607.html)
-   [NodeJS SDK](https://help.aliyun.com/document_detail/96618.html)
-   [Java SDK](https://help.aliyun.com/document_detail/97331.html)
-   [Python SDK](https://help.aliyun.com/document_detail/98292.html)
-   [iOS SDK](https://help.aliyun.com/document_detail/100534.html)

您可以单击[此处](https://certification.aliyun.com/open/#/certificationlist)，查询阿里云物联网平台适配的平台及详细信息。

如果您使用的平台未被适配，请访问[官方Github](https://github.com/aliyun/iotkit-embedded/issues)，给我们提出建议。

## 基于Alink协议开发 {#section_uyv_bm1_mgb .section}

如果提供的设备端SDK无法满足您的需求，可以参考[Alink协议文档](intl.zh-CN/设备端开发指南/基于Alink协议开发/Alink协议.md#)自行开发。

## 泛化协议SDK {#section_czd_vn1_mgb .section}

您可以使用泛化协议SDK，快速构建桥接服务，搭建设备或平台与阿里云物联网平台的双向数据通道。

-   [核心SDK](../../../../../intl.zh-CN/用户指南/泛化协议/核心SDK开发.md#)
-   [Server SDK](../../../../../intl.zh-CN/用户指南/泛化协议/Server SDK开发/Server SDK开发.md#)

## HTTP/2 SDK {#section_fd3_rvj_wgb .section}

您可以使用HTTP/2 SDK，建立设备端与阿里云物联网平台的数据通道。

-   [HTTP/2 SDK \(Java\)](intl.zh-CN/设备端开发指南/C-SDK/使用HTTP__2 SDK(Java)建连.md#)

