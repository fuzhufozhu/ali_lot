# API列表 {#reference_kd4_l4z_wdb .reference}

以下是物联网平台 API 列表。

## 产品管理相关 API {#section_sqj_czc_xdb .section}

|API|描述|
|:--|:-|
|[CreateProduct](cn.zh-CN/云端开发指南/云端API参考/产品管理/CreateProduct.md#)|创建产品。|
|[UpdateProduct](cn.zh-CN/云端开发指南/云端API参考/产品管理/UpdateProduct.md#)|修改产品信息。|
|[QueryProductList](cn.zh-CN/云端开发指南/云端API参考/产品管理/QueryProductList.md#)|查询产品列表。|
|[QueryProduct](cn.zh-CN/云端开发指南/云端API参考/产品管理/QueryProduct.md#)|查询产品详细信息。|

## 设备管理相关 API {#section_dcj_qzc_xdb .section}

|API|描述|
|:--|:-|
|[RegisterDevice](cn.zh-CN/云端开发指南/云端API参考/设备管理/RegisterDevice.md#)|注册设备。|
|[QueryDeviceDetail](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceDetail.md#)|查询设备详情。|
|[QueryDevice](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevice.md#)|查询产品的设备列表。|
|[DeleteDevice](cn.zh-CN/云端开发指南/云端API参考/设备管理/DeleteDevice.md#)|删除设备。|
|[GetDeviceStatus](cn.zh-CN/云端开发指南/云端API参考/设备管理/GetDeviceStatus.md#)|获取设备的运行状态。|
|[BatchGetDeviceState](cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchGetDeviceState.md#)|批量获取设备状态。|
|[DisableThing](cn.zh-CN/云端开发指南/云端API参考/设备管理/DisableThing.md#)|禁用设备。|
|[EnableThing](cn.zh-CN/云端开发指南/云端API参考/设备管理/EnableThing.md#)|解禁设备。|
|[BatchCheckDeviceNames](cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchCheckDeviceNames.md#)|批量检查设备名称。|
|[BatchRegisterDeviceWithApplyId](cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchRegisterDeviceWithApplyId.md#)|根据 ApplyId 批量申请设备。|
|[BatchRegisterDevice](cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchRegisterDevice.md#)|批次申请特定数量设备。|
|[QueryBatchRegisterDeviceStatus](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryBatchRegisterDeviceStatus.md#)|查询批量注册设备状态。|
|[QueryPageByApplyId](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryPageByApplyId.md#)|查询批次设备列表。|
|[QueryDeviceEventData](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceEventData.md#)|获取设备的事件历史数据。|
|[QueryDevicePropertyData](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevicePropertyData.md#)|获取设备的属性历史数据。|
|[QueryDeviceServiceData](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceServiceData.md#)|获取设备的服务记录历史数据。|
|[InvokeThingService](cn.zh-CN/云端开发指南/云端API参考/设备管理/InvokeThingService.md#)|调用设备的服务。|
|[QueryDevicePropertyStatus](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevicePropertyStatus.md#)|查询设备的属性快照。|
|[SetDeviceProperty](cn.zh-CN/云端开发指南/云端API参考/设备管理/SetDeviceProperty.md#)|设置设备的属性。|
|[SaveDeviceProp](cn.zh-CN/云端开发指南/云端API参考/设备管理/SaveDeviceProp.md#)|设置设备标签。|
|[QueryDeviceProp](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceProp.md#)|查询设备标签列表。|
|[DeleteDeviceProp](cn.zh-CN/云端开发指南/云端API参考/设备管理/DeleteDeviceProp.md#)|删除设备标签。|
|[GetThingTopo](cn.zh-CN/云端开发指南/云端API参考/设备管理/GetThingTopo.md#)|查询网关设备或子设备所具有的拓扑关系。|
|[NotifyAddThingTopo](cn.zh-CN/云端开发指南/云端API参考/设备管理/NotifyAddThingTopo.md#)|通知云端增加设备拓扑关系。|
|[RemoveThingTopo](cn.zh-CN/云端开发指南/云端API参考/设备管理/RemoveThingTopo.md#)|移除网关设备或子设备所具有的拓扑关系。|
|[QueryDeviceStatistics](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceStatistics.md#)|获取设备的统计数量。|
|[GetGatewayBySubDevice](cn.zh-CN/云端开发指南/云端API参考/设备管理/GetGatewayBySubDevice.md#)|根据挂载的子设备信息查询对应的网关设备信息。|

## 规则引擎相关 API {#section_enr_vbd_xdb .section}

|API|描述|
|:--|:-|
|[ListRule](cn.zh-CN/云端开发指南/云端API参考/规则引擎/ListRule.md#)|查询规则列表。|
|[CreateRule](cn.zh-CN/云端开发指南/云端API参考/规则引擎/CreateRule.md#)|创建规则。|
|[GetRule](cn.zh-CN/云端开发指南/云端API参考/规则引擎/GetRule.md#)|查询规则信息。|
|[UpdateRule](cn.zh-CN/云端开发指南/云端API参考/规则引擎/UpdateRule.md#)|修改规则。|
|[DeleteRule](cn.zh-CN/云端开发指南/云端API参考/规则引擎/DeleteRule.md#)|删除规则。|
|[ListRuleActions](cn.zh-CN/云端开发指南/云端API参考/规则引擎/ListRuleActions.md#)|查询规则动作列表。|
|[GetRuleAction](cn.zh-CN/云端开发指南/云端API参考/规则引擎/GetRuleAction.md#)|查询规则动作信息。|
|[CreateRuleAction](cn.zh-CN/云端开发指南/云端API参考/规则引擎/CreateRuleAction.md#)|创建规则动作。|
|[UpdateRuleAction](cn.zh-CN/云端开发指南/云端API参考/规则引擎/UpdateRuleAction.md#)|更新规则动作。|
|[DeleteRuleAction](cn.zh-CN/云端开发指南/云端API参考/规则引擎/DeleteRuleAction.md#)|删除规则动作。|
|[StartRule](cn.zh-CN/云端开发指南/云端API参考/规则引擎/StartRule.md#)|启动规则。|
|[StopRule](cn.zh-CN/云端开发指南/云端API参考/规则引擎/StopRule.md#)|停止规则。|

## Topic 管理相关 API {#section_bqz_v42_xdb .section}

|API|描述|
|:--|:-|
|[QueryProductTopic](cn.zh-CN/云端开发指南/云端API参考/Topic管理/QueryProductTopic.md#)|查询产品Topic类。|
|[CreateProductTopic](cn.zh-CN/云端开发指南/云端API参考/Topic管理/CreateProductTopic.md#)|创建产品Topic类。|
|[UpdateProductTopic](cn.zh-CN/云端开发指南/云端API参考/Topic管理/UpdateProductTopic.md#)|修改产品Topic类。|
|[DeleteProductTopic](cn.zh-CN/云端开发指南/云端API参考/Topic管理/DeleteProductTopic.md#)|删除产品Topic类。|
|[CreateTopicRouteTable](cn.zh-CN/云端开发指南/云端API参考/Topic管理/CreateTopicRouteTable.md#)|添加Topic路由表。|
|[QueryTopicRouteTable](cn.zh-CN/云端开发指南/云端API参考/Topic管理/QueryTopicRouteTable.md#)|查询Topic路由表。|
|[QueryTopicReverseRouteTable](cn.zh-CN/云端开发指南/云端API参考/Topic管理/QueryTopicReverseRouteTable.md#)|查询Topic反向路由表。|
|[DeleteTopicRouteTable](cn.zh-CN/云端开发指南/云端API参考/Topic管理/DeleteTopicRouteTable.md#)|删除Topic路由表。|

## 消息通信相关 API {#section_oyc_jp2_xdb .section}

|API|描述|
|:--|:-|
|[Pub](cn.zh-CN/云端开发指南/云端API参考/消息通信/Pub.md#)|发布消息到Topic。|
|[RRpc](cn.zh-CN/云端开发指南/云端API参考/消息通信/RRpc.md#)|发消息给设备，并同步返回响应。|
|[PubBroadcast](cn.zh-CN/云端开发指南/云端API参考/消息通信/PubBroadcast.md#)|发布广播消息。|

## 设备影子相关 API {#section_ptd_vp2_xdb .section}

|API|描述|
|:--|:-|
|[GetDeviceShadow](cn.zh-CN/云端开发指南/云端API参考/设备影子/GetDeviceShadow.md#)|查询设备影子。|
|[UpdateDeviceShadow](cn.zh-CN/云端开发指南/云端API参考/设备影子/UpdateDeviceShadow.md#)|更新设备影子。|

