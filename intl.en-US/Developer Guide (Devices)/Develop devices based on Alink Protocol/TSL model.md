# TSL model {#concept_dcp_stw_y2b .concept}

A device can publish requests to this topic to obtain the [Device TSL model](../../../../../reseller.en-US/User Guide/Create products and devices/TSL/What is Thing Specification Language (TSL)?.md#) from IoT Platform.

-   Topic：/sys/\{productKey\}/\{deviceName\}/thing/dsltemplate/get
-   Reply topic：/sys/\{productKey\}/\{deviceName\}/thing/dsltemplate/get\_reply

The Allink data format of a request

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.dsltemplate.get"
}
```

The Allink data format of a response

```
{
  "id": "123",
  "code": 200,
  "data": {
    "schema": "https://iot-tsl.oss-cn-shanghai.aliyuncs.com/schema.json",
    "link": "/sys/1234556554/airCondition/thing/",
    "profile": {
      "productKey": "1234556554",
      "deviceName": "airCondition"
    },
    "properties": [
      {
        "identifier": "fan_array_property",
        "name": "Fan array property",
        "accessMode": "r",
        "required": true,
        "dataType": {
          "type": "array",
          "specs": {
            "size": "128",
            "item": {
              "type": "int"
            }
          }
        }
      }
    ],
    "events": [
      {
        "identifier": "alarm",
        "name": "alarm",
        "desc": "Fan alert",
        "type": "alert",
        "required": true,
        "outputData": [
          {
            "identifier": "errorCode",
            "name": "Error code",
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
    ],
    "services": [
      {
        "identifier": "timeReset",
        "name": "timeReset",
        "desc": "Time calibration",
        "inputData": [
          {
            "identifier": "timeZone",
            "name": "Time zone",
            "dataType": {
              "type": "text",
              "specs": {
                "length": "512"
              }
            }
          }
        ],
        "outputData": [
          {
            "identifier": "curTime",
            "name": "Current time",
            "dataType": {
              "type": "date",
              "specs": {}
            }
          }
        ],
        "method": "thing.service.timeReset"
      },
      {
        "identifier": "set",
        "name": "set",
        "required": true,
        "desc": "Set properties",
        "method": "thing.service.property.set",
        "inputData": [
          {
            "identifier": "fan_int_property",
            "name": "Integer property of the fan",
            "accessMode": "rw",
            "required": true,
            "dataType": {
              "type": "int",
              "specs": {
                "min": "0",
                "max": "100",
                "unit": "g/ml",
                "unitName": "Millilitter"
              }
            }
          }
        ],
        "outputData": []
      },
      {
        "identifier": "get",
        "name": "get",
        "required": true,
        "desc": "Get properties",
        "method": "thing.service.property.get",
        "inputData": [
          "array_property",
          "fan_int_property",
          "batch_enum_attr_id",
          "fan_float_property",
           "fan_double_property",
          "fan_text_property",
          "Maid ",
          "batch_boolean_attr_id",
          "fan_struct_property"
        ],
        "outputData": [
          {
            "identifier": "fan_array_property",
            "name": "Fan array property",
            "accessMode": "r",
            "required": true,
            "dataType": {
              "type": "array",
              "specs": {
                "size": "128",
                "item": {
                  "type": "int"
                }
              }
            }
          }
        ]
      }
    ]
  }
}
```

Parameter descriptions:

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. Reserve the parameter value for future use.|
|version|String |Protocol version. Currently, the value is 1.0.|
|params|Object|Leave this parameter empty.​|
|method|String|Request method.|
|productKey|String|ProductKey. In the example, the ProductKey is 1234556554.|
|deviceName|String |Device name. In the example, the device name is airCondition.|
|data|Object|TSL model of the device. For more information, see[What is Thing Specification Language \(TSL\)?](../../../../../reseller.en-US/User Guide/Create products and devices/TSL/What is Thing Specification Language (TSL)?.md#)|

Error codes

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6321|tsl: device not exist in product|The device does not exist.​|

