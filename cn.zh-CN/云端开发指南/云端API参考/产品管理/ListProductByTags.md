# ListProductByTags {#reference_mqk_txk_3gb .reference}

调用该接口根据标签分页查询产品列表。

## 请求参数 {#section_uzk_rdl_3gb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值ListProductByTags。|
|ProductTags|List<String\>|是|产品标签。ProductTag包括TagKey和TagValue，分别对应标签的key和value。请参见下表[ProductTag](#)。-   支持按照TagKey和TagValue组合来搜索。
-   支持只按照TagKey来搜索。
-   传入多个ProductTag是或的关系。

|
|Page​Size|Integer​|否|指定返回结果中每页显示的记录数量。最大值是50。默认值是10。|
|CurrentPage|Integer|否|指定显示返回结果中的第几页。默认值为1。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|TagKey|String|是|产品标签键\(key\)。|
|TagValue|String|否|产品标签值\(value\)。|

## 返回参数 {#section_ibl_sfl_3gb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|ProductInfos|List<String\>|调用成功时，返回产品信息列表。详情见下表[ProductInfo](#)|

|名称|类型|描述|
|:-|:-|:-|
|ProductKey|String|产品Key。|
|ProductName|String|产品名称。|
|NodeType|Integer|节点类型。|
|CreateTime|Long|产品创建时间。|
|Description|String|产品描述。|

## 示例 {#section_ufv_p3l_3gb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/&Action=ListProductByTags
&CurrentPage=1
&PageSize=10
&ProductTag.1.TagKey=Reen
&ProductTag.1.TagValue=reen
&ProductTag.2.TagKey=Lock
&ProductTag.2.TagValue=1234
&公共请求参数
```

**返回示例**

JSON格式

```
{
  "ProductInfos": {
    "ProductInfo": [
      {
        "Description": "Bulbs in the rooms",
        "ProductKey": "a1h7knJdld1",
        "NodeType": 0,
        "CreateTime": 1545355537000,
        "ProductB: "Bulbs"
      }
    ]
  },
  "RequestId": "2E410BE3-C688-487B-9BF1-F04B33632CCC",
  "Success": true
}
```

XML格式

```
<ListProductByTagsResponse>
    <ProductInfos>
		<ProductInfo>
			<Description>Bulbs in the rooms</Description>
			<ProductKey>a1h7knJdld1</ProductKey>
			<NodeType>0</NodeType>
			<CreateTime>1545355537000</CreateTime>
			<ProductName>Bulbs</ProductName>
		</ProductInfo>
	</ProductInfos>
	<RequestId>2E410BE3-C688-487B-9BF1-F04B33632CCC</RequestId>
	<Success>true</Success>
</ListProductByTagsResponse>
```

