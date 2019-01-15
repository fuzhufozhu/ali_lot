# Disable and delete devices {#concept_iyv_ptw_y2b .concept}

Gateways can disable and delete their sub-devices.

## Disable devices​ {#section_xwd_p1h_12b .section}

Downstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/disable
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/disable\_reply

This topic disables a device connection. IoT Platform publishes messages to this topic asynchronously, and the devices subscribe to this topic.  Gateways can subscribe to this topic to disable the corresponding sub-devices.

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.disable"

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
|id|String|Message ID.|
|version|String|Protocol version. Currently, the value is 1.0.|
|params|Object|Request parameters. Leave empty.|
|method|String|Request method.|
|code|Integer|Results information. For more information, see[Common codes on devices](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Common codes on devices.md#)|

## Enable devices​ {#section_mrb_hbx_y2b .section}

Downstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/enable
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/enable\_reply

This topic enables a device connection. IoT Platform publishes messages to this topic asynchronously, and the devices subscribe to this topic. Gateways can subscribe to this topic to enable the corresponding sub-devices.

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.enable"
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

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID.|
|version|String|Protocol version. Currently, the value is 1.0.|
|params|Object|Request parameters. Leave empty.|
|method|String|Request method.|
|code|Integer|Result code. For more information, see the common codes.|

## Delete devices {#section_cls_hbx_y2b .section}

Downstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/delete
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/delete\_reply

This topic deletes a device connection. IoT Platform publishes messages to this topic asynchronously, and the devices subscribe to this topic. Gateways can subscribe to this topic to delete the corresponding sub-devices.

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.delete"
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

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID.|
|version|String|Protocol version. Currently, the value is 1.0.|
|params|Object|Request parameters. Leave empty.|
|method|String|Request method.|
|code|String|Result code. For more information, see the common codes.|

