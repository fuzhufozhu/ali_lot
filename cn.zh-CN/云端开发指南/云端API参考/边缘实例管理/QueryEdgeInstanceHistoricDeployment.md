# QueryEdgeInstanceHistoricDeployment {#reference_878653 .reference}

调用该接口查询边缘实例历史部署记录。

## 限制说明 {#section_a5c_t2z_y4a .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_km5_f89_f31 .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryEdgeInstanceHistoricDeployment。|
|InstanceId|String|是|边缘实例ID。|
|CurrentPage|Interger|是|从返回结果中的第几页开始显示。最小取值1。如果传入的值小于1，系统按1处理。|
|PageSize|Interger|是|返回结果中每页显示的记录数量。最大取值30；最小取值是10。如果传入的值小于10，系统按10处理。|
|StartTime|Long|否|查询起始时间。 如果不传入起止时间，则查询该实例的全部历史部署记录。

 |
|EndTime|Long|否|查询结束时间。 如果不传入起止时间，则查询该实例的全部历史部署记录。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_0ii_jd3_08j .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|
|Data|Struct|调用成功时，返回的数据。详情请参见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|Total|Integer|总记录数量。|
|PageSize|Integer|返回结果中每页显示的记录数量。|
|CurrentPage|Integer|当前页码。|
|DeploymentList|List|边缘实例列表。参数详情请参见下表Deployment。|

|名称|类型|描述|
|:-|:-|:-|
|GmtCreate|String|部署单创建时间。|
|GmtModified|String|部署单最后一次更新时间。|
|GmtCompleted|String|完成时间。|
|DeploymentId|String|部署单ID。|
|Description|String|描述。|
|Status|Integer|实例的部署单状态。 -   0：未开始（init）。
-   1：正在进行中（processing）。
-   2：成功（success）。
-   3：失败（failure）。

 |
|Type|String|部署单类型： -   deploy：部署。
-   reset：重置。

 |

## 示例 {#section_cv6_2a1_6mv .section}

请求示例

``` {#codeblock_pwu_nst_4x9}
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryEdgeInstanceHistoricDeployment
&PageSize=15
&EndTime=1561543998639
&InstanceId=PgEfYupSn6Pvhfkx****
&CurrentPage=1
&StartTime=1558951998639
&公共参数
```

返回示例

-   JSON格式

    ``` {#codeblock_37c_13y_8ae}
    {
      "RequestId": "C9D9C91B-1B3B-4D84-BE58-68E7B2A989E4",
      "Data": {
        "DeploymentList": [
          {
            "Status": 2,
            "DeploymentId": "e4803e566b424fa68e7f4b1c747c****",
            "GmtCompleted": "2019-06-28 12:07:16",
            "Type": "deploy",
            "GmtCreate": "2019-06-28 12:06:57",
            "Description": "deploy_1561694817061",
            "GmtModified": "2019-06-28 12:07:16"
          },
          {
            "Status": 2,
            "DeploymentId": "9261e308a9504fde9b4cf8462b0b****",
            "GmtCompleted": "2019-06-26 18:12:35",
            "Type": "deploy",
            "GmtCreate": "2019-06-26 18:12:29",
            "Description": "deploy_1561543948874",
            "GmtModified": "2019-06-26 18:12:35"
          }
        ],
        "PageSize": 2,
        "CurrentPage": 1,
        "Total": 6
      },
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_lcf_wjg_5jt}
    <?xml version="1.0" encoding="UTF-8"?>
    <QueryEdgeInstanceHistoricDeploymentResponse>
      <RequestId>C9D9C91B-1B3B-4D84-BE58-68E7B2A989E4</RequestId>
      <Data>
        <DeploymentList>
          <Deployment>
            <Status>2</Status>
            <DeploymentId>e4803e566b424fa68e7f4b1c747c****</DeploymentId>
            <GmtCompleted>2019-06-28 12:07:16</GmtCompleted>
            <Type>deploy</Type>
            <GmtCreate>2019-06-28 12:06:57</GmtCreate>
            <Description>deploy_1561694817061</Description>
            <GmtModified>2019-06-28 12:07:16</GmtModified>
          </Deployment>
          <Deployment>
            <Status>2</Status>
            <DeploymentId>9261e308a9504fde9b4cf8462b0b****</DeploymentId>
            <GmtCompleted>2019-06-26 18:12:35</GmtCompleted>
            <Type>deploy</Type>
            <GmtCreate>2019-06-26 18:12:29</GmtCreate>
            <Description>deploy_1561543948874</Description>
            <GmtModified>2019-06-26 18:12:35</GmtModified>
          </Deployment>
        </DeploymentList>
        <PageSize>2</PageSize>
        <CurrentPage>1</CurrentPage>
        <Total>6</Total>
      </Data>
      <Code>Success</Code>
      <Success>true</Success>
    </QueryEdgeInstanceHistoricDeploymentResponse>
    ```


