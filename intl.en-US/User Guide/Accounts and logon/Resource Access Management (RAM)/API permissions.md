# API permissions {#concept_sqt_mw4_tdb .concept}

Each operation in the following table represents the value of Action that you specify when creating authentication policies for RAM users.

|Operation|RAM action|Resource|Description|
|:--------|:---------|:-------|:----------|
|CreateProduct|iot:CreateProduct|\*|Create a product.|
|UpdateProduct|iot:UpdateProduct|\*|Modify information about a product.|
|QueryProduct|iot:QueryProduct|\*|Query information about a product.|
|QueryProductList|iot:QueryProductList|\*|Query information about all products.|
|GetCats|iot:GetCats|\*|Get the product category.|
|QueryProductByUserId|iot:QueryProductByUserId|\*|Query the products of a user who has a specified user ID.|
|RegisterDevice|iot:RegisterDevice|\*|Register a device.|
|QueryDevice|iot:QueryDevice|\*|Query information about all devices that are affiliated to a specified product.|
|QueryByDeviceId|iot:QueryByDeviceId|\*|Query information about a device that has a specified device ID.|
|QueryDeviceByName|iot:QueryDeviceByName|\*|Query information about a device that has a specified device name.|
|Deletedevice|Iot: deletedevice|\*|Delete a device.|
|ApplyDeviceWithNamesRequest|iot:ApplyDeviceWithNamesRequest|\*|Create multiple devices at a time.|
|QueryApplyStatus|iot:QueryApplyStatus|\*|Query application status.|
|QueryPageByApplyId|iot:QueryPageByApplyId|\*|Query information about devices that are registered simultaneously in one application that has a specified application ID. The query result is displayed on multiple pages.|
|BatchGetDeviceState|iot:BatchGetDeviceState|\*|Get status of multiple devices at a time.|
|BatchRegisterDeviceWithApplyId|iot:BatchRegisterDeviceWithApplyId|\*|Register multiple devices simultaneously using a given application ID.|
|BatchRegisterDevice|iot:BatchRegisterDevice|\*|Register multiple devices at a time.|
|QueryBatchRegisterDeviceStatus|iot:QueryBatchRegisterDeviceStatus|\*|Query the status of the application to register multiple devices at a time.|
|BatchCheckDeviceNames|iot:BatchCheckDeviceNames|\*|Check whether names of the devices that you want to register simultaneously are valid.|
|GetDeviceStatus|iot:GetDeviceStatus|\*|Get the running status of a device.|
|QueryDeviceStatistics|iot:QueryDeviceStatistics|\*|Query device statistics.|
|QueryDeviceEventData|iot:QueryDeviceEventData|\*|Query historical events of a device.|
|QueryDeviceServiceData|iot:QueryDeviceServiceData|\*|Query historical servicing records of a device.|
|SetDeviceProperty|iot:SetDeviceProperty|\*|Configure properties of a device.|
|InvokeThingService|iot:InvokeThingService|\*|Enable a service on a device.|
|QueryDevicePropertyStatus|iot:QueryDevicePropertyStatus|\*|Query the property information of a device.|
|QueryDeviceDetail|iot:QueryDeviceDetail|\*|Query details about a device.|
|DisableThing|iot:DisableThing|\*|Disable a device.|
|EnableThing|iot:EnableThing|\*|Enable a device.|
|GetThingTopo|iot:GetThingTopo|\*|Query devices connected to the specified device in the topological hierarchy.|
|RemoveThingTopo|iot:RemoveThingTopo|\*|Detach sub-devices from the gateway in the topological hierarchy. |
|NotifyAddThingTopo|iot:NotifyAddThingTopo|\*|Notify the specified gateway device to attach a list of devices to the gateway in the topological hierarchy.|
|QueryDevicePropertyData|iot:QueryDevicePropertyData|\*|Query historical property records of a device.|
|QueryTopic|iot:QueryTopic|\*|Query a topic.|
|StartRule|iot:StartRule|\*|Enable a rule.|
|StopRule|iot:StopRule|\*|Disable a rule.|
|ListRule|iot:ListRule|\*|Query information about all rules.|
|GetRule|iot:GetRule|\*|Query details about a rule.|
|CreateRule|iot:CreateRule|\*|Create a rule.|
|UpdateRule|iot:UpdateRule|\*|Modify a rule.|
|DeleteRule|iot:DeleteRule|\*|Delete a rule.|
|CreateRuleAction|iot:CreateRuleAction|\*|Create a data forwarding action for a rule.|
|UpdateRuleAction|iot:UpdateRuleAction|\*|Modify a data forwarding action.|
|DeleteRuleAction|iot:DeleteRuleAction|\*|Delete a data forwarding action.|
|GetRuleAction|iot:GetRuleAction|\*|Query information about a data forwarding action.|
|ListRuleActions|iot:ListRuleActions|\*|Query information about all data forwarding actions of a rule.|
|Pub|iot:Pub|\*|Publish a message.|
|Sub|iot:Sub|\*|Subscribe to messages.|
|Unsub|iot:Unsub|\*|Cancel subscriptions to messages.|
|ServerOnline|iot:ServerOnline|\*|Bring online a device. The service subscribes to the status of this device.|
|DeviceGrant|iot:DeviceGrant|\*|Grant a permission to a device.|
|DeviceRevokeByTopic|iot:DeviceRevokeByTopic|\*|Revoke the topic subscription permission from devices that subscribe to a specified topic.|
|DeviceRevokeById|iot:DeviceRevokeById|\*|Revoke a permission from a device that has a specified ID.|
|DevicePermitModify|iot:DevicePermitModify|\*|Modify a permission of a device.|
|ListPermits|iot:ListPermits|\*|Lists permissions of a device.|
|RRpc|iot:RRpc|\*|Send a request to a device and retrieve a response from the device.|
|CreateProductTopic|iot:CreateProductTopic|\*|Create a topic category for a product.|
|DeleteProductTopic|iot:DeleteProductTopic|\*|Delete a topic category.|
|QueryProductTopic|iot:QueryProductTopic|\*|Query information about all topic categories of a product.|
|UpdateProductTopic|iot:UpdateProductTopic|\*|Modify a topic category.|
|QueryDeviceTopic|iot:QueryDeviceTopic|\*|Query information about all topics to which a device subscribes to.|
|DebugRuleSql|iot:DebugRuleSql|\*|Debug SQL scripts for the rules engine.|

