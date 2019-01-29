# Device properties, events, and services {#concept_mvc_4tw_y2b .concept}

If you have defined the [TSL](../../../../../reseller.en-US/User Guide/Create products and devices/TSL/What is Thing Specification Language (TSL)?.md#) for a product, the devices of this product can separately report data regarding the properties, events, and services that you have defined. This topic describes how data is reported based on the TSL. For information about the data format of TSL, see [The TSL format](../../../../../reseller.en-US/User Guide/Create products and devices/TSL/The TSL format.md#) .

When you create a product, you must select a data type for devices of the product. IoT Platform supports two data types: ICA Standard Data Format \(Alink JSON\) and Do not parse/Custom .

-   ICA Standard Data Format \(Alink JSON\): Devices generate data in the standard format defined by IoT Platform, and then report the data to IoT Platform. We recommend that you select this data type. Note that the following examples in this topic use the Alink JSON format.
-   Do not parse/Custom: Devices report raw data, such as binary data, to IoT Platform, and then IoT Platform parses the raw data to be standard data using the parsing script that you have submitted in the console. For information about how to edit a data parsing script, see [Data parsing](../../../../../reseller.en-US/User Guide/Create products and devices/Data parsing.md#).

## Devices report properties {#section_g4j_5zg_12b .section}

Report data \(Do not parse/Custom\)

-   Request topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw
-   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw\_reply

Report Data \(Alink JSON\)

-   Request topic: /sys/\{productKey\}/\{deviceName\}/thing/event/property/post
-   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/event/property/post\_reply

You can configure [Rules Engine](../../../../../reseller.en-US/User Guide/Rules/Data Forwarding/Overview.md#) to forward the received property data to another Alibaba Cloud product instance. The following figure is an example of rule action configuration.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18885/154875882912151_en-US.png)

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "Power": {
      "value": "on",
      "time": 1524448722000
    },
    "WF": {
      "value": 23.6,
      "time": 1524448722000
    }
  },
  "method": "thing.event.property.post"
}
```

Response data

-   If the data type of the product is Do not parse/Custom, then the response data is similar to the following output, and will be parsed by IoT Platform using the data parsing script before being sent to the device. For information about data processing procedures, see [Devices report properties and events](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Alink protocol.md#).

    ```
    {
    "id":"123",
    "code":200,
    "method":"thing.event.property.post"
    "data":{}
    }
    ```

-   Alink response message

    ```
    {
      "id": "123",
      "code": 200,
      "data": {}
    }
    ```


|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|version|String |The protocol version. Currently, the value is 1.0.|
|params|Object|The request parameters. In the preceding request example, the device reports two properties: Power and WF. Property information includes time \(the time when the property is reported\) and value \(the value of the property\).|
|time|Long|The time when the property is reported.|
|value|Object|The value of the property.|
|method|String |The request method.|

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|code|Integer|The result code. See [Common codes on devices](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Common codes on devices.md#).|
|data|String |The data that is returned when the request is successful.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|6106|map size must less than 200|The number of reported properties exceeds the maximum limit. Up to 200 properties can be reported at a time.|
|6313|tsl service not available| The TSL verification service is not available.

 IoT Platform verifies all the received properties according to the TSLs of products. This error is reported when a system exception occurs. For the TSL definition, see [What is Thing Specification Language \(TSL\)?](../../../../../reseller.en-US/User Guide/Create products and devices/TSL/What is Thing Specification Language (TSL)?.md#).

 **Note:** If the TSL verification service is available, but some reported properties do not match with any properties defined in the TSL, IoT Platform ignores the invalid properties. If all the reported properties do not match with any properties defined in the TSL, IoT Platform ignores them all. In this case, the response will still indicate that the verification is successful.

 |

## Set device properties {#section_jkt_v1x_y2b .section}

Push data to devices \(Do not parse/Custom\)

-   Request topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw
-   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw\_reply

Push data to devices \(Alink JSON\)

-   Request topic: /sys/\{productKey\}/\{deviceName\}/thing/service/property/set
-   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/service/property/set\_reply

You can get property setting results from the topic of data exchange as follows: `/sys/{productKey}/{deviceName}/thing/downlink/reply/message`. You can also configure [Rules Engine](../../../../../reseller.en-US/User Guide/Rules/Data Forwarding/Overview.md#) to forward property setting results to another Alibaba Cloud product instance. The following figure is an example of rule action configuration.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18885/154875882912171_en-US.png)

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "temperature": "30.5"
  },
  "method": "thing.service.property.set"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|version|String |The protocol version. Currently, the value is 1.0.|
|params|Object|The property parameters. In the preceding request example, the property to be set is```
{ "temperature": "30.5" }
```

.|
|method|String |The request method.|

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|code|Integer|The result code. See [Common codes on devices](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Common codes on devices.md#).|
|data|String |The data that is returned when the request is successful.|

## Devices report events {#section_lnn_1bx_y2b .section}

Report data \(Do not parse/Custom\)

-   Request topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw
-   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw\_reply

Report Data \(Alink JSON\)

-   Request topic: /sys/\{productKey\}/\{deviceName\}/thing/event/\{tsl.event.identifier\}/post
-   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/event/\{tsl.event.identifier\}/post\_reply

You can configure [Rules Engine](../../../../../reseller.en-US/User Guide/Rules/Data Forwarding/Overview.md#) to forward the received event data to another Alibaba Cloud product instance. The following figure is an example of rule action configuration.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18885/154875882912169_en-US.png)

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "value": {
      "Power": "on",
      "WF": "2"
    },
    "time": 1524448722000
  },
  "method": "thing.event.{tsl.event.identifier}.post"
}
```

Response message

-   If the data type of the product is Do not parse/Custom, then the response message is similar to the following output, and will be parsed by IoT Platform using the data parsing script before being sent to the device. For information about data processing procedures, see [Devices report properties and events](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Alink protocol.md#).

    ```
    {
    "id":"123",
    "code":200,
    "method":"thing.event.{tsl.event.identifier}.post"
    "data":{}
    }
    ```

-   Alink response message

    ```
    {
      "id": "123",
      "code": 200,
      "data": {}
    }
    ```


|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|version|String |The protocol version. Currently, the value is 1.0.|
|params|List|The parameters of the reported events.|
|value|Object|The event information. In the preceding request example, the events are:```
{
      "Power": "on",
      "WF": "2"
    }
```

|
|time|Long|The UTC timestamp when the event occurs.|
|method|String |The request method.|

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|code|Integer|The result code. See [Common codes on devices](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Common codes on devices.md#).|
|data|String |The data that is returned when the request is successful.|

Examples

For example, an event alarm has been defined in the TSL of a product:

```
{
  "schema": "https://iot-tsl.oss-cn-shanghai.aliyuncs.com/schema.json",
  "link": "/sys/${productKey}/airCondition/thing/",
  "profile": {
    "productKey": "al123456789",
    "deviceName": "airCondition"
  },
  "events": [
    {
      "identifier": "alarm",
      "name": "alarm",
      "desc": "Fan alarm",
      "type": "alert",
      "required": true,
      "outputData": [
        {
          "identifier": "errorCode",
          "name": "ErrorCode",
          "dataType": {
            "type": "text",
            "specs": {
              "length": "255"
            }
          }
        }
      ],
      "method": "thing.event.alarm.post"
    }
  ]
}
```

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "value": {
      "errorCode": "error"
    },
    "time": 1524448722000
  },
  "method": "thing.event.alarm.post"
}
```

**Note:** 

-   `tsl.event.identifier` indicates the event identifier in the TSL. For information about TSL template, see [What is Thing Specification Language \(TSL\)?](../../../../../reseller.en-US/User Guide/Create products and devices/TSL/What is Thing Specification Language (TSL)?.md#).
-   IoT Platform verifies all the events reported by devices according to the TSLs of products. If the reported event does not match with any events defined in the TSL, an error code is returned.

## Call device services {#section_sjk_bbx_y2b .section}

-   Push data to devices \(Do not parse/Custom\)

    -   Request topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw
    -   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw\_reply
-   Push data to devices \(Alink JSON\)

    -   Request topic: /sys/\{productKey\}/\{deviceName\}/thing/service/\{tsl.service.identifier\}
    -   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/service/\{tsl.service.identifier\}\_reply

When you [define a service](../../../../../reseller.en-US/User Guide/Create products and devices/TSL/Define features.md#), you must select a method for the service. The method for a service can be either synchronous method or asynchronous method .

-   Synchronous method: IoT Platform uses the RRPC method to push requests to devices. For information about the RRPC method, see [What is RRPC](../../../../../reseller.en-US/User Guide/RRPC/What is RRPC?.md#).
-   Asynchronous method: IoT Platform pushes requests to devices in an asynchronous manner, and the devices return operation results in an asynchronous manner.

    Only when asynchronous method is selected for a service does IoT Platform subscribe to the response topic. You can get the operation results from the topic of data exchange as follows: `/sys/{productKey}/{deviceName}/thing/downlink/reply/message`.

    You can configure [Rules Engine](../../../../../reseller.en-US/User Guide/Rules/Data Forwarding/Overview.md#) to forward service call results returned by devices to another Alibaba Cloud product instance. The following figure is an example of rule action configuration.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18885/154875882912171_en-US.png)


Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "Power": "on",
    "WF": "2"
  },
  "method": "thing.service.{tsl.service.identifier}"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|version|String |The protocol version. Currently, the value is 1.0.|
|params|Map|The parameters used to call a service, including the identifier and value of the service. Example:```
{
      "Power": "on",
      "WF": "2"
    }
```

|
|method|String |The request method.|

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|code|Integer|The result code. See [Common codes on devices](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Common codes on devices.md#).|
|data|String |The data that is returned when the request is successful.The value of data is determined by the TSL of the product. If the device does not return any information about the service, the value of data is empty. If the device returns service information, the returned data value will strictly comply with the definition of the service in the TSL.

|

Examples

For example, the service SetWeight has been defined in the TSL of the product as follows:

```
{
  "schema": "https://iotx-tsl.oss-ap-southeast-1.aliyuncs.com/schema.json",
  "profile": {
    "productKey": "testProduct01"
  },
  "services": [
    {
      "outputData": [
        {
          "identifier": "OldWeight",
          "dataType": {
            "specs": {
              "unit": "kg",
              "min": "0",
              "max": "200",
              "step": "1"
            },
            "type": "double"
          },
          "name": "OldWeight"
        },
        {
          "identifier": "CollectTime",
          "dataType": {
            "specs": {
              "length": "2048"
            },
            "type": "text"
          },
          "name": "CollectTime"
        }
      ],
      "identifier": "SetWeight",
      "inputData": [
        {
          "identifier": "NewWeight",
          "dataType": {
            "specs": {
              "unit": "kg",
              "min": "0",
              "max": "200",
              "step": "1"
            },
            "type": "double"
          },
          "name": "NewWeight"
        }
      ],
      "method": "thing.service.SetWeight",
      "name": "SetWeight",
      "required": false,
      "callType": "async"
    }
  ]
}
```

Request message for a service call:

```
{
  "method": "thing.service.SetWeight",
  "id": "105917531",
  "params": {
    "NewWeight": 100.8
  },
  "version": "1.0.0"
}
```

Response message:

```
{
  "id": "105917531",
  "code": 200,
  "data": {
    "CollectTime": "1536228947682",
    "OldWeight": 100.101
  }
}
```

**Note:** `tsl.service.identifier` indicates the identifier of the service in TSL. For information about how to define a TSL, see [What is Thing Specification Language \(TSL\)?](../../../../../reseller.en-US/User Guide/Create products and devices/TSL/What is Thing Specification Language (TSL)?.md#).

## Gateway devices report data {#section_rb2_r2t_lgb .section}

A gateway device can report properties and events of itself and properties and events of its sub-devices to IoT Platform.

**Note:** 

-   A gateway can report up to 200 properties at one time.
-   A gateway can report up to 20 events at one time.
-   A gateway can report the properties and events of up to 20 sub-devices.
-   IoT Platform then verifies the devices, topological relationships, and property and event definitions in the TSL. If any one of the verifications fail, the data report also fails.

Report data \(Do not parse/Custom\)

-   Request topic: Topic：/sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw
-   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw\_reply

Report data \(Alink JSON\)

-   Request topic: /sys/\{productKey\}/\{deviceName\}/thing/event/property/pack/post
-   Response topic: /sys/\{productKey\}/\{deviceName\}/thing/event/property/pack/post\_reply

Request message

```
{
  "id": "123", 
  "version": "1.0", 
  "params": {
    "properties": {
      "Power": {
        "value": "on", 
        "time": 1524448722000
      }, 
      "WF": {
        "value": { }, 
        "time": 1524448722000
      }
    }, 
    "events": {
      "alarmEvent1": {
        "value": {
          "param1": "on", 
          "param2": "2"
        }, 
        "time": 1524448722000
      }, 
      "alertEvent2": {
        "value": {
          "param1": "on", 
          "param2": "2"
        }, 
        "time": 1524448722000
      }
    }, 
    "subDevices": [
      {
        "identity": {
          "productKey": "", 
          "deviceName": ""
        }, 
        "properties": {
          "Power": {
            "value": "on", 
            "time": 1524448722000
          }, 
          "WF": {
            "value": { }, 
            "time": 1524448722000
          }
        }, 
        "events": {
          "alarmEvent1": {
            "value": {
              "param1": "on", 
              "param2": "2"
            }, 
            "time": 1524448722000
          }, 
          "alertEvent2": {
            "value": {
              "param1": "on", 
              "param2": "2"
            }, 
            "time": 1524448722000
          }
        }
      }
    ]
  }, 
  "method": "thing.event.property.pack.post"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Request Parameters

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|version|String|The protocol version. Currently, the value is 1.0.|
|params|Object|The request parameters.|
|properties|Object|The information about a property, including property name, value and time when the property was generated.|
|events|Object|The information about an event, including property name, value and time when the event was generated.|
|subDevices|Object|The sub-device information.|
|productKey|String |The ProductKey of a sub-device.|
|deviceName|String |The name of a sub-device.|
|method|String |The request method.|

Response parameters

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String |The message ID.|
|code|Integer|The result code. 200 indicates that the request is successful.|
|data|Object|The data that is returned when the request is successful.|

