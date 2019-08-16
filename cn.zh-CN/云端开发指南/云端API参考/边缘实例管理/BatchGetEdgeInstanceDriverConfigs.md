# BatchGetEdgeInstanceDriverConfigs {#reference_878631 .reference}

调用该接口批量获取边缘实例驱动配置。

## 限制说明 {#section_v10_cpa_vll .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_7vo_e8x_nch .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchGetEdgeInstanceDriverConfigs。|
|InstanceId|String|是|边缘实例ID。|
|DriverIds|List\(String\)|是|要查询的驱动ID列表。 最多可传入20个驱动ID。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_nju_5v7_1z4 .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|
|DriverConfigList|List|调用成功时，返回的数据。详情请参见下表DriverConfig。|

|名称|类型|描述|
|:-|:-|:-|
|DriverId|String|驱动ID。|
|ConfigList|List|驱动配置信息，详情请参见下表Config。|

|名称|类型|描述|
|:-|:-|:-|
|ConfigId|String|配置ID。|
|Format|String|配置文件格式：KV、JSON、或FILE。|
|Content|String|配置内容文本或存储配置内容文件的OSS地址。|
|Key|String|配置的关键字。在有多个配置的情况下，用于区分配置。|

## 示例 {#section_vy6_u4o_lv0 .section}

请求示例

``` {#codeblock_6qu_7vx_dem}
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchGetEdgeInstanceDriverConfigs
&DriverIds.1=021d154d2a2f4dd7a489773d9e04****
&InstanceId=F3APY0tPLhmgGtx0****
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_69x_ofn_dnc}
    {
      "DriverConfigList": [
        {
          "DriverId": "021d154d2a2f4dd7a489773d9e04****",
          "ConfigList": [
            {
              "Format": "JSON",
              "Content": "{\"test\":123}",
              "ConfigId": "dac71722ceac4a299dbf3e8dc3c8****"
            }
          ]
        }
      ],
      "RequestId": "D6113390-F507-458B-8544-7B01F945630B",
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_f6y_blp_sol}
    <?xml version="1.0" encoding="UTF-8"?>
    <BatchGetEdgeInstanceDriverConfigsResponse>
      <DriverConfigList>
        <DriverConfig>
          <DriverId>021d154d2a2f4dd7a489773d9e04503c</DriverId>
          <ConfigList>
            <Config>
              <Format>JSON</Format>
              <Content>{"test":123}</Content>
              <ConfigId>dac71722ceac4a299dbf3e8dc3c89c92</ConfigId>
            </Config>
          </ConfigList>
        </DriverConfig>
      </DriverConfigList>
      <RequestId>D6113390-F507-458B-8544-7B01F945630B</RequestId>
      <Code>Success</Code>
      <Success>true</Success>
    </BatchGetEdgeInstanceDriverConfigsResponse>
    ```


