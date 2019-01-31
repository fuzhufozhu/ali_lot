# QueryDevicePropertiesData {#reference_tkf_kmp_1gb .reference}

调用该接口批量查询指定设备的属性上报数据。

## 说明 {#section_hxf_qmp_1gb .section}

-   此接口为高级版产品特有接口。
-   一次调用最多可查询10个属性的历史数据。
-   每个属性最多返回100条数据。

## 请求参数 {#section_npx_tmp_1gb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：QueryDevicePropertiesData。|
|ProductKey|String|是|设备所属的产品Key。|
|DeviceName|String|是|设备名称。|
|Identifiers|List|是|属性的标识符列表。设备的属性Identifier信息，可在控制台中，设备所属的产品的功能定义中查看。不可输入重复的属性Identifier。

|
|StartTime|Long|是|属性记录的开始时间。取值为13位毫秒值时间戳。|
|EndTime|Long|是|属性记录的结束时间。取值为13位毫秒值时间戳。|
|PageSize|Integer|是|单个属性可返回的数据记录数量。最大值为100。任意一个属性返回的数据记录数量不超过该值。

|
|Asc|Integer|是|返回结果中，属性记录按时间排序的方式。取值：-   0：倒序。倒序查询时，StartTime必须大于EndTime。
-   1：正序。正序查询时，StartTime必须小于EndTime。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_lvt_l4p_1gb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|NextValid|Boolean|是否有下一页属性记录。true表示有，false表示没有。返回NextValid为true时，可以将NextTime的值作为下次查询的StartTime，继续查询本次查询不显示的数据。

|
|NextTime|Long|下一页属性记录的起始时间。可以将NextTime的值作为下次查询的StartTime，继续查询本次查询不显示的数据。

|
|Code|String|调用失败时，返回的错误码。请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|PropertyDataInfos|List|调用成功时，返回的属性信息列表。具体信息，请参见下表PropertyDataInfo。|

|名称|类型|描述|
|:-|:-|:-|
|Identifier|String|属性的标识符。|
|List|List|属性数据列表。具体信息，请参见下表PropertyInfo。|

|名称|类型|描述|
|:-|:-|:-|
|Time|Long|属性上报时间。|
|Value|String|属性值。|

## 示例 {#section_dqb_4sp_1gb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDevicePropertiesData
&Asc=0
&DeviceName=water
&EndTime=1540115948152
&Identifier.1=Weight
&Identifier.2=Circle
&PageSize=100
&ProductKey=a1bdOxkDbGC
&StartTime=1540116010723
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
    	"NextValid": true,
    	"NextTime": 1540115949818,
    	"RequestId": "75649FF8-36CD-421B-96C6-35B725FE823B",
    	"Success": true
    	"PropertyDataInfos": {
                 "PropertyDataInfo": [{
    		  "List":{
    			 "PropertyInfo": [{
    				"Value": "166.0",
    				"Time": 1540116010518
    			  },
    			  {
    				"Value": "166.0",
    				"Time": 1540116009906
    			  },
    			  {
    				"Value": "134.0",
    				"Time": 1540115951051
    			  },
    			  {
    				"Value": "133.0",
    				"Time": 1540115950431
    		          },
    			  {
    				"Value": "133.0",
    				"Time": 1540115949819
    			  }
    			]
    		      }, 
    		   "Identifier":"Circle"
        
    		 },
    		 {
    		  "List": {
    		          "PropertyInfo": [{
    				"Value": "99.0",
    				"Time": 1540116010314
    			  },
    			  {
    				"Value": "99.0",
    				"Time": 1540116009702
    			  },
    			  {
    				"Value": "98.0",
    				"Time": 1540116009090
    			  },
    			  {
    				"Value": "51.0",
    				"Time": 1540115950844
    			  },
    			  {
    				"Value": "50.0",
    				"Time": 154011595228
    			   }
    			 ]
    			},
    		   "Identifier": "Weight"
    		 }
    	   ]
         },
    }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <QueryDevicePropertiesData>
    	<NextValid>true</NextValid>
    	<NextTime>1540115949818</NextTime>
    	<RequestId>75649FF8-36CD-421B-96C6-35B725FE823B</RequestId>
    	<PropertyDataInfos>
    		<PropertyDataInfo>
    			<List>
    				<PropertyInfo>
    					<Value>166.0</Value>
    					<Time>1540116010518</Time>
    				</PropertyInfo>
    				<PropertyInfo>
    					<Value>166.0</Value>
    					<Time>1540116009906</Time>
    				</PropertyInfo>
    				<PropertyInfo>
    					<Value>134.0</Value>
    					<Time>1540115951051</Time>
    				</PropertyInfo>
    				<PropertyInfo>
    					<Value>133.0</Value>
    					<Time>1540115950431</Time>
    				</PropertyInfo>
    				<PropertyInfo>
    					<Value>133.0</Value>
    					<Time>1540115949819</Time>
    				</PropertyInfo>
    			</List>
    			<Identifier>Circle</Identifier>
    		</PropertyDataInfo>
    		<PropertyDataInfo>
    			<List>
    				<PropertyInfo>
    					<Value>99.0</Value>
    					<Time>1540116010314</Time>
    				</PropertyInfo>
    				<PropertyInfo>
    					<Value>99.0</Value>
    					<Time>1540116009702</Time>
    				</PropertyInfo>
    				<PropertyInfo>
    					<Value>98.0</Value>
    					<Time>1540116009090</Time>
    				</PropertyInfo>
    				<PropertyInfo>
    					<Value>51.0</Value>
    					<Time>1540115950844</Time>
    				</PropertyInfo>
    				<PropertyInfo>
    					<Value>50.0</Value>
    					<Time>1540115950228</Time>
    				</PropertyInfo>
    			</List>
    			<Identifier>Weight</Identifier>
    		</PropertyDataInfo>
    	</PropertyDataInfos>
    	<Success>true</Success>
    </QueryDevicePropertiesData>
    ```


