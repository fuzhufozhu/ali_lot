# QueryEdgeInstance {#reference_878617 .reference}

调用该接口查询当前账号下的所有边缘实例。

## 限制说明 {#section_844_rnh_ctg .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_v38_qbx_pyd .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryEdgeInstance。|
|PageSize|Integer|是|返回结果中每页显示的记录数量。最大取值30；最小取值是10。如果传入的值小于10，系统按10处理。|
|CurrentPage|Integer|是|从返回结果中的第几页开始显示。最小取值 1。如果传入的值小于1，系统按1处理。|
|Name|String|否|边缘实例名称。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_ghn_qqj_3fv .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|
|Data|Struct|调用成功时，返回的数据。详情请参见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|Total|Integer|实例数量。|
|PageSize|Integer|返回结果中每页显示的记录数量。|
|CurrentPage|Integer|当前页码。|
|InstanceList|List|边缘实例列表。参数详情请参见下表Instance。|

|参数|类型|描述|
|:-|:-|:-|
|InstanceId|String|边缘实例ID。|
|Name|String|边缘实例名称。|
|Tags|String|边缘实例标签。|
|LatestDeploymentStatus|Integer|实例最近一次部署单状态。 -   0：未开始（init）。
-   1：正在进行（processing）。
-   2：成功（success）。
-   3：失败（failure）。

 |
|LatestDeploymentType|String|实例最近一次部署单类型： -   deploy：部署。
-   reset：重置。

 |
|GmtCreate|String|实例创建时间。|
|GmtModified|String|实例最后一次更新时间。|
|RoleArn|String|授权角色的全局资源描述符。|
|RoleName|String|授权角色名称。|
|RoleAttachTime|String|角色绑定时间。|
|Spec|Integer|产品规格。 -   10：轻量版。
-   20：标准版。
-   30：专业版。

 |
|BizEnable|Boolean|边缘实例是否开启。取值： -   true：开启。
-   false：关闭。

 |

## 示例 {#section_z6e_nk9_jvs .section}

请求示例

``` {#codeblock_9yn_34w_rls}
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryEdgeInstance
&PageSize=15
&CurrentPage=1
&公共参数
```

返回示例

-   JSON格式

    ``` {#codeblock_hzx_gvg_t5g}
    {
      "RequestId": "199BBC5D-FC99-46CB-88E2-FB98E92446FA",
      "Data": {
        "PageSize": 2,
        "CurrentPage": 1,
        "Total": 201,
        "InstanceList": [
          {
            "GmtCreate": "2019-07-17 14:34:28",
            "BizEnable": true,
            "InstanceId": "9iqyQAKDb2aYGVKa****",
            "GmtModified": "2019-07-17 14:51:38",
            "Spec": 20,
            "Name": "test_le1"
          },
          {
            "GmtCreate": "2018-10-22 20:09:29",
            "BizEnable": true,
            "InstanceId": "ZpbCxkQpbb2kpmm0****",
            "LatestDeploymentType": "reset",
            "GmtModified": "2019-07-16 16:50:50",
            "Spec": 30,
            "Name": "test_le1",
            "LatestDeploymentStatus": 1
          }
        ]
      },
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_7w6_yss_alg}
    <?xml version="1.0" encoding="UTF-8"?>
    <QueryEdgeInstanceResponse>
      <RequestId>199BBC5D-FC99-46CB-88E2-FB98E92446FA</RequestId>
      <Data>
        <PageSize>2</PageSize>
        <CurrentPage>1</CurrentPage>
        <Total>201</Total>
        <InstanceList>
          <Instance>
            <GmtCreate>2019-07-17 14:34:28</GmtCreate>
            <BizEnable>true</BizEnable>
            <InstanceId>9iqyQAKDb2aYGVKaDBF4</InstanceId>
            <GmtModified>2019-07-17 14:51:38</GmtModified>
            <Spec>20</Spec>
            <Name>test_le1</Name>
          </Instance>
          <Instance>
            <GmtCreate>2018-10-22 20:09:29</GmtCreate>
            <BizEnable>true</BizEnable>
            <InstanceId>ZpbCxkQpbb2kpmm0DBF4</InstanceId>
            <LatestDeploymentType>reset</LatestDeploymentType>
            <GmtModified>2019-07-16 16:50:50</GmtModified>
            <Spec>30</Spec>
            <Name>test_le1</Name>
            <LatestDeploymentStatus>1</LatestDeploymentStatus>
          </Instance>
        </InstanceList>
      </Data>
      <Code>Success</Code>
      <Success>true</Success>
    </QueryEdgeInstanceResponse>
    ```


