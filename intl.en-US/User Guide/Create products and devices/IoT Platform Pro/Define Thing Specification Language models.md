# Define Thing Specification Language models {#concept_okp_zlv_tdb .concept}

Thing Specification Language \(TSL\) is a data model that digitizes a physical entity and constructs the entity in the cloud.  On the IoT platform, a TSL model refers to a set of product features. After the features are defined for a product, the system automatically generates a TSL model of the product. A TSL model describes what a product is, what the product can do, and what services the product can provide.

## Product feature types {#section_srr_1f4_vdb .section}

The product feature types include properties, services, and events.

|Feature type|Description|
|:-----------|:----------|
|Property|Describes the running status of a device, such as the current temperature read by the environmental monitoring equipment. Properties support the GET and SET  request methods. Application systems can send requests to get and set properties.|
|Service|Capabilities or methods of a device that are exposed and can be used by an external requester. You can set the input and output parameters. Compared with properties,  services can use instructions to implement more complex business logic, such as performing a specific task.|
|Event|Events generated during operation. Events typically contain notifications that require action or attention, and they may contain multiple output parameters. For example, an event may be a notification about the completion of a task, a system failure, or a temperature alert. You can subscribe to or push events.|

## Define features {#section_xxh_z24_vdb .section}

After you have successfully created a Pro Edition product, you can add custom features to the product.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).
2.  On the Products page, click **View** next to the product.
3.  Click** Define Feature** \> **Add**.
4.  Choose a feature type from **Property**, **Service**, and **Event**, configure the parameters of the feature, and then click **OK**.

The following sections describe the settings for adding properties, services, and events.

After defining the product features, you can click **View TSL** on the Define Feature page to view  the feature description in JSON format. You can also click **Edit** for a feature to edit the feature.

## Add properties {#section_w4t_x44_vdb .section}

On the Add Feature page, select **Property** as the feature type.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12829/2462_en-US.png)

-   Feature Name: Property name, such as power consumption. A product cannot have duplicate feature names. If you have chosen a feature template during product creation, the system displays standard properties from the standard feature library for you to choose when you enter a feature name.
-   Identifier: Identifies a property that is unique under a product. The identifier takes the value of identifier in the Alink JSON format. The device uses this identifier as the key to report property data. The cloud verifies the identifier and determines whether to accept the data or not. An identifier may contain English letters, numbers, and underscores \(\_\), for example, PowerConsumption.
-   Data Type: Property type. Values include **int32** \(32-bit integral\), **float** \(single-precision floating-point\), **double** \(double-precision floating-point\), **enum** \(enumeration\), **bool** \(Boolean\), **text** \(character string\), **date** \(time stamp\), **struct** \(JSON object\), and **array** \(array\).
    -   int32/float/double: Specifies an integer or floating point with the predefined value range and unit.
    -   enum: Specifies an enumeration entry with the predefined value and description, for example, 1-Heating mode and 2-Cooling mode.
    -   bool: Specifies the boolean value. Values include 0 and 1, for example, 0-Disabled and 1-Enabled.
    -   text: Specifies a string with a predefined length up to 1024 bytes.
    -   date: Specifies a UTC time stamp in strings, in milliseconds.
    -   struct: Defines a JSON struct and adds JSON parameters, such as defining the color definition of a light as the Red, Green, and Blue parameters. Struct nesting is not supported.
    -   array: Specifies the data type of the elements in an array. Values are **int32**, **float**, **double**, and **text**. The data types of all elements in an array must be the same, and an array can contain a maximum of 128 elements.
-   Read/Write Type: Values include **Read/Write** and **Read-Only**. You can use both the GET and SET methods if the Read/Write Type option is set to Read/Write. If the Read/Write Type option is set to Read/Only, you can use only the GET method.
-   Description: Describes a property or adds remarks to it.

## Add services {#section_zww_tp4_vdb .section}

On the Add Feature page,  select **Service** as the feature type.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12829/2473_en-US.png)

-   Feature Name: Service name. If you have chosen a feature template during product creation, the system displays standard services from the standard feature library for you to choose when you enter a feature name.
-   Identifier: Identifies a service that is unique under a product. An identifier takes the value of identifier in the Alink JSON format. The device uses this identifier as the key to report service data. The identifier may contain English letters, numbers, and underscores \(\_\).
-   Invoke Method: Values are **Asynchronous** and **Synchronous**. For an asynchronous call, the cloud returns the result directly after the call is executed instead of waiting for responses from the device. For a synchronous call, the cloud waits for the response from the device. If no responses are received, the call times out.
-   \(Optional\) Input Parameters: Input parameters of the service. Click **Add Parameter**, and add an input parameter in the dialog box that appears. You can either choose a property as an input parameter or enter an input parameter. For example, specify the Sprinkling Interval and Sprinkling Amount properties as the input parameters of the Automatic Sprinkler service feature. When Automatic Sprinkler is called, the sprinkler automatically starts irrigation according to the sprinkling interval and amount.

    **Note:** A service supports a maximum of 10 input parameters.

-   \(Optional\) Output Parameters: Output parameters of the service. Click **Add Parameter**, and add an output parameter in the dialog box that appears. You can either choose a property as an output parameter or enter an output parameter. For example, specify the Soil Humidity property as an output parameter. When Automatic Sprinkler is called, the cloud returns the data about soil humidity.

    **Note:** A service supports a maximum of 10 output parameters.

-   Description: Describes a service or adds remarks to it.

## Add events {#section_dfw_3ns_vdb .section}

On the Add Feature page, select **Event** as the feature type.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12829/2481_en-US.png)

-   Feature Name: Event name.
-   Identifier: Identifies an event that is unique under a product. The identifier takes the value of identifier in the Alink JSON format. The device uses this identifier as the key to report event data, such as ErrorCode.
-   Event Type: Values include **Info**, **Alert**, and **Error**. **Info** represents general notifications reported by the device, such as the completion of a specific task. **Alert** and **Error** represent exceptions or anomalies reported by the device during operation. They have higher priority. You can perform logic processing or analytics, depending on the event type.
-   \(Optional\) Output Parameters: Output parameters of an event. Click **Add Parameter**, and add an output parameter in the dialog box that appears. You can either choose a property as an output parameter or enter an output parameter. For example, specify the Voltage property as an output parameter. In this case, the device reports the error with the current voltage value for further fault diagnosis.

    **Note:** An event supports a maximum of 10 output parameters.

-   Description: Describes an event and adds remarks to it.

## TSL JSON parameters {#section_ldw_gys_vdb .section}

After defining all the features for a product, the system automatically generates a TSL model for the product. TSL uses the JSON format . On the Define Feature page, click **View TSL** to view TSL in JSON format. The following describes the TSL JSON parameters:

```

{
"schema": "TSL schema of a thing",
"link": "System-level URI in the cloud, used to invoke services and subscribe to events",
"profile ":{
"productKey": " Product key",
"deviceName": "Device name"
},
"properties": [
{
"identifier": "Identifies a property that is unique under a product",
"name": "Property name",
"accessMode": "Read/write type of properties, including Read-Only and Read/Write",
"required": "Whether a property that is required in the standard category is also required for a standard feature",
"dataType": {
"type": "Property type: int (original), float (original), double (original), text (original), date (UTC string in milliseconds), bool (integer, 0 or 1), enum (integer), struct (supports int, float, double, text, date, and bool), array (supports int, double, float, and text)",
"specs": {
"min": "Minimum value, available only for the int, float, and double property types",
"max": "Maximum value, available only for the int, float, and double property types",
"unit": "Property unit",
"unitName": "Unit name",
"size":"Array size, up to 128 elements, available only for the array property type",
"item": {
"type":"Type of an array element "
}
}
}
}
],
"events": [
{
"identifier": "Identifies an event that is unique under a product, where "post" are property events reported by default",
"name": "Event name",
"desc": "Event description",
"type": "Event types, including info, alert, and error",
"required": "Whether the event is required for a standard feature",
"outputData": [
{
"identifier": "Uniquely identifies a parameter",
"name": "Parameter name",
"dataType": {
"type": "Property type: int (original), float (original), double (original), text (original), date (UTC string in milliseconds)，bool (integer, 0 or 1), enum (integer), struct (supports int, float, double, text, date, and bool), array (supports int, double, float, and text)",
"specs": {
"min": "Minimum value, available only for the int, float, and double property types",
"max": "Maximum value, available only for the int, float, and double property types",
"unit": "Property unit",
"unitName": "Unit name",
"size":"Array size, up to 128 elements, available only for the array property type)",
"item": {
"type":"Type of an array element "
}
}
}
}
],
"method": "Name of the method to invoke the event, generated according to the identifier"
}
],
"services": [
{
"identifier": "Identifies a service that is unique under a product (set and get are default services generated according to the read/write type of the property)",
"name": "Service name",
"desc": "Service description",
"required": "Whether the service is required for a standard feature",
"inputData": [
{
"identifier": "Uniquely identifies an input parameter",
"name": "Name of an input paramerter",
"dataType": {
"type": "Property type: int (original), float (original), double (original), text (original), date (UTC string in milliseconds), bool (integer, 0 or 1), enum (integer), struct (supports int, float, double, text, date, and bool), array (supports int, double, float, and text)",
"specs": {
"min": "Minimum value, available only for the int, float, and double property types",
"max": "Maximum value, available only for the int, float, and double property types",
"unit": "Property unit",
"unitName": "Unit name",
"size":"Array size, up to 128 elements, available only for the array property type",
"item": {
"type":"Type of an array element "
}
}
}
}
],
"outputData": [
{
"identifier": "Uniquely identifies an output parameter",
"name": "Name of an output parameter",
"dataType": {
"type": "Property type: int (original), float (original), double (original), text (original), date (UTC string in milliseconds), bool (integer, 0 or 1), enum (integer), struct (supports int, float, double, text, date, and bool), array (supports int, double, float, and text)",
"specs": {
"min": "Minimum value, available only for the int, float, and double property types",
"max": "Maximum value, available only for the int, float, and double property types",
"unit": "Property unit",
"unitName": "Unit name",
"size":"Array size, up to 128 elements, available only for the array property type",
"item": {
"type":"Type of an array element, available only for the array property type"
}
}
}
}
],
"method": "Name of the method to invoke the service, which is generated according to the identifier"
}
]
}
```

