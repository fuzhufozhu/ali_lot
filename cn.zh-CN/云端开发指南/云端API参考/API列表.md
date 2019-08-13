# API列表 {#reference_kd4_l4z_wdb .reference}

以下是物联网平台 API 列表。

## 产品管理相关 API {#section_sqj_czc_xdb .section}

|API|描述|
|:--|:-|
|[CreateProduct](intl.zh-CN/云端开发指南/云端API参考/产品管理/CreateProduct.md#)|创建产品。|
|[UpdateProduct](intl.zh-CN/云端开发指南/云端API参考/产品管理/UpdateProduct.md#)|修改产品信息。|
|[QueryProductList](intl.zh-CN/云端开发指南/云端API参考/产品管理/QueryProductList.md#)|查询产品列表。|
|[QueryProduct](intl.zh-CN/云端开发指南/云端API参考/产品管理/QueryProduct.md#)|查询产品详细信息。|
|[DeleteProduct](intl.zh-CN/云端开发指南/云端API参考/产品管理/DeleteProduct.md#)|删除指定产品。|
|[CreateProductTags](intl.zh-CN/云端开发指南/云端API参考/产品管理/CreateProductTags.md#)|创建产品标签。|
|[UpdateProductTags](intl.zh-CN/云端开发指南/云端API参考/产品管理/UpdateProductTags.md#)|更新产品标签。|
|[DeleteProductTags](intl.zh-CN/云端开发指南/云端API参考/产品管理/DeleteProductTags.md#)|删除产品标签。|
|[ListProductTags](intl.zh-CN/云端开发指南/云端API参考/产品管理/ListProductTags.md#)|查询产品的所有标签。|
|[ListProductByTags](intl.zh-CN/云端开发指南/云端API参考/产品管理/ListProductByTags.md#)|根据标签查询产品。|

## 设备管理相关 API {#section_dcj_qzc_xdb .section}

|API|描述|
|:--|:-|
|[RegisterDevice](intl.zh-CN/云端开发指南/云端API参考/设备管理/RegisterDevice.md#)|注册设备。|
|[QueryDeviceDetail](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceDetail.md#)|查询设备详情。|
|[BatchQueryDeviceDetail](intl.zh-CN/云端开发指南/云端API参考/设备管理/BatchQueryDeviceDetail.md#)|批量查询设备详情。|
|[QueryDevice](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevice.md#)|查询产品的设备列表。|
|[DeleteDevice](intl.zh-CN/云端开发指南/云端API参考/设备管理/DeleteDevice.md#)|删除设备。|
|[GetDeviceStatus](intl.zh-CN/云端开发指南/云端API参考/设备管理/GetDeviceStatus.md#)|获取设备的运行状态。|
|[BatchGetDeviceState](intl.zh-CN/云端开发指南/云端API参考/设备管理/BatchGetDeviceState.md#)|批量获取设备状态。|
|[DisableThing](intl.zh-CN/云端开发指南/云端API参考/设备管理/DisableThing.md#)|禁用设备。|
|[EnableThing](intl.zh-CN/云端开发指南/云端API参考/设备管理/EnableThing.md#)|解禁设备。|
|[BatchCheckDeviceNames](intl.zh-CN/云端开发指南/云端API参考/设备管理/BatchCheckDeviceNames.md#)|批量检查设备名称。|
|[BatchRegisterDeviceWithApplyId](intl.zh-CN/云端开发指南/云端API参考/设备管理/BatchRegisterDeviceWithApplyId.md#)|根据 ApplyId 批量申请设备。|
|[BatchRegisterDevice](intl.zh-CN/云端开发指南/云端API参考/设备管理/BatchRegisterDevice.md#)|批次申请特定数量设备。|
|[QueryBatchRegisterDeviceStatus](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryBatchRegisterDeviceStatus.md#)|查询批量注册设备状态。|
|[QueryPageByApplyId](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryPageByApplyId.md#)|查询批次设备列表。|
|[QueryDeviceEventData](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceEventData.md#)|查询设备的事件历史数据。|
|[QueryDevicePropertyData](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevicePropertyData.md#)|查询设备的属性历史数据。|
|[QueryDevicePropertiesData](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevicePropertiesData.md#)|批量查询指定设备的多个属性的历史数据。|
|[QueryDeviceServiceData](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceServiceData.md#)|获取设备的服务记录历史数据。|
|[InvokeThingService](intl.zh-CN/云端开发指南/云端API参考/设备管理/InvokeThingService.md#)|调用设备的服务。|
|[InvokeThingsService](intl.zh-CN/云端开发指南/云端API参考/设备管理/InvokeThingsService.md#)|批量调用设备的服务。|
|[QueryDevicePropertyStatus](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevicePropertyStatus.md#)|查询设备的属性快照。|
|[SetDeviceProperty](intl.zh-CN/云端开发指南/云端API参考/设备管理/SetDeviceProperty.md#)|设置设备的属性。|
|[SetDevicesProperty](intl.zh-CN/云端开发指南/云端API参考/设备管理/SetDevicesProperty.md#)|批量设置设备属性。|
|[SaveDeviceProp](intl.zh-CN/云端开发指南/云端API参考/设备管理/SaveDeviceProp.md#)|设置设备标签。|
|[QueryDeviceProp](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceProp.md#)|查询设备标签列表。|
|[DeleteDeviceProp](intl.zh-CN/云端开发指南/云端API参考/设备管理/DeleteDeviceProp.md#)|删除设备标签。|
|[GetThingTopo](intl.zh-CN/云端开发指南/云端API参考/设备管理/GetThingTopo.md#)|查询网关设备或子设备所具有的拓扑关系。|
|[NotifyAddThingTopo](intl.zh-CN/云端开发指南/云端API参考/设备管理/NotifyAddThingTopo.md#)|通知网关增加设备拓扑关系。|
|[RemoveThingTopo](intl.zh-CN/云端开发指南/云端API参考/设备管理/RemoveThingTopo.md#)|移除网关设备或子设备所具有的拓扑关系。|
|[QueryDeviceStatistics](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceStatistics.md#)|获取设备的统计数量。|
|[GetGatewayBySubDevice](intl.zh-CN/云端开发指南/云端API参考/设备管理/GetGatewayBySubDevice.md#)|根据挂载的子设备信息查询对应的网关设备信息。|
|[QueryDeviceByTags](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceByTags.md#)|根据标签查询设备。|
|[SetDeviceDesiredProperty](intl.zh-CN/云端开发指南/云端API参考/设备管理/SetDeviceDesiredProperty.md#)|为指定设备批量设置期望属性值。|
|[QueryDeviceDesiredProperty](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceDesiredProperty.md#)|查询指定设备的期望属性值。|
|[QueryDeviceFileList](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceFileList.md#)|查询指定设备上传到物联网平台的所有文件。|
|[QueryDeviceFile](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceFile.md#)|查询指定设备上传到物联网平台的指定文件信息。|
|[DeleteDeviceFile](intl.zh-CN/云端开发指南/云端API参考/设备管理/DeleteDeviceFile.md#)|删除指定设备上传到物联网平台的指定文件。|
|[BatchUpdateDeviceNickname](intl.zh-CN/云端开发指南/云端API参考/设备管理/BatchUpdateDeviceNickname.md#)|批量更新设备备注名称。|

## 分组管理相关API {#section_atx_lcg_mfb .section}

|API|描述|
|:--|:-|
|[CreateDeviceGroup](intl.zh-CN/云端开发指南/云端API参考/分组管理/CreateDeviceGroup.md#)|创建分组。|
|[DeleteDeviceGroup](intl.zh-CN/云端开发指南/云端API参考/分组管理/DeleteDeviceGroup.md#)|删除分组。|
|[UpdateDeviceGroup](intl.zh-CN/云端开发指南/云端API参考/分组管理/UpdateDeviceGroup.md#)|修改分组信息。|
|[QueryDeviceGroupInfo](intl.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupInfo.md#)|查询分组详情。|
|[QueryDeviceGroupList](intl.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupList.md#)|分页查询分组列表。|
|[BatchAddDeviceGroupRelations](intl.zh-CN/云端开发指南/云端API参考/分组管理/BatchAddDeviceGroupRelations.md#)|添加设备到分组。|
|[BatchDeleteDeviceGroupRelations](intl.zh-CN/云端开发指南/云端API参考/分组管理/BatchDeleteDeviceGroupRelations.md#)|删除分组中已添加的指定设备。|
|[SetDeviceGroupTags](intl.zh-CN/云端开发指南/云端API参考/分组管理/SetDeviceGroupTags.md#)|添加或更新分组标签。|
|[QueryDeviceGroupTagList](intl.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupTagList.md#)|查询分组标签列表。|
|[QueryDeviceGroupByDevice](intl.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupByDevice.md#)|查询指定设备所在的分组列表。|
|[QuerySuperDeviceGroup](intl.zh-CN/云端开发指南/云端API参考/分组管理/QuerySuperDeviceGroup.md#)|根据子分组ID查询父分组信息。|
|[QueryDeviceListByDeviceGroup](intl.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceListByDeviceGroup.md#)|查询分组中的设备列表。|
|[QueryDeviceGroupByTags](intl.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupByTags.md#)|根据标签查询设备分组。|

## 规则引擎相关 API {#section_enr_vbd_xdb .section}

|API|描述|
|:--|:-|
|[ListRule](intl.zh-CN/云端开发指南/云端API参考/规则引擎/ListRule.md#)|查询规则列表。|
|[CreateRule](intl.zh-CN/云端开发指南/云端API参考/规则引擎/CreateRule.md#)|创建规则。|
|[GetRule](intl.zh-CN/云端开发指南/云端API参考/规则引擎/GetRule.md#)|查询规则信息。|
|[UpdateRule](intl.zh-CN/云端开发指南/云端API参考/规则引擎/UpdateRule.md#)|修改规则。|
|[DeleteRule](intl.zh-CN/云端开发指南/云端API参考/规则引擎/DeleteRule.md#)|删除规则。|
|[ListRuleActions](intl.zh-CN/云端开发指南/云端API参考/规则引擎/ListRuleActions.md#)|查询规则动作列表。|
|[GetRuleAction](intl.zh-CN/云端开发指南/云端API参考/规则引擎/GetRuleAction.md#)|查询规则动作信息。|
|[CreateRuleAction](intl.zh-CN/云端开发指南/云端API参考/规则引擎/CreateRuleAction.md#)|创建规则动作。|
|[UpdateRuleAction](intl.zh-CN/云端开发指南/云端API参考/规则引擎/UpdateRuleAction.md#)|更新规则动作。|
|[DeleteRuleAction](intl.zh-CN/云端开发指南/云端API参考/规则引擎/DeleteRuleAction.md#)|删除规则动作。|
|[StartRule](intl.zh-CN/云端开发指南/云端API参考/规则引擎/StartRule.md#)|启动规则。|
|[StopRule](intl.zh-CN/云端开发指南/云端API参考/规则引擎/StopRule.md#)|停止规则。|

## Topic 管理相关 API {#section_bqz_v42_xdb .section}

|API|描述|
|:--|:-|
|[QueryProductTopic](intl.zh-CN/云端开发指南/云端API参考/Topic管理/QueryProductTopic.md#)|查询产品Topic类。|
|[CreateProductTopic](intl.zh-CN/云端开发指南/云端API参考/Topic管理/CreateProductTopic.md#)|创建产品Topic类。|
|[UpdateProductTopic](intl.zh-CN/云端开发指南/云端API参考/Topic管理/UpdateProductTopic.md#)|修改产品Topic类。|
|[DeleteProductTopic](intl.zh-CN/云端开发指南/云端API参考/Topic管理/DeleteProductTopic.md#)|删除产品Topic类。|
|[CreateTopicRouteTable](intl.zh-CN/云端开发指南/云端API参考/Topic管理/CreateTopicRouteTable.md#)|添加Topic路由表。|
|[QueryTopicRouteTable](intl.zh-CN/云端开发指南/云端API参考/Topic管理/QueryTopicRouteTable.md#)|查询Topic路由表。|
|[QueryTopicReverseRouteTable](intl.zh-CN/云端开发指南/云端API参考/Topic管理/QueryTopicReverseRouteTable.md#)|查询Topic反向路由表。|
|[DeleteTopicRouteTable](intl.zh-CN/云端开发指南/云端API参考/Topic管理/DeleteTopicRouteTable.md#)|删除Topic路由表。|

## 消息通信相关 API {#section_oyc_jp2_xdb .section}

|API|描述|
|:--|:-|
|[Pub](intl.zh-CN/云端开发指南/云端API参考/消息通信/Pub.md#)|发布消息到Topic。|
|[RRpc](intl.zh-CN/云端开发指南/云端API参考/消息通信/RRpc.md#)|发消息给设备，并同步返回响应。|
|[PubBroadcast](intl.zh-CN/云端开发指南/云端API参考/消息通信/PubBroadcast.md#)|发布广播消息。|

## 设备影子相关 API {#section_ptd_vp2_xdb .section}

|API|描述|
|:--|:-|
|[GetDeviceShadow](intl.zh-CN/云端开发指南/云端API参考/设备影子/GetDeviceShadow.md#)|查询设备影子。|
|[UpdateDeviceShadow](intl.zh-CN/云端开发指南/云端API参考/设备影子/UpdateDeviceShadow.md#)|更新设备影子。|

