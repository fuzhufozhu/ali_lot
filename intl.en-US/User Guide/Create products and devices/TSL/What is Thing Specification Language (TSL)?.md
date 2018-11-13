# What is Thing Specification Language \(TSL\)? {#concept_okp_zlv_tdb .concept}

Thing Specification Language \(TSL\) is a data model that digitizes a physical entity and constructs the entity in the Cloud. On the IoT platform, a TSL model refers to a set of product features. After the features have been defined for a product, the system automatically generates a TSL model of the product. A TSL model describes what a product is, what the product can do, and what services the product can provide.

A TSL model is a file in JSON format. TSL files are the digitized expressions of physical entities, such as sensors, vehicle-mounted devices, buildings and factories. A TSL file describes an entity in three dimensions: property \(what the entity is\), service \(what the entity can do\), and event \(what event information the entity reports\). Defining these three dimensions is to define the product features.

The feature types of a product are Properties, Services and Events. You can define these three types of features on the console.

|Feature Type|Description|
|:-----------|:----------|
|Properties|Describes the running status of a device, such as the current temperature read by the environmental monitoring equipment. Properties support GET and SET request methods. Application systems can send requests to retrieve and set properties.|
|Services|Capabilities or methods of a device that are exposed and can be used by an external requester. You can set the input and output parameters. Compared with properties, services can use instructions to implement more complex business logic, such as a specific task.|
|Events|Events generated during operation. Events typically contain notifications that require action or attention, and they may contain multiple output parameters. For example, an event may be a notification about the completion of a task, a system failure, or a temperature alert. You can subscribe to or push events.|

