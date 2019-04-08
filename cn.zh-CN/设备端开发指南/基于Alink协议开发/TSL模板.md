# TSL模板 {#concept_dcp_stw_y2b .concept}

设备可以通过上行请求获取设备的[TSL模板](../../../../../intl.zh-CN/用户指南/产品与设备/物模型/概述.md#)。

-   请求Topic：`/sys/{productKey}/{deviceName}/thing/dsltemplate/get`
-   响应Topic：`/sys/{productKey}/{deviceName}/thing/dsltemplate/get_reply`

Alink请求数据格式

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.dsltemplate.get"
}
```

Alink响应数据格式

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
        "name": "风扇数组属性",
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
        "desc": "风扇警报",
        "type": "alert",
        "required": true,
        "outputData": [
          {
            "identifier": "errorCode",
            "name": "错误码",
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
        "desc": "校准时间",
        "inputData": [
          {
            "identifier": "timeZone",
            "name": "时区",
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
            "name": "当前的时间",
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
        "desc": "属性设置",
        "method": "thing.service.property.set",
        "inputData": [
          {
            "identifier": "fan_int_property",
            "name": "风扇整数型属性",
            "accessMode": "rw",
            "required": true,
            "dataType": {
              "type": "int",
              "specs": {
                "min": "0",
                "max": "100",
                "unit": "g/ml",
                "unitName": "毫升"
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
        "desc": "属性获取",
        "method": "thing.service.property.get",
        "inputData": [
          "array_property",
          "fan_int_property",
          "batch_enum_attr_id",
          "fan_float_property",
          "fan_double_property",
          "fan_text_property",
          "fan_date_property",
          "batch_boolean_attr_id",
          "fan_struct_property"
        ],
        "outputData": [
          {
            "identifier": "fan_array_property",
            "name": "风扇数组属性",
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

参数说明：

|参数|取值|说明|
|:-|:-|:-|
|id|String|消息ID号。需定义为String类型的数字，且设备维度唯一。|
|version|String|协议版本号，目前协议版本号为1.0。|
|params|Object|为空即可。|
|method|String|请求方法，取值thing.dsltemplate.get。|
|productKey|String|产品的Key，示例中取值为1234556554。|
|deviceName|String|设备名称，示例中取值为airCondition。|
|data|Object|物的TSL描述，具体参考[物模型文档](../../../../../intl.zh-CN/用户指南/产品与设备/物模型/概述.md#)。|

错误码

|错误码|消息|描述|
|:--|:-|:-|
|460|request parameter error|请求参数错误。|
|6321|tsl: device not exist in product|设备不存在。|

