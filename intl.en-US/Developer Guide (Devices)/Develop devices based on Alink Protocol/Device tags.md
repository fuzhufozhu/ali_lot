# Device tags {#concept_pvz_qtw_y2b .concept}

Some static extended device information, such as vendor model and device model, can be saved as device tags.

## Report tags {#section_hnn_z1h_12b .section}

Upstrea​m​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/update
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/update\_reply

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "attrKey": "Temperature",
      "attrValue": "36.8"
    }
  ],
  "method": "thing.deviceinfo.update"
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

Parameter description

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. Reserve the value of the parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object| Request parameters.

 This parameter can contain a maximum of 200 items.​

 |
|method|String|Request method.|
|attrKey|String| Tag name.​

 -   Length: Up to 100 bytes.
-   Valid characters: Lowercase letters a to z, uppercase letters A to Z, digits 0 to 9, and underscores \(\_\).
-   The tag name must start with an English letter or underscore \(\_\).

 |
|attrValue|String|Tag value.​|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error codes

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6100|device not found|The device does not exist.|

## Delete tags​ {#section_dzs_jbx_y2b .section}

Upstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/delete
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/delete\_reply

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "attrKey": "Temperature"
    }
  ],
  "method": "thing.deviceinfo.delete"
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

Parameter description

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. Reserve the value of the parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters.|
|method|String|Request method.|
|attrKey|String| Tag name.​

 -   Length: Up to 100 bytes.
-   Valid characters: Lowercase letters a to z, uppercase letters A to Z, digits 0 to 9, and underscores \(\_\).
-   The tag name must start with an English letter or underscore \(\_\).

 |
|attrValue|String|Tag value.​|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6100|device not found|The device does not exist.|

