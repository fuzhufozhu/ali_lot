# GetLoraNodesTask {#concept_sjz_1p2_zgb .concept}

调用该接口查询批量注册LoRaWAN设备任务的状态。

## 请求参数 {#section_thx_sp2_zgb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：GetLoraNodesTask。|
|TaskId|String|是|注册LoRaWAN设备任务的ID，即调用[SubmitNodesAddingTask](cn.zh-CN/云端开发指南/云端API参考/设备管理/CreateLoRaNodesTask.md#)创建任务后，返回的TaskId。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_azr_kzf_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。|
|TaskId|String|调用成功时，返回的任务ID。|
|TotalCount|Long|任务中待注册设备的总数。|
|SuccessCount|Long|注册成功的数量。|
|FailedCount|Long|注册失败的数量 。|
|SuccessDevEuis|List<String\>|注册成功的设备的DevEUI列表。|
|TaskState|String|任务执行状态。 -   RUNNING：任务正在执行中。
-   FINISH：任务已执行完毕。

 |

## 示例 {#section_dyx_1r2_zgb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=GetLoraNodesTask
&TaskId=62146
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "TotalCount": 2,
      "SuccessCount": 2,
      "RequestId": "D5C7AC10-97A4-42EA-8F92-CDBCCC02DDAC",
      "SuccessDevEuis": {
        "SuccessDevEui": [
          "d896e0ffff01****",
          "d896e0ffff01****"
        ]
      },
      "TaskId": "62146",
      "TaskState": "FINISH",
      "Success": true
    }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <GetLoraNodesTask>
    	<TotalCount>2</TotalCount>
    	<SuccessCount>2</SuccessCount>
    	<RequestId>D5C7AC10-97A4-42EA-8F92-CDBCCC02DDAC</RequestId>
    	<SuccessDevEuis>
    		<SuccessDevEui>d896e0ffff01****</SuccessDevEui>
    		<SuccessDevEui>d896e0ffff01****</SuccessDevEui>
    	</SuccessDevEuis>
    	<TaskId>62146</TaskId>
    	<TaskState>FINISH</TaskState>
    	<Success>true</Success>
    </GetLoraNodesTask>
    ```


