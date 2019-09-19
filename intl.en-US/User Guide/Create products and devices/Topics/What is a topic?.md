# What is a topic? {#concept_mny_tnl_vdb .concept}

A server and a device communicate with each other in IoT Platform through topics. Topics are associated with devices, and topic categories are associated with products. A topic category of a product is automatically applied to all devices under the product to generate device-specific topics for message communication.

## Topic category {#section_exz_xv5_vdb .section}

To simplify authorization and facilitate communication between devices and IoT Platform, topic categories were introduced. A topic category is a set of topics within the same product. For example, topic category `/${YourProductKey}/$ {YourDeviceName}/user/update` is a set that contains the following two topics: `/${YourProductKey}/device1/user/update` and `/${YourProductKey}/device2/user/update`.

After a device is created, all topic categories of the product are automatically applied to the device. You do not need to assign topics to each individual device.

![](images/35287_en-US.png "Automatically create a topic")

Descriptions for topic categories:

-   A topic category uses a forward slash \(/\) to separate elements in different hierarchical levels. A topic category contains the following fixed elements: `${YourProductKey}` indicates the product identifier; `${YourDeviceName}` indicates the device name.
-   Each element name can contain only letters, numbers, and underscores \(\_\). An element in each level cannot be left empty.
-   A device can have Pub and Sub permissions to a topic. **Pub** indicates that the device can publish messages to the topic. **Sub** indicates that the device can subscribe to the topic.
-   After you modify a topic category on the console, you must modify the related topic on devices.

## Topic {#section_ozb_bw5_vdb .section}

A topic category is used for topic definition rather than communication. Only topics can be used for communication.

-   Topics use the same format as topic categories. The difference is that variable `${YourDeviceName}` in the topic category is replaced by a specific device name in the topic.
-   A topic is automatically derived from the topic category of the product based on the corresponding device name. A topic contains the device name \(`DeviceName`\) and can be used for data communication only by the specified device. For example, topic `/${YourProductKey}/device1/user/update` belongs to the device named device1. Only device1 can publish messages and subscribe to this topic. Other devices cannot use this topic.

## Supported wildcards {#section_uns_lkw_hhb .section}

To use the rules engine data forwarding function to forward device data, you must specify the source topic of the messages when writing an SQL statement. When you specify a topic in [setting a forwarding rule](reseller.en-US/User Guide/Rules/Data Forwarding/Create and configure a rule.md#), you can use the following wildcards. One element can contain only one wildcard.

|Wildcard|Description|
|:-------|:----------|
|\#|Must be set as the last element in the topic. This wildcard can match any element in the current level and sub-levels. For example, in topic `/${YourProductKey}/device1/user /#`, wildcard `#` is added next to the `/user` element to represent all elements after `/user`. This topic can represent `/${YourProductKey}/device1/user/update` and `/${YourProductKey}/device1/user/update/error`.|
|+|Matches all elements in the current level. For example, in topic `/${YourProductKey}/+/user/update`, the device name element is replaced by wildcard `+` to represent all devices under the product. This topic can represent `/${YourProductKey}/device1/user/update` and `/${YourProductKey}/device2/user/update`.|

## System topics and custom topics {#section_oth_5lw_hhb .section}

IoT Platform supports the following types of topics:

|Type|Description|
|:---|:----------|
|System topics| The system-defined topics. System topics cannot be modified and deleted. System topics include topics used by IoT Platform functions, such as TSL model-related functions and firmware upgrade.

 For example, topics related to TSL models generally start with`/sys/`. Topics related to firmware upgrade start with `/ota/`. Topics for the device shadow function start with `/shadow/`.

 **Note:** System topics are not completely displayed in the Topic Categories list and the Topic List. For more information about function-specific topics, see related function documentation.

 |
|Custom topics|You can [customize a topic category](reseller.en-US/User Guide/Create products and devices/Topics/Create a topic category.md#) on the Topic Categories tab page according to your business requirements. The topic categories you have customized for the product will be automatically applied to all devices under the product.|

