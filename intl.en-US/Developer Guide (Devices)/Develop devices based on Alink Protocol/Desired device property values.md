# Desired device property values {#concept_hqg_nty_zgb .concept}

After you set a desired property value for a device in IoT Platform, the property value is updated in real time if the device is online. If the device is offline, the desired value is cached in IoT Platform. When the device comes online again, it will obtain the desired value and update the property value. This topic describes the message formats related to desired property values.

## Obtain desired property values {#section_cjc_x5y_zgb .section}

Upstream data in Alink JSON format

A device requests the desired property values from IoT Platform.

-   Request topic: `/sys/{productKey}/{deviceName}/thing/property/desired/get`
-   Response topic: `/sys/{productKey}/{deviceName}/thing/property/desired/get_reply`

Request format

```
{
    "id" : "123",
    "version": "1.0",
    "params" : [
        "power",
        "temperature"
    ]
}
```

Response format

```
{
    "id":"123",
    "code": 200,
    "data":{
		"power": {
			"value": "on",
			"version": 2
		}
	}
}
```

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. Define the message ID to be a string of numbers, and be unique in the device.|
|version|String|The protocol version. Currently, the value can only be 1.0.|
|params|List|The identifier list of properties of which you want to obtain the desired values. In this example, the following property identifiers are listed:

 ```
[ 
 "power",
 "temperature"
 ]
```

 |

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID.|
|code|Integer|The result code. For more information, see the [common codes on the device](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Common codes on devices.md#).|
|data|Object|The desired value information that is returned. In this example, the desired value information about property "power" is returned. The information includes the value and version of the property.

 ```
{
  "power": {
    "value": "on", 
    "version": 2
  }
}
```

 **Note:** If no desired value is set for a property in IoT Platform or the desired value has been cleared, the returned data will not contain the identifier of this property. In this example, the property "temperature" does not have a desired value, therefore, the returned data does not contain this property identifier.

 For more information about the parameters in data, see the following table [Parameters in data](#).

 |

|Parameter|Type|Description|
|:--------|:---|:----------|
|key|String|The identifier of the property, such as "power" in this example.|
|value|O​bject|The desired value.|
|version|Integer|The current version of the desired value. **Note:** When you set the desired property value for the first time, this value is 0. After the first desired value is set, the version automatically changes to 1. Then, the version increases by 1 every time you set the desired value.

 |

## Clear desired property values {#section_kqt_y5y_zgb .section}

Upstream data in Alink JSON format

Requests to clear the desired property values that are cached in IoT Platform.

-   Request topic: `/sys/{productKey}/{deviceName}/thing/property/desired/delete`
-   Response topic: `/sys/{productKey}/{deviceName}/thing/property/desired/delete_reply`

Request format

```
{
    "id" : "123",
    "version": "1.0",
    "params" : [{
		"power": {
			"version": 1
		},
		"temperature": {
		}
	}
}
```

Response format

```
{
    "id":"123",
    "code":200,
    "data":{
	}
}
```

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. Define the message ID to be a string of numbers, and be unique in the device.|
|version|String|The protocol version. Currently, the value can only be 1.0.|
|params|List|The list of the properties of which you want to clear the desired values. A property is identified by the identifier and version. For example: ```
{
  "power": {
    "version": 1
  }, 
  "temperature": { }
}
```

 For more information about params, see the following table [Parameters in params](#).

 |

|Parameter|Type|Description|
|:--------|:---|:----------|
|key|String|The identifier of the property. In this example, the following property identifiers are listed: power and temperature.|
|version|Integer|The current version of the desired value. **Note:** 

-   You can obtain the value of the version parameter from topic `/sys/{productKey}/{deviceName}/thing/property/desired/get`.
-   If you set version to 2, IoT Platform clears the desired value only if the current version is 2. If the current version of the desired value is 3 in IoT Platform, this clear request will be ignored.
-   If you are not sure about the current version, do not specify this parameter in the request. When there is no version in the request, IoT Platform does not verify the version, but clears the desired value directly.

 |

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID.|
|code|Integer|The result code. For more information, see the [common codes on the device](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Common codes on devices.md#).|
|data|String|The returned data.|

