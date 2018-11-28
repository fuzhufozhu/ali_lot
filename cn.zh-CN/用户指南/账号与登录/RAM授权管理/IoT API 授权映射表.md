# IoT API 授权映射表 {#concept_sqt_mw4_tdb .concept}

下表中 IoT API 名称，即您在创建 IoT 相关授权策略时，参数 Action 的可选值。

为RAM用户授权的具体方法，请参见[自定义权限](intl.zh-CN/用户指南/账号与登录/RAM授权管理/自定义权限.md#)。

|IoT API|RAM 授权操作（Action\)|资源 （Resource）|接 口 说 明|
|:------|:----------------|:------------|:------|
|CreateProduct|iot:CreateProduct|\*|创建产品。|
|UpdateProduct|iot:UpdateProduct|\*|修改产品。|
|QueryProduct|iot:QueryProduct|\*|查询产品信息。|
|QueryProductList|iot:QueryProductList|\*|查询产品列表。|
|DeleteProduct|iot:DeleteProduct|\*|删除产品。|
|RegisterDevice|iot:RegisterDevice|\*|注册设备。|
|QueryDevice|iot:QueryDevice|\*|查询指定产品下的所有设备列表。|
|DeleteDevice|iot:DeleteDevice|\*|删除设备。|
|QueryPageByApplyId|iot:QueryPageByApplyId|\*|查询批量注册的设备信息。|
|BatchGetDeviceState|iot:BatchGetDeviceState|\*|批量获取设备状态。|
|BatchRegisterDeviceWithApplyId|iot:BatchRegisterDeviceWithApplyId|\*|根据ApplyId批量申请设备。|
|BatchRegisterDevice|iot:BatchRegisterDevice|\*|批量注册设备（随机生成设备名）。|
|QueryBatchRegisterDeviceStatus|iot:QueryBatchRegisterDeviceStatus|\*|查询批量注册设备的处理状态和结果。|
|BatchCheckDeviceNames|iot:BatchCheckDeviceNames|\*|批量自定义设备名称。|
|QueryDeviceStatistics|iot:QueryDeviceStatistics|\*|获取设备的统计数量。|
|QueryDeviceEventData|iot:QueryDeviceEventData|\*|获取设备的事件历史数据。|
|QueryDeviceServiceData|iot:QueryDeviceServiceData|\*|获取设备的服务记录历史数据。|
|SetDeviceProperty|iot:SetDeviceProperty|\*|设置设备的属性。|
|SetDevicesProperty|iot:SetDevicesProperty|\*|批量设置设备属性。|
|InvokeThingService|iot:InvokeThingService|\*|调用设备的服务。|
|InvokeThingsService|iot:InvokeThingsService|\*|批量调用设备服务。|
|QueryDevicePropertyStatus|iot:QueryDevicePropertyStatus|\*|查询设备的属性快照。|
|QueryDeviceDetail|iot:QueryDeviceDetail|\*|查询设备详情。|
|DisableThing|iot:DisableThing|\*|禁用设备。|
|EnableThing|iot:EnableThing|\*|解除设备的禁用状态。|
|GetThingTopo|iot:GetThingTopo|\*|查询设备拓扑关系。|
|RemoveThingTopo|iot:RemoveThingTopo|\*|移除设备拓扑关系。|
|NotifyAddThingTopo|iot:NotifyAddThingTopo|\*|通知云端增加设备拓扑关系。|
|QueryDevicePropertyData|iot:QueryDevicePropertyData|\*|获取设备的属性历史数据。|
|GetGatewayBySubDevice|iot:GetGatewayBySubDevice|\*|根据挂载的子设备信息查询对应的网关设备信息。|
|SaveDeviceProp|iot:SaveDeviceProp|\*|为指定设备设置标签。|
|QueryDeviceProp|iot:QueryDeviceProp|\*|查询指定设备的标签列表。|
|DeleteDeviceProp|iot:DeleteDeviceProp|\*|删除设备标签。|
|QueryDeviceByTags|iot:QueryDeviceByTags|\*|根据标签查询设备。|
|CreateDeviceGroup|iot:CreateDeviceGroup|\*|创建分组。|
|UpdateDeviceGroup|iot:UpdateDeviceGroup|\*|更新分组信息。|
|DeleteDeviceGroup|iot:DeleteDeviceGroup|\*|删除分组。|
|BatchAddDeviceGroupRelations|iot:BatchAddDeviceGroupRelations|\*|添加设备到分组。|
|BatchDeleteDeviceGroupRelations|iot:BatchDeleteDeviceGroupRelations|\*|将设备从分组中删除。|
|QueryDeviceGroupInfo|iot:QueryDeviceGroupInfo|\*|查询分组详情。|
|QueryDeviceGroupList|iot:QueryDeviceGroupList|\*|查询分组列表。|
|SetDeviceGroupTags|iot:SetDeviceGroupTags|\*|添加或更新分组标签。|
|QueryDeviceGroupTagList|iot:QueryDeviceGroupTagList|\*|查询分组标签列表。|
|QueryDeviceGroupByDevice|iot:QueryDeviceGroupByDevice|\*|查询指定设备所在的分组列表。|
|StartRule|iot:StartRule|\*|启动规则。|
|StopRule|iot:StopRule|\*|暂停规则。|
|ListRule|iot:ListRule|\*|查询规则列表。|
|GetRule|iot:GetRule|\*|查询规则详情。|
|CreateRule|iot:CreateRule|\*|创建规则。|
|UpdateRule|iot:UpdateRule|\*|修改规则。|
|DeleteRule|iot:DeleteRule|\*|删除规则。|
|CreateRuleAction|iot:CreateRuleAction|\*|创建规则中的数据转发方法。|
|UpdateRuleAction|iot:UpdateRuleAction|\*|修改规则中的数据转发方法。|
|DeleteRuleAction|iot:DeleteRuleAction|\*|删除规则中的数据转发方法。|
|GetRuleAction|iot:GetRuleAction|\*|查询规则中的数据转发方法的详细信息。|
|ListRuleActions|iot:ListRuleActions|\*|获取规则中的数据转发方法列表。|
|Pub|iot:Pub|\*|发布消息。|
|PubBroadcast|iot:PubBroadcast|\*|向订阅了指定产品广播Topic的所有设备发送消息。|
|RRpc|iot:RRpc|\*|发送消息给设备并得到设备响应。|
|CreateProductTopic|iot:CreateProductTopic|\*|创建产品Topic类。|
|DeleteProductTopic|iot:DeleteProductTopic|\*|删除产品Topic类。|
|QueryProductTopic|iot:QueryProductTopic|\*|查询产品Topic类列表。|
|UpdateProductTopic|iot:UpdateProductTopic|\*|修改产品Topic类。|
|CreateTopicRouteTable|iot:CreateTopicRouteTable|\*|新建Topic间的消息路由关系。|
|DeleteTopicRouteTable|iot:DeleteTopicRouteTable|\*|删除Topic路由关系。|
|QueryTopicReverseRouteTable|iot:QueryTopicReverseRouteTable|\*|查询指定Topic订阅的源Topic。|
|QueryTopicRouteTable|iot:QueryTopicRouteTable|\*|查询向指定Topic订阅消息的目标Topic。|
|GetDeviceShadow|iot:GetDeviceShadow|\*|查询设备的影子信息。|
|UpdateDeviceShadow|iot:UpdateDeviceShadow|\*|修改设备的影子信息。|

