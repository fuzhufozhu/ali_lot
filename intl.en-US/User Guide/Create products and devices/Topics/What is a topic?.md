# What is a topic? {#concept_mny_tnl_vdb .concept}

Servers and devices communicate with each other in IoT Platform through topics. Topics are associated with devices, and topic categories are associated with products.

## What is a topic category? {#section_exz_xv5_vdb .section}

To simplify authorization operations and facilitate communication between devices and IoT Platform, topic categories were introduced. When you create a product, IoT Platform will create a default topic category  for the product. In addition, when you create a device, the topic category will be automatically assigned to the device. You do not need to authorize each individual device to publish or subscribe to a topic.

![](images/35287_en-US.png "The process of automatically creating a topic")

When you create a product, IoT Platform automatically creates standard topic categories for the product. You can view all topic categories of the product on the Topic Categories  tab page.

Description of topic categories:

-   A topic category is a set of topics within the same product. For example,  the topic category `/${productKey}/${deviceName}/update` is a collection of the specific topics:  `/${productKey}/device1/update` and `/${productKey}/device2/update`.
-   The topic category must use a forward slash \(/\) to delimit the topic hierarchy. Two of the category levels are reserved: `${productKey}` represents the product identifier, and `${deviceName}` represents the device name.
-   Each category level can only contain letters, numbers, and underscores \(\_\). Topic category levels cannot be left empty.
-   Operations available for devices: **Publish** indicate that the device can publish messages to a topic. **Subscribe** indicates that the device can subscribe to messages of a topic.
-   IoT Platform Basic supports customized topic categories. Customizing topic categories allows for flexible communication to suit your business needs. Customizing topic categories and modifying category level names is not supported  in IoT Platform Pro.
-   The system-defined topic categories are pre-defined by IoT Platform Pro,  do not support customization, and do not begin with `/${productKey}`. For example, in IoT Platform Pro, topic categories provided for the Thing Special Language \(TSL\)  begin with `/sys/`, topic categories provided for firmware upgrades begin with `/ota/`, and topic categories provided for device shadows  begin with `/shadow/`.

## What is a topic? {#section_ozb_bw5_vdb .section}

A topic category is used for topic definition rather than communication. A topic is used for communication.

-   Topics and topic categories use the same format. The difference is that in a topic category, the `${deviceName}` is a variable, but in a topic  it represents a specific device name.
-   A topic is automatically derived from the device name and the topic category of the product. A topic contains a device name \(`deviceName`\), which can only be used in Pub/Sub  communication. For example, the topic `/${productKey}/device1/update` is owned by the device with name device1 . Therefore, you can only publish or subscribe to messages to this topic for the device  with name device1, and cannot use it for device with name device2 to publish or subscribe to messages.
-   When you configure the rules engine, the topic that you configure can contain one wildcard character.

    |Wildcard character|Description|
    |:-----------------|:----------|
    |\#|Must be the last character in the topic, and works as a wildcard by matching all topics in the current tree and all sub-trees of the topic hierarchy. For example, the topic `/productKey/device1/#` can represent `/productKey/device1/update` and `productKey/device1/update/error`.|
    |+|Matches all topics in the current tree of the topic hierarchy. For example, the topic `/productKey/+/update`  can represent `/${productKey}/device1/update`  and `/${productKey}/device2/update`.|


