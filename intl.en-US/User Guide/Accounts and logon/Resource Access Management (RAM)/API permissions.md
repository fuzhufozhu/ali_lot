# API permissions {#concept_sqt_mw4_tdb .concept}

Each operation in the following table represents the value of Action that you specify when creating authentication policies for RAM users.

|Operation|RAM action|Resource|Description|
|:--------|:---------|:-------|:----------|
|CreateProduct|iot:CreateProduct|\*|Create a product.|
|UpdateProduct|iot:UpdateProduct|\*|Modify the information of a product.|
|QueryProduct|iot:QueryProduct|\*|Query the detailed information of a product.|
|QueryProductList|iot:QueryProductList|\*|Query the list of all products.|
|DeleteProduct|iot:DeleteProduct|\*|Delete a product.|
|RegisterDevice|iot:RegisterDevice|\*|Register a device.|
|QueryDevice|iot:QueryDevice|\*|Query the list of all devices of a specified product.|
|DeleteDevice|Iot: DeleteDevice|\*|Delete a device.|
|QueryPageByApplyId|iot:QueryPageByApplyId|\*|Query the information of devices that are registered at a time.|
|BatchGetDeviceState|iot:BatchGetDeviceState|\*|Get the status of multiple devices at a time.|
|BatchRegisterDeviceWithApplyId|iot:BatchRegisterDeviceWithApplyId|\*|Register multiple devices simultaneously using a given application ID.|
|BatchRegisterDevice|iot:BatchRegisterDevice|\*|Register devices at a time \(not specify device names\).|
|QueryBatchRegisterDeviceStatus|iot:QueryBatchRegisterDeviceStatus|\*|Query the processing status and result of device registration of multiple devices.|
|BatchCheckDeviceNames|iot:BatchCheckDeviceNames|\*|Specify device names in batch.|
|QueryDeviceStatistics|iot:QueryDeviceStatistics|\*|Query device statistics.|
|QueryDeviceEventData|iot:QueryDeviceEventData|\*|Query historical events of a device.|
|QueryDeviceServiceData|iot:QueryDeviceServiceData|\*|Query historical servicing records of a device.|
|SetDeviceProperty|iot:SetDeviceProperty|\*|Configure properties of a device.|
|InvokeThingService|iot:InvokeThingService|\*|Invoke a service of a device.|
|QueryDevicePropertyStatus|iot:QueryDevicePropertyStatus|\*|Query the property information of a device.|
|QueryDeviceDetail|iot:QueryDeviceDetail|\*|Query details about a device.|
|DisableThing|iot:DisableThing|\*|Disable a device.|
|EnableThing|iot:EnableThing|\*|Enable a device that has been disabled.|
|GetThingTopo|iot:GetThingTopo|\*|Query the topological relationships of a device.|
|RemoveThingTopo|iot:RemoveThingTopo|\*|Delete the topological relationships of a device.|
|NotifyAddThingTopo|iot:NotifyAddThingTopo|\*|Notify a gateway device to add topological relationships of the connected sub-devices.|
|QueryDevicePropertyData|iot:QueryDevicePropertyData|\*|Query historical property records of a device.|
|GetGatewayBySubDevice|iot:GetGatewayBySubDevice|\*|Query the gateway device information using the sub-device information.|
|SaveDeviceProp|iot:SaveDeviceProp|\*|Create a tag for a device.|
|QueryDeviceProp|iot:QueryDeviceProp|\*|Query the tag lists of a device.|
|DeleteDeviceProp|iot:DeleteDeviceProp|\*|Delete a tag of a device.|
|QueryAppDeviceList|iot:QueryAppDeviceList|\*|Query the list of devices that are bound with an specified app.|
|StartRule|iot:StartRule|\*|Enable a rule.|
|StopRule|iot:StopRule|\*|Stop a rule.|
|ListRule|iot:ListRule|\*|Query the information of all rules.|
|GetRule|iot:GetRule|\*|Query the details of a rule.|
|CreateRule|iot:CreateRule|\*|Create a rule.|
|UpdateRule|iot:UpdateRule|\*|Modify a rule.|
|DeleteRule|iot:DeleteRule|\*|Delete a rule.|
|CreateRuleAction|iot:CreateRuleAction|\*|Create a data forwarding method for a rule.|
|UpdateRuleAction|iot:UpdateRuleAction|\*|Modify a data forwarding method.|
|DeleteRuleAction|iot:DeleteRuleAction|\*|Delete a data forwarding method.|
|GetRuleAction|iot:GetRuleAction|\*|Query the detailed information of data forwarding method|
|ListRuleActions|iot:ListRuleActions|\*|Query the list of data forwarding methods in a rule.|
|Pub|iot:Pub|\*|Publish messages.|
|PubBroadcast|iot:PubBroadcast|\*|Publish messages to the devices that have subscribed to a broadcast topic.|
|RRpc|iot:RRpc|\*|Send a request to a device and retrieve a response from the device.|
|CreateProductTopic|iot:CreateProductTopic|\*|Create a topic category for a product.|
|DeleteProductTopic|iot:DeleteProductTopic|\*|Delete a topic category.|
|QueryProductTopic|iot:QueryProductTopic|\*|Query information about all topic categories of a product.|
|UpdateProductTopic|iot:UpdateProductTopic|\*|Modify a topic category.|
|CreateTopicRouteTable|iot:CreateTopicRouteTable|\*|Create message routing relationships between topics.|
|DeleteTopicRouteTable|iot:DeleteTopicRouteTable|\*|Delete message routing relationships.|
|QueryTopicReverseRouteTable|iot:QueryTopicReverseRouteTable|\*|Query the source topic of a target topic.|
|QueryTopicRouteTable|iot:QueryTopicRouteTable|\*|Query the target topics of a source topic.|
|GetDeviceShadow|iot:GetDeviceShadow|\*|Query the shadow information of a device.|
|UpdateDeviceShadow|iot:UpdateDeviceShadow|\*|Modify the shadow information of a device.|

