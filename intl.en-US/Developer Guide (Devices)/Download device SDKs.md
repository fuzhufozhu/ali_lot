# Download device SDKs {#concept_jlk_mjl_vdb .concept}

IoT Platform provides multiple device SDKs to help you develop your devices and connect them to IoT Platform. If you want to develop your own SDK, see [Communications over Alink protocol](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Communications over Alink protocol.md#) for Alink data information.

## Prerequisites {#section_j13_sxr_52b .section}

Before you develop a device SDK, you must complete all console configurations and obtain all necessary information \(such as the device certificate information and topic information\). For details, see the User Guide.

## Use SDKs provided by IoT Platform {#section_e4x_yxr_52b .section}

You can use an SDK provided by IoT Platform and configure the SDK according to your business requirements and the protocol you want to use. For more information, see [Documents of Link Kit SDKs](https://help.aliyun.com/document_detail/100576.html). Link Kit SDKs include:

-   [C SDK](https://help.aliyun.com/document_detail/96623.html)
-   [Android SDK](https://help.aliyun.com/document_detail/96607.html)
-   [Node.js SDK](https://help.aliyun.com/document_detail/96618.html)
-   [Java SDK](https://help.aliyun.com/document_detail/97331.html)
-   [Python SDK](https://help.aliyun.com/document_detail/98292.html)
-   [iOS SDK](https://help.aliyun.com/document_detail/100534.html)

For a complete list of platforms supported by Alibaba Cloud IoT Platform, see [here](https://certification.aliyun.com/open/#/certificationlist).

If the platform you want to use is not supported by IoT Platform, you can create a new issue on the [Github](https://github.com/aliyun/iotkit-embedded/issues) page.

## Develop an SDK based on the Alink protocol {#section_uyv_bm1_mgb .section}

If you have specific development requirements that cannot be met by the provided SDKs, you can develop your own SDK. For Alink information, see [Alink protocol](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Communications over Alink protocol.md#).

## General protocol SDK {#section_czd_vn1_mgb .section}

You can use the general protocol SDK to develop the communications channel between your devices and Alibaba Cloud IoT Platform, to allow two-way data communication. For more information, see the following topics:

-   [Develop Core SDK](../../../../../reseller.en-US/User Guide/General protocols/Develop Core SDK.md#)
-   [Develop Server SDK](../../../../../reseller.en-US/User Guide/General protocols/Server SDK/Server SDK.md#)

## HTTP/2 SDKs {#section_fd3_rvj_wgb .section}

HTTP/2 SDKs are used to build data communication channels between devices and IoT Platform. For more information, see the following topics:

-   [HTTP/2 SDK \(Java\)](reseller.en-US/Developer Guide (Devices)/C-SDK/Configure the HTTP__2 Java SDK of devices.md#)
-   [HTTP/2 SDK \(. NET\)](reseller.en-US/Developer Guide (Devices)/C-SDK/Configure the HTTP__2 .NET SDK for devices.md#)

