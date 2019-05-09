# QueryLoRaJoinPermissions {#reference_pjj_z12_zgb .reference}

调用该接口查询LoRaWAN入网凭证列表。

## 请求参数 {#section_wrw_422_zgb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryLoRaJoinPermissions。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_kbx_bf2_zgb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。|
|JoinPermissions|List<JoinPermission\>|调用成功时，返回的入网凭证数据。详情见下表JoinPermission。|

|名称|类型|描述|
|:-|:-|:-|
|JoinPermissionId|String|入网凭证ID，入网凭证的唯一标识。|
|OwnerAliyunPk|String|入网凭证创建者的阿里云账号ID。|
|JoinPermissionName|String|入网凭证名称。|
|FreqBandPlanGroupId|String|入网凭证采用的频谱计划ID。取值： -   CN470：同频
-   CN470：异频
-   AS923：同频

 |
|ClassMode|String|入网凭证采用的通信模式。取值： -   A：终端设备允许双向通信。
-   B：终端设备会在预设时间中开放接收窗口。
-   C：终端设备持续开放接收窗口，只在传输时关闭。

 |
|JoinPermissionType|String|入网凭证的类型。取值： -   LOCAL：专用凭证。
-   ROAMING：漫游凭证。

 |
|Enabled|Boolean|入网凭证的启停用状态。取值： -   true：启用
-   false：停用

 |

## 示例 {#section_vcn_jh2_zgb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryLoRaJoinPermissions
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId": "A6B85BE6-D199-4634-AC24-AD08CD289D9B",
      "Success": true,
      "JoinPermissions": {
        "JoinPermission": [
          {
            "Enabled": true,
            "JoinPermissionType": "LOCAL",
            "JoinPermissionId": "8***",
            "OwnerAliyunPk": "1375364789****",
            "ClassMode": "B",
            "JoinPermissionName": "ForTest"
          }
        ]
      }
    }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <QueryLoRaJoinPermissions>
    	<RequestId>A6B85BE6-D199-4634-AC24-AD08CD289D9B</RequestId>
    	<Success>true</Success>
    	<JoinPermissions>
    		<JoinPermission>
    			<Enabled>true</Enabled>
    			<JoinPermissionType>LOCAL</JoinPermissionType>
    			<JoinPermissionId>8***</JoinPermissionId>
    			<OwnerAliyunPk>1375364789****</OwnerAliyunPk>
    			<ClassMode>B</ClassMode>
    			<JoinPermissionName>ForTest</JoinPermissionName>
    		</JoinPermission>
    	</JoinPermissions>
    </QueryLoRaJoinPermissions>
    ```


