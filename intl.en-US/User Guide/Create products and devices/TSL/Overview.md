# Overview {#concept_okp_zlv_tdb .concept}

Thing Specification Language \(TSL\) is a data model that digitizes a physical entity and constructs the entity data model in IoT Platform. In IoT Platform, a TSL model refers to a set of product features. After you have defined features for a product, the system automatically generates a TSL model of the product. A TSL model describes what a product is, what the product can do, and what services the product can provide.

A TSL model is a file in JSON format. TSL files are the digitized expressions of physical entities, such as sensors, vehicle-mounted devices, buildings and factories. A TSL file describes an entity in three dimensions: property \(what the entity is\), service \(what the entity can do\), and event \(what event information the entity reports\). Defining these three dimensions is to define the product features.

Therefore, the feature types of a product are Properties, Services and Events. You can define these three types of features in the console.

|Feature type|Description|
|:-----------|:----------|
|Property|Describes a running status of a device, such as the current temperature read by the environmental monitoring equipment. You can use GET and SET methods to send requests to get and set device properties.|
|Service|Indicates a feature or method of a device that can be used by a user. You can set input parameters and output parameters for a service. Compared with properties, services can implement more complex business logic, for example, a specific task.|
|Event|Indicates the notifications of a type of event occurred when a device is running. Events typically indicate notifications that require actions or attention, and they may contain multiple output parameters. For example, events can be notifications about the completion of tasks, system failures, or temperature alerts. You can subscribe to events or push events to a message receiving target.|

## Use TSL {#section_kvm_mnt_xfb .section}

1.  In the IoT Platform console, [Define features](intl.en-US/User Guide/Create products and devices/TSL/Define features.md#) or [Import Thing Specification Language \(TSL\)](intl.en-US/User Guide/Create products and devices/TSL/Import Thing Specification Language (TSL).md#).
2.  Develop the SDK. See the documentations of [Link Kit SDK](https://www.alibabacloud.com/help/product/93051.htm) for help information.
3.  Connect the SDK to IoT Platform. Then, devices can report properties and events to IoT Platform, and in IoT Platform, you can set properties and call device services.

