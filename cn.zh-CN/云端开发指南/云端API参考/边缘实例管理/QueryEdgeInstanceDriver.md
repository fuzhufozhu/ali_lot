# QueryEdgeInstanceDriver {#reference_878626 .reference}

调用该接口查询边缘实例的驱动列表。

## 限制说明 {#section_s61_pox_9rt .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_13g_aid_5uf .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryEdgeInstanceDriver。|
|InstanceId|String|是|边缘实例ID。|
|PageSize|Integer|是|返回结果中每页显示的记录数量。最大取值30；最小取值10。如果传入的值小于10，系统按10处理。|
|CurrentPage|Integer|是|从返回结果中的第几页开始显示。最小取值 1。如果传入的值小于1，系统按1处理。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_as2_cnp_m6x .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|
|Data|Struct|调用成功时，返回的数据。详情请参见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|Total|Integer|驱动数量。|
|PageSize|Integer|返回结果中每页显示的记录数量。|
|CurrentPage|Integer|当前页码。|
|DriverList|List|驱动列表。参数详情请参见下表Driver。|

|名称|类型|描述|
|:-|:-|:-|
|DriverId|String|驱动ID。|
|GmtCreate|String|驱动创建时间。|
|GmtModified|String|驱动最后一次更新时间。|

## 示例 {#section_5t6_1ep_oi6 .section}

请求示例

``` {#codeblock_vu2_ld9_so7}
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryEdgeInstanceDriver
&PageSize=30
&InstanceId=F3APY0tPLhmgGtx0****
&CurrentPage=1
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_rql_qvu_y3i}
    {
      "RequestId": "77F540BC-A0EF-46A4-ABDE-18644C69AAF5",
      "Data": {
        "PageSize": 30,
        "CurrentPage": 1,
        "Total": 1,
        "DriverList": [
          {
            "GmtCreate": "2019-06-26 17:22:25",
            "DriverId": "9c1ae7bd59f1469abbdccc959228****",
            "GmtModified": "2019-06-26 17:22:25"
          }
        ]
      },
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_1x8_3hz_2pl}
    <?xml version="1.0" encoding="UTF-8"?>
    <QueryEdgeInstanceDriverResponse>
      <RequestId>77F540BC-A0EF-46A4-ABDE-18644C69AAF5</RequestId>
      <Data>
        <PageSize>30</PageSize>
        <CurrentPage>1</CurrentPage>
        <Total>1</Total>
        <DriverList>
          <Driver>
            <GmtCreate>2019-06-26 17:22:25</GmtCreate>
            <DriverId>9c1ae7bd59f1469abbdccc959228****</DriverId>
            <GmtModified>2019-06-26 17:22:25</GmtModified>
          </Driver>
        </DriverList>
      </Data>
      <Code>Success</Code>
      <Success>true</Success>
    </QueryEdgeInstanceDriverResponse>
    ```


