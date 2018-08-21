# IoT Platform terms {#concept_brv_q23_d2b .concept}

The section describes the terms that are used in Alibaba Cloud IoT Platform.

## Terms {#section_dnz_hg3_d2b .section}

|Term|Description|
|----|-----------|
|Product|A product is a set of devices that have the same features. IoT Platform issues a unique ProductKey for each product. One product may consist of thousands of devices.|
|Device|A physical device that constitutes a product. IoT Platform issues a DeviceName that is unique under the same product for each device. Devices can connect directly to IoT Platform, or be mounted as sub-devices to a gateway that is connected to IoT Platform.|
|Gateway|A gateway can connect directly to IoT Platform and provide sub-device management features. Sub-devices can only communicate with IoT Platform through a gateway.|
|Sub-device|A sub-device is essentially a device. Sub-devices cannot connect directly to IoT Platform and can only get connected through a gateway.|
|Three key fields|The three key fields are ProductKey, DeviceName, and DeviceSecret.-   ProductKey is the unique identifier of a product in IoT Platform. This parameter is very important and used in device authentication and communication. You should keep this parameter safe.
-   DeviceName is the device name that is automatically generated or defined by the user during device registration. Each device has a unique DeviceName under the same product. This parameter is very important and used in device authentication and communication. You must keep this parameter safe.
-   DeviceSecret is the private key issued by IoT Platform for each device. DeviceSecret is used in pair with DeviceName. This parameter is very important and used in device authentication. You must keep this parameter safe and never disclose this parameter.

|
|ProductSecret|ProductSecret is the private key issued by IoT Platform for each product. ProductSecret is usually used in pair with ProductKey for unique-certificate-per-product authentication. This parameter is very important. You must keep this parameter safe and never disclose this parameter.|
|Topic|A topic is a UTF-8 character string that is used as a transmission medium during Pub/Sub communication. A device can publish messages to a topic or subscribe to messages from a topic.|
|Topic category|A topic category is a set of topics associated with different devices under the same product. $\{productKey\} and $\{deviceName\} can be combined to specify a unique device. A topic category is applicable to all devices under the same product.|
|Publish|The operation permission that allows a device to publish messages to a topic.|
|Subscribe|The operation permission that allows a device to subscribe to messages from a topic.|
|RRPC|RRPC is short for Revert-RPC. RRPC allows you to send a request to a specified device and receive a response from the device.|
|Tag|Tags consist of product tags and device tags.-   Product tags describe the information that is common to all devices under the same product.
-   Device tags describe the unique features of devices. You can add custom tags based on your needs.

|
|Alink|The protocol for communication between the devices and IoT Platform.|
|TSL model|IoT Platform uses the Thing Specification Language \(TSL\) to describe devices. A TSL model includes properties, services and events. TSL models use JSON format. You can organize data based on TSL and report data to the Cloud.|
|Property|A feature that describes the running status of a device, such as the temperature information collected by an environmental monitoring equipment. Properties support GET and SET request methods. Application systems can send requests to retrieve and set properties.|
|Service|A feature that describes the capabilities or methods provided by a device that can be used by external requesters. You can specify the input and output parameters. Compared with properties, services can use one command to implement more complex business logic, such as performing a specific task.|
|Event|A feature that describes the events that are generated when a device is running. Events usually contain notifications that require action or attention, and may contain multiple output parameters. For example, an event may be a notification that a task is completed, a device fault that has occurred, or a temperature alert. You can subscribe to or push events.|
|Data parsing script|For devices under a product created in IoT Platform Pro, if the passthrough or custom mode is used to send data to IoT Platform, you need to write data parsing scripts to convert the binary data or custom JSON data that is sent by the device to Alink JSON data.|
|Device shadow|A device shadow is a JSON file that is used to store the status information for a device or application. Each device has a unique device shadow in IoT Platform. Regardless of whether the device is connected to the Internet, you can use the device shadow to retrieve and set the device status through MQTT or HTTP.|
|Rules engine|Rules engine provides a SQL-based language that enables you to filter the data from topics and send processed data to other Alibaba Cloud services, such as Message Service and Table Store.|

