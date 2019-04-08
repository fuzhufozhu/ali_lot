# Remote configuration {#concept_j4b_vtw_y2b .concept}

This article introduces Topics and Alink JSON format requests and responses for remote conficuration. For how to use remote configuration, see [Remote configuration](../../../../../reseller.en-US/User Guide/Monitoring and Maintenance/Remote configuration.md#) in User Guide.

## Device requests configuration information from IoT Platform {#section_xg4_fbh_12b .section}

Upstream

-   Request topic: `/sys/{productKey}/{deviceName}/thing/config/get`
-   Reply topic: `/sys/{productKey}/{deviceName}/thing/config/get_reply`

Request message

```
{
  "id": 123,
  "version": "1.0",
  "params": {
    "configScope": "product",
    "getType": "file"
  },
  "method": "thing.config.get"
}
```

Response message

```
{
  "id": "123",
  "version": "1.0",
  "code": 200,
  "data": {
    "configId": "123dagdah",
    "configSize": 1234565,
    "sign": "123214adfadgadg",
    "signMethod": "Sha256",
    "url": "https://iotx-config.oss-cn-shanghai.aliyuncs.com/nopoll_0.4.4.tar.gz?Expires=1502955804&OSSAccessKeyId=XXXXXXXXXXXXXXXXXXXX&Signature=XfgJu7P6DWWejstKJgXJEH0qAKU%3D&security-token=CAISuQJ1q6Ft5B2yfSjIpK6MGsyN1Jx5jo6mVnfBglIPTvlvt5D50Tz2IHtIf3NpAusdsv03nWxT7v4flqFyTINVAEvYZJOPKGrGR0DzDbDasumZsJbo4f%2FMQBqEaXPS2MvVfJ%2BzLrf0ceusbFbpjzJ6xaCAGxypQ12iN%2B%2Fr6%2F5gdc9FcQSkL0B8ZrFsKxBltdUROFbIKP%2BpKWSKuGfLC1dysQcO1wEP4K%2BkkMqH8Uic3h%2Boy%2BgJt8H2PpHhd9NhXuV2WMzn2%2FdtJOiTknxR7ARasaBqhelc4zqA%2FPPlWgAKvkXba7aIoo01fV4jN5JXQfAU8KLO8tRjofHWmojNzBJAAPpYSSy3Rvr7m5efQrrybY1lLO6iZy%2BVio2VSZDxshI5Z3McKARWct06MWV9ABA2TTXXOi40BOxuq%2B3JGoABXC54TOlo7%2F1wTLTsCUqzzeIiXVOK8CfNOkfTucMGHkeYeCdFkm%2FkADhXAnrnGf5a4FbmKMQph2cKsr8y8UfWLC6IzvJsClXTnbJBMeuWIqo5zIynS1pm7gf%2F9N3hVc6%2BEeIk0xfl2tycsUpbL2FoaGk6BAF8hWSWYUXsv59d5Uk%3D",
    "getType": "file"
  }
}
```

Parameter description

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. You need to define IDs for upstream messages using numbers, and the message IDs must be unique within the device.|
|version|String|Protocol version. Currently, the value is 1.0.|
|configScope|String|Configuration scope. Currently, IoT Platform supports only product dimension configuration. Value: product.|
|getType|String|Desired file type of the configuration. Currently, the supported type is file. Set the value to file.|
|method|String|Request method. The value is thing.config.get.|
|configId|String|ID of the configuration.|
|configSize|Long|Size of the configuration file, in bytes.|
|sign|String|Signature value.|
|signMethod|String|Signing method. The supported signing method is Sha256.|
|url|String|The OSS address where the configuration file is stored.|
|code|Integer|Result code. A value of 200 indicates that the operation is successful, and other status codes indicate that the operation has failed.|

Error codes

|Error code|Error message|Description|
|:---------|:------------|:----------|
|6713|thing config function is not available|Remote configuration feature of the product has been disabled. On the Remote Configuration page of the IoT Platform console, enable remote configuration for the product .|
|6710|no data|Not found any configured data.|

## Push configurations​ in the IoT Platform console to devices. {#section_hvc_sbx_y2b .section}

Downstream​

-   Request topic: `/sys/{productKey}/{deviceName}/thing/config/push`
-   Reply topic: `/sys/{productKey}/{deviceName}/thing/config/push_reply`

Devices subscribe to this configuration push topic for configurations that is pushed by IoT Platform. After you have edited and submitted a configuration file in the IoT Platform console, IoT Platform pushes the configuration to the devices in an asynchronous method. IoT Platform subscribes to a data exchange topic for the result of asynchronous calls. The data exchange topic is `/{productKey}/{deviceName}/thing/downlink/reply/message`.

You can use [Rules Engine](../../../../../reseller.en-US/User Guide/Rules/Data Forwarding/Overview.md#) to forward the results returned by the devices to another Alibaba Cloud product. The following figure shows an example of rule action configuration.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18885/155473621612171_en-US.png)

Request message:

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "configId": "123dagdah",
    "configSize": 1234565,
    "sign": "123214adfadgadg",
    "signMethod": "Sha256",
    "url": "https://iotx-config.oss-cn-shanghai.aliyuncs.com/nopoll_0.4.4.tar.gz?Expires=1502955804&OSSAccessKeyId=XXXXXXXXXXXXXXXXXXXX&Signature=XfgJu7P6DWWejstKJgXJEH0qAKU%3D&security-token=CAISuQJ1q6Ft5B2yfSjIpK6MGsyN1Jx5jo6mVnfBglIPTvlvt5D50Tz2IHtIf3NpAusdsv03nWxT7v4flqFyTINVAEvYZJOPKGrGR0DzDbDasumZsJbo4f%2FMQBqEaXPS2MvVfJ%2BzLrf0ceusbFbpjzJ6xaCAGxypQ12iN%2B%2Fr6%2F5gdc9FcQSkL0B8ZrFsKxBltdUROFbIKP%2BpKWSKuGfLC1dysQcO1wEP4K%2BkkMqH8Uic3h%2Boy%2BgJt8H2PpHhd9NhXuV2WMzn2%2FdtJOiTknxR7ARasaBqhelc4zqA%2FPPlWgAKvkXba7aIoo01fV4jN5JXQfAU8KLO8tRjofHWmojNzBJAAPpYSSy3Rvr7m5efQrrybY1lLO6iZy%2BVio2VSZDxshI5Z3McKARWct06MWV9ABA2TTXXOi40BOxuq%2B3JGoABXC54TOlo7%2F1wTLTsCUqzzeIiXVOK8CfNOkfTucMGHkeYeCdFkm%2FkADhXAnrnGf5a4FbmKMQph2cKsr8y8UfWLC6IzvJsClXTnbJBMeuWIqo5zIynS1pm7gf%2F9N3hVc6%2BEeIk0xfl2tycsUpbL2FoaGk6BAF8hWSWYUXsv59d5Uk%3D",
    "getType": "file"
  },
  "method": "thing.config.push"
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
|id|String|Message ID. IoT Platform generates IDs for downstream messages.|
|version|String|Protocol version. Currently, the value is 1.0.|
|configScope|String|Configuration scope. Currently, IoT Platform supports only product dimension configuration. Value: product.|
|getType|String|Desired file type of the configuration. Currently, the supported type is file. Set the value to file.|
|configId|String|ID of the configuration.|
|configSize|Long|Size of the configuration file, in bytes.|
|sign|String|Signature value.|
|signMethod|String|Signing method. The supported signing method is Sha256.|
|url|String|The OSS address where the configuration file is stored.|
|method|String|Request method. The value is thing.config.push.|
|code|Integer|Result code. For more information, see Common codes on devices.|

