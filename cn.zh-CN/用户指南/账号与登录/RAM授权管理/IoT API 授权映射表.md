# IoT API 授权映射表 {#concept_sqt_mw4_tdb .concept}

下表中IoT API名称，即您在创建IoT相关授权策略时，参数Action的可选值。

|IoT API|RAM 授权操作（Action\)|资源 （Resource）|接 口 说 明|
|:------|:----------------|:------------|:------|
|CreateProduct|iot:CreateProduct|\*|创建产品|
|UpdateProduct|iot:UpdateProduct|\*|修改产品|
|QueryProduct|iot:QueryProduct|\*|查询产品|
|QueryProductList|iot:QueryProductList|\*|查询产品列表|
|GetCats|iot:GetCats|\*|获取产品类型信息|
|QueryProductByUserId|iot:QueryProductByUserId|\*|根据用户id查询产品|
|RegisterDevice|iot:RegisterDevice|\*|注册设备|
|QueryDevice|iot:QueryDevice|\*|批量查询设备|
|QueryByDeviceId|iot:QueryByDeviceId|\*|根据deviceId查询设备|
|QueryDeviceByName|iot:QueryDeviceByName|\*|根据deviceName查询设备|
|DeleteDevice|iot:DeleteDevice|\*|删除设备|
|ApplyDeviceWithNamesRequest|iot:ApplyDeviceWithNamesRequest|\*|批量创建设备|
|QueryApplyStatus|iot:QueryApplyStatus|\*|查询申请状态|
|QueryPageByApplyId|iot:QueryPageByApplyId|\*|根据申请id分页查询|
|BatchGetDeviceState|iot:BatchGetDeviceState|\*|批量获取设备状态|
|BatchRegisterDeviceWithApplyId|iot:BatchRegisterDeviceWithApplyId|\*|根据ApplyId批量申请设备|
|BatchRegisterDevice|iot:BatchRegisterDevice|\*|批次申请特定数量设备|
|QueryBatchRegisterDeviceStatus|iot:QueryBatchRegisterDeviceStatus|\*|查询批量注册设备状态|
|BatchCheckDeviceNames|iot:BatchCheckDeviceNames|\*|批量检查设备名称|
|GetDeviceStatus|iot:GetDeviceStatus|\*|获取设备的运行状态|
|QueryDeviceStatistics|iot:QueryDeviceStatistics|\*|获取设备的统计数量|
|QueryDeviceEventData|iot:QueryDeviceEventData|\*|获取设备的事件历史数据|
|QueryDeviceServiceData|iot:QueryDeviceServiceData|\*|获取设备的服务记录历史数据|
|SetDeviceProperty|iot:SetDeviceProperty|\*|设置设备的属性|
|InvokeThingService|iot:InvokeThingService|\*|设备的服务调用|
|QueryDevicePropertyStatus|iot:QueryDevicePropertyStatus|\*|查询设备的属性快照|
|QueryDeviceDetail|iot:QueryDeviceDetail|\*|查询设备详情|
|DisableThing|iot:DisableThing|\*|设备的禁用|
|EnableThing|iot:EnableThing|\*|设备的解禁|
|GetThingTopo|iot:GetThingTopo|\*|查询设备拓扑关系|
|RemoveThingTopo|iot:RemoveThingTopo|\*|移除设备拓扑关系|
|NotifyAddThingTopo|iot:NotifyAddThingTopo|\*|通知云端增加设备拓扑关系|
|QueryDevicePropertyData|iot:QueryDevicePropertyData|\*|获取设备的属性历史数据|
|QueryTopic|iot:QueryTopic|\*|查询Topic|
|StartRule|iot:StartRule|\*|启动规则|
|StopRule|iot:StopRule|\*|暂停规则|
|ListRule|iot:ListRule|\*|查询规则列表|
|GetRule|iot:GetRule|\*|查询规则详情|
|CreateRule|iot:CreateRule|\*|创建规则|
|UpdateRule|iot:UpdateRule|\*|修改规则|
|DeleteRule|iot:DeleteRule|\*|删除规则|
|CreateRuleAction|iot:CreateRuleAction|\*|创建规则中的转发数据方法|
|UpdateRuleAction|iot:UpdateRuleAction|\*|修改规则中的转发数据方法|
|DeleteRuleAction|iot:DeleteRuleAction|\*|删除规则中的转发数据方法|
|GetRuleAction|iot:GetRuleAction|\*|获取规则中的转发数据方法|
|ListRuleActions|iot:ListRuleActions|\*|获取规则中的转发数据方法列表|
|Pub|iot:Pub|\*|发布消息|
|Sub|iot:Sub|\*|订阅消息|
|Unsub|iot:Unsub|\*|取消订阅|
|DeviceRevokeById|iot:DeviceRevokeById|\*|通过ID撤销设备权限|
|DevicePermitModify|iot:DevicePermitModify|\*|修改设备权限|
|ListPermits|iot:ListPermits|\*|列出设备的权限|
|RRpc|iot:RRpc|\*|发送消息给设备并得到设备响应|
|CreateProductTopic|iot:CreateProductTopic|\*|创建产品Topic类|
|DeleteProductTopic|iot:DeleteProductTopic|\*|删除产品Topic类|
|QueryProductTopic|iot:QueryProductTopic|\*|查询产品Topic类列表|
|UpdateProductTopic|iot:UpdateProductTopic|\*|修改产品Topic类|
|QueryDeviceTopic|iot:QueryDeviceTopic|\*|查询设备Topic列表|
|DebugRuleSql|iot:DebugRuleSql|\*|规则引擎SQL调试|

