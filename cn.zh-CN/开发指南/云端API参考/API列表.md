# API列表 {#reference_kd4_l4z_wdb .reference}

以下是物联网平台 API 列表。

## 产品管理相关 API {#section_sqj_czc_xdb .section}

|API|描述|
|:--|:-|
|[CreateProduct](intl.zh-CN/开发指南/云端API参考/产品管理/CreateProduct.md#)|创建产品。|
|[UpdateProduct](intl.zh-CN/开发指南/云端API参考/产品管理/UpdateProduct.md#)|修改产品信息。|
|[QueryProductList](intl.zh-CN/开发指南/云端API参考/产品管理/QueryProductList.md#)|查询产品列表。|
|[QueryProduct](intl.zh-CN/开发指南/云端API参考/产品管理/QueryProduct.md#)|查询产品详细信息。|

## 设备管理相关 API {#section_dcj_qzc_xdb .section}

|API|描述|
|:--|:-|
|[RegisterDevice](intl.zh-CN/开发指南/云端API参考/设备管理/RegisterDevice.md#)|注册设备。|
|[QueryDeviceDetail](intl.zh-CN/开发指南/云端API参考/设备管理/QueryDeviceDetail.md#)|查询设备详情。|
|[QueryDevice](intl.zh-CN/开发指南/云端API参考/设备管理/QueryDevice.md#)|查询产品的设备列表。|
|[DeleteDevice](intl.zh-CN/开发指南/云端API参考/设备管理/DeleteDevice.md#)|删除设备。|
|[GetDeviceStatus](intl.zh-CN/开发指南/云端API参考/设备管理/GetDeviceStatus.md#)|获取设备的运行状态。|
|[BatchGetDeviceState](intl.zh-CN/开发指南/云端API参考/设备管理/BatchGetDeviceState.md#)|批量获取设备状态。|
|[DisableThing](intl.zh-CN/开发指南/云端API参考/设备管理/DisableThing.md#)|禁用设备。|
|[EnableThing](intl.zh-CN/开发指南/云端API参考/设备管理/EnableThing.md#)|解禁设备。|
|[BatchCheckDeviceNames](intl.zh-CN/开发指南/云端API参考/设备管理/BatchCheckDeviceNames.md#)|批量检查设备名称。|
|[BatchRegisterDeviceWithApplyId](intl.zh-CN/开发指南/云端API参考/设备管理/BatchRegisterDeviceWithApplyId.md#)|根据 ApplyId 批量申请设备。|
|[BatchRegisterDevice](intl.zh-CN/开发指南/云端API参考/设备管理/BatchRegisterDevice.md#)|批次申请特定数量设备。|
|[QueryBatchRegisterDeviceStatus](intl.zh-CN/开发指南/云端API参考/设备管理/QueryBatchRegisterDeviceStatus.md#)|查询批量注册设备状态。|
|[QueryPageByApplyId](intl.zh-CN/开发指南/云端API参考/设备管理/QueryPageByApplyId.md#)|查询批次设备列表。|
|[QueryDeviceEventData](intl.zh-CN/开发指南/云端API参考/设备管理/QueryDeviceEventData.md#)|获取设备的事件历史数据。|
|[QueryDevicePropertyData](intl.zh-CN/开发指南/云端API参考/设备管理/QueryDevicePropertyData.md#)|获取设备的属性历史数据。|
|[QueryDeviceServiceData](intl.zh-CN/开发指南/云端API参考/设备管理/QueryDeviceServiceData.md#)|获取设备的服务记录历史数据。|
|[InvokeThingService](intl.zh-CN/开发指南/云端API参考/设备管理/InvokeThingService.md#)|调用设备的服务。|
|[QueryDevicePropertyStatus](intl.zh-CN/开发指南/云端API参考/设备管理/QueryDevicePropertyStatus.md#)|查询设备的属性快照。|
|[SetDeviceProperty](intl.zh-CN/开发指南/云端API参考/设备管理/SetDeviceProperty.md#)|设置设备的属性。|
|[SaveDeviceProp](intl.zh-CN/开发指南/云端API参考/设备管理/SaveDeviceProp.md#)|设置设备标签。|
|[QueryDeviceProp](intl.zh-CN/开发指南/云端API参考/设备管理/QueryDeviceProp.md#)|查询设备标签列表。|
|[DeleteDeviceProp](intl.zh-CN/开发指南/云端API参考/设备管理/DeleteDeviceProp.md#)|删除设备标签。|
|[GetThingTopo](intl.zh-CN/开发指南/云端API参考/设备管理/GetThingTopo.md#)|查询网关设备或子设备所具有的拓扑关系。|
|[NotifyAddThingTopo](intl.zh-CN/开发指南/云端API参考/设备管理/NotifyAddThingTopo.md#)|通知云端增加设备拓扑关系。|
|[RemoveThingTopo](intl.zh-CN/开发指南/云端API参考/设备管理/RemoveThingTopo.md#)|移除网关设备或子设备所具有的拓扑关系。|
|[QueryDeviceStatistics](intl.zh-CN/开发指南/云端API参考/设备管理/QueryDeviceStatistics.md#)|获取设备的统计数量。|

## 规则引擎相关 API {#section_enr_vbd_xdb .section}

|API|描述|
|:--|:-|
|[ListRule](intl.zh-CN/开发指南/云端API参考/规则引擎/ListRule.md#)|查询规则列表。|
|[CreateRule](intl.zh-CN/开发指南/云端API参考/规则引擎/CreateRule.md#)|创建规则。|
|[GetRule](intl.zh-CN/开发指南/云端API参考/规则引擎/GetRule.md#)|查询规则信息。|
|[UpdateRule](intl.zh-CN/开发指南/云端API参考/规则引擎/UpdateRule.md#)|修改规则。|
|[DeleteRule](intl.zh-CN/开发指南/云端API参考/规则引擎/DeleteRule.md#)|删除规则。|
|[ListRuleActions](intl.zh-CN/开发指南/云端API参考/规则引擎/ListRuleActions.md#)|查询规则动作列表。|
|[GetRuleAction](intl.zh-CN/开发指南/云端API参考/规则引擎/GetRuleAction.md#)|查询规则动作信息。|
|[CreateRuleAction](intl.zh-CN/开发指南/云端API参考/规则引擎/CreateRuleAction.md#)|创建规则动作。|
|[UpdateRuleAction](intl.zh-CN/开发指南/云端API参考/规则引擎/UpdateRuleAction.md#)|更新规则动作。|
|[DeleteRuleAction](intl.zh-CN/开发指南/云端API参考/规则引擎/DeleteRuleAction.md#)|删除规则动作。|
|[StartRule](intl.zh-CN/开发指南/云端API参考/规则引擎/StartRule.md#)|启动规则。|
|[StopRule](intl.zh-CN/开发指南/云端API参考/规则引擎/StopRule.md#)|停止规则。|

## Topic 管理相关 API {#section_bqz_v42_xdb .section}

|API|描述|
|:--|:-|
|[QueryProductTopic](intl.zh-CN/开发指南/云端API参考/消息通信/QueryProductTopic.md#)|查询产品Topic类。|
|[CreateProductTopic](intl.zh-CN/开发指南/云端API参考/消息通信/CreateProductTopic.md#)|创建产品Topic类。|
|[UpdateProductTopic](intl.zh-CN/开发指南/云端API参考/消息通信/UpdateProductTopic.md#)|修改产品Topic类。|
|[DeleteProductTopic](intl.zh-CN/开发指南/云端API参考/消息通信/DeleteProductTopic.md#)|删除产品Topic类。|
|[CreateTopicRouteTable](intl.zh-CN/开发指南/云端API参考/消息通信/CreateTopicRouteTable.md#)|添加Topic路由表。|
|[QueryTopicRouteTable](intl.zh-CN/开发指南/云端API参考/消息通信/QueryTopicRouteTable.md#)|查询Topic路由表。|
|[QueryTopicReverseRouteTable](intl.zh-CN/开发指南/云端API参考/消息通信/QueryTopicReverseRouteTable.md#)|查询Topic反向路由表。|
|[DeleteTopicRouteTable](intl.zh-CN/开发指南/云端API参考/消息通信/DeleteTopicRouteTable.md#)|删除Topic路由表。|

## 通信相关 API {#section_oyc_jp2_xdb .section}

|API|描述|
|:--|:-|
|[Pub](intl.zh-CN/开发指南/云端API参考/通信相关/Pub.md#)|发布消息到Topic。|
|[RRpc](intl.zh-CN/开发指南/云端API参考/通信相关/RRpc.md#)|发消息给设备，并同步返回响应。|
|[PubBroadcast](intl.zh-CN/开发指南/云端API参考/通信相关/PubBroadcast.md#)|发布广播消息。|

## 设备影子相关 API {#section_ptd_vp2_xdb .section}

|API|描述|
|:--|:-|
|[GetDeviceShadow](intl.zh-CN/开发指南/云端API参考/设备影子/GetDeviceShadow.md#)|查询设备影子。|
|[UpdateDeviceShadow](intl.zh-CN/开发指南/云端API参考/设备影子/UpdateDeviceShadow.md#)|更新设备影子。|

