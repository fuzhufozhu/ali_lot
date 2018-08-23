# Error codes {#reference_c1q_nms_s2b .reference}

This article lists the error codes which are returned when calls to IoT Platform APIs fail.

## System error codes {#section_kgd_5qf_t2b .section}

An error code beginning with `iot.system` indicates a system error.

|Error code|Description|
|:---------|:----------|
|iot.system.SystemException|A system exception occurred.|

## Common error codes {#section_og4_ysy_s2b .section}

Error codes beginning with `iot.common` are common error codes.

|Error code|Description|
|:---------|:----------|
|iot.common.InvalidPageParams|The specified page size or page number is invalid.|
|iot.common.InvalidTenant|The tenant is invalid.|
|iot.common.QueryDeviceActionError|Failed to query the device.|
|iot.common.QueryDevicePropertyActionError|Failed to query the device property.|
|iot.common.QueryProductActionError|Failed to query the product.|
|iot.common.QueryProductCountActionError|Failed to query the total number of products.|
|iot.common.RamActionPermissionDeny|You do not have the RAM permission to access the resource.|

## Error codes related to IoT Platform products {#section_ozj_1ty_s2b .section}

Error codes beginning with `iot.prod` are error codes related to IoT Platform products.

|Error code|Description|
|:---------|:----------|
|iot.prod.AlreadyExistedProductName|The same product name already exists.|
|iot.prod.CreateProductFailed|Failed to create the product.|
|iot.prod.CreateProductTopicFailed|Failed to create the topic category.|
|iot.prod.InvalidAliyunCommodityCode|The value of AliyunCommodityCode is invalid.|
|iot.prod.InvalidFormattedCatId|The format of the device type is invalid.|
|iot.prod.InvalidFormattedProductkey|The format of the ProductKey is invalid.|
|iot.prod.InvalidFormattedProductName|The format of the ProductName is invalid.|
|iot.prod.LongProductDesc|The product description length exceeds the maximum limit.|
|iot.prod.InvalidNodeType|The node type of the product is invalid.|
|iot.prod.NotExistedProduct|The specified product does not exist.|
|iot.prod.NotOpenID2Service|You have not activated the ID2 service.|
|iot.prod.NotSeniorProduct|The product is not a Pro Edition product.|
|iot.prod.NullProductKey|ProductKey cannot be empty.|
|iot.prod.NullProductName|ProductName cannot be empty.|
|iot.prod.ProductCountExceedMax|The total number of products exceeds the maximum limit.|
|iot.prod.QueryDeviceCountActionError|Failed to query the total number of devices.|
|iot.prod.QueryProductAbilitiesFailed|Failed to query the product feature.|
|iot.prod.QueryProductAbilityFailed|Failed to query the product feature.|
|iot.prod.QueryProductListActionError|Failed to query the product list.|
|iot.prod.UpdateProductFailed|Failed to update the product information.|

## Error codes related to devices {#section_egs_q4z_s2b .section}

Error codes beginning with `iot.device` are error codes related to devices.

|Error code|Description|
|:---------|:----------|
|iot.device.AddTopoRelationFailed|Failed to add topological relationship.|
|iot.device.AlreadyExistedDeviceName|The device name already exists.|
|iot.device.ApplyManyDevicesFailed|Failed to batch register devices.|
|iot.device.CreateDeviceFailed|Failed to create the device.|
|iot.device.CreateDeviceTaskIsRunning|The device creation task is still in progress.|
|iot.device.DeviceApplyIsNotFound|The ApplyId is not found.|
|iot.device.DeviceCountExceeded|Number of devices requested exceeds the maximum limit.|
|iot.device.DeleteDeviceFailed|Failed to delete the device.|
|iot.device.DeleteDevicePropertyFailed|Failed to delete the device property.|
|iot.device.DisableDeviceFailed|Failed to disable the device.|
|iot.device.EnableDeviceFailed|Failed to enable the device.|
|iot.device.InactiveDevice|The device is inactive.|
|iot.device.InvalidFormattedApplyId|The ApplyId is incorrect.|
|iot.device.IncorrentDeviceApplyInfo|The device application information is not correct.|
|iot.device.InvalidFormattedDeviceName|The format of the device name is invalid.|
|iot.device.InvalidFormattedDevicePropertyKey|The format of the device property name is invalid.|
|iot.device.InvalidFormattedDevicePropertiesString|The format of device property string is invalid.|
|iot.device.InvalidIoTId|The device IotId is incorrect.|
|iot.device.InvalidTimeBucket|The specified time range is invalid.|
|iot.device.InvokeThingServiceFailed|Failed to invoke the service.|
|iot.device.LongDevicePropertiesString|The length of the property string exceeds the maximum limit.|
|iot.device.NoneDeviceNameElement|The device name list cannot be empty.|
|iot.device.NoneDeviceProperties|No valid device property is found.|
|iot.device.NotExistedDevice|The specified device does not exist.|
|iot.device.NullApplyId|ApplyId cannot be empty.|
|iot.device.NullDeviceName|Device name cannot be empty.|
|iot.device.NullDevicePropertyKey|Device property name cannot be empty.|
|iot.device.NullDevicePropertiesString|Device property cannot be empty.|
|iot.device.QueryDeviceApplyActionError|An error occurred when the device application information was queried.|
|iot.device.QueryDeviceAttrDataHistoryFailed|Failed to query the device property data.|
|iot.device.QueryDeviceAttrStatusFailed|Failed to query the device property status.|
|iot.device.QueryDeviceEventHistoryFailed|Failed to query the device event data.|
|iot.device.QueryDeviceListActionError|Failed to query the device list.|
|iot.device.QueryDeviceServiceHistoryFailed|Failed to query the device service data.|
|iot.device.QueryDeviceStatisticsFailed|Failed to query the device statistics.|
|iot.device.QueryDeviceStatusFailed|Failed to query the device status.|
|iot.device.QueryTopoRelationFailed|Failed to query the topological relationship.|
|iot.device.RemoveTopoRelationFailed|Failed to delete the topological relationship.|
|iot.device.SaveOrUpdateDevicePropertiesFailed|Failed to add or update the device property.|
|iot.device.SetDevicePropertyFailed|Failed to set device property.|
|iot.device.TooManyDevicePropertiesPerTime|The total number of device properties exceeds the limit.|
|iot.device.TopoRelationCountExceeded|The number of topology relationships exceeds the limit.|
|iot.device.VerifyDeviceFailed|Failed to verify the device.|

## Message related error codes. {#section_xs2_x4f_t2b .section}

Error codes beginning with `iot.messagebroker` are error codes related to message communication. This type of error code appears when you fail to call APIs related to message communication, device shadow, and rule engine. \(For error codes related to rule engine APIs, see the following section.\)

|Error code|Description|
|:---------|:----------|
|iot.messagebroker.CreateTopicRouteFailed|Failed to create the route between the specified topics.|
|iot.messagebroker.CreateTopicTemplateException|An exception occurred when creating the topic category.|
|iot.messagebroker.CreateTopicTemplateFailed|Failed to create the topic category.|
|iot.messagebroker.DeleteTopicTemplateException|An exception occurred when deleting the topic category.|
|iot.messagebroker.DeleteTopicTemplateFailed|Failed to delete the topic category.|
|iot.messagebroker.DestTopicNameArraySizeIsLarge|The number of target topics for one source topic exceeds the limit.|
|iot.messagebroker.DeleteTopicRouteFailed|Failed to delete the route between the topics.|
|iot.messagebroker.DesireInfoInShadowMessageIsNotJson|desire information of device shadow is not in JSON format.|
|iot.messagebroker.DesireValueIsNullInShadowMessage|desire information of device shadow cannot be empty.|
|iot.messagebroker.ElementKeyOrValueIsNullInDesire|desire information key or value is empty.|
|iot.messagebroker.ElementKeyOrValueIsNullInReport|report information key or value is empty.|
|iot.messagebroker.HALFCONN|Failed because the device is semi-connected.|
|iot.messagebroker.InvalidFormattedSrcTopicName|Format of the source topic is invalid.|
|iot.messagebroker.InvalidFormattedTopicName|Format of the topic is invalid.|
|iot.messagebroker.InvalidFormattedTopicTemplateId|Format of the topic category is invalid.|
|iot.messagebroker.InvalidTimeoutValue|The timeout value is invalid.|
|iot.messagebroker.InvalidTopicTemplateOperationValue|The operation value of the topic category is not correct.|
|iot.messagebroker.InvalidVersionValueInShadowMessage|The version value of device shadow is not correct.|
|iot.messagebroker.MethodValueIsNotUpdate|The method value of device shadow is not update.|
|iot.messagebroker.MessageContentIsNotBase64Encode|The message content is not base64 encoded.|
|iot.messagebroker.NoneElementInDesire|desire cannot be empty.|
|iot.messagebroker.NoneElementInReport|report cannot be empty.|
|iot.messagebroker.NoneElementDestTopicNameInArray|Target topic list cannot be empty.|
|iot.messagebroker.NotFoundDesireInShadowMessage|desire information of state of device shadow is not found.|
|iot.messagebroker.NotFoundMethodInShadowMessage|method information of device shadow is not found.|
|iot.messagebroker.NotFoundReportInShadowMessage|report information of device shadow is not found.|
|iot.messagebroker.NotFoundStateInShadowMessage|state information of device shadow is not found.|
|iot.messagebroker.NotFoundVersionOrNullVersionValue|version information is not complete or the value of version is empty.|
|iot.messagebroker.NotMatchedProductKeyWithSrcTopicOwner|The product, which the source topic belongs to, does not belong to the current account. |
|iot.messagebroker.NullMessageContent|Message content cannot be empty.|
|iot.messagebroker.NullShadowMessage|Content of device shadow cannot be empty.|
|iot.messagebroker.NullSrcTopicName|Source topic name cannot be empty.|
|iot.messagebroker.NullTopicName|Topic cannot be empty.|
|iot.messagebroker.NullTopicTemplateId|Topic category cannot be empty.|
|iot.messagebroker.NullTopicTemplateOperation|Operation of the topic category cannot be empty.|
|iot.messagebroker.OFFLINE|Failed because the device is offline.|
|iot.messagebroker.PublishMessageException|An exception occurred when publishing message.|
|iot.messagebroker.PublishMessageFailed|Failed to publish message.|
|iot.messagebroker.QueryDeviceShadowActionError|Failed to query device shadow.|
|iot.messagebroker.QueryProductTopicListActionError|Failed to query topic category list.|
|iot.messageborker.QueryTopicReverseRouteTableListActionError|Failed to query source topic list.|
|iot.messageborker.QueryTopicRouteTableListActionError|Failed to query route table list.|
|iot.messagebroker.QueryTopicTemplateActionError|Failed to query topic category.|
|iot.messagebroker.QueryTopicTemplateException|An exception occurred when querying topic category.|
|iot.messagebroker.RateLimit|Failed because of rate limit.|
|iot.messagebroker.ReportInShadowMessageIsNotJson|report information of state is not in JSON format.|
|iot.messagebroker.RrpcException|An exception occurred when sending message by PPRC.|
|iot.messagebroker.RrpcFailed|Failed to send message by RRPC.|
|iot.messagebroker.ShadowMessageIsNotJson|Device shadow is not in JSON format.|
|iot.messagebroker.ShadowMessageLengthIsLarge|The length of device shadow exceeds the limit.|
|iot.messagebroker.TIMEOUT|Failed because of timeout.|
|iot.messagebroker.TooManyElementInDesire|The total number of desire elements exceeds the limit.|
|iot.messagebroker.TooManyElementInReport|The total number of report elements exceeds the limit.|
|iot.messagebroker.TopicAlreadyFound|The topic category name already exists.|
|iot.messagebroker.TopicTemplateCountExceedMax|The number of topic categories of the product exceeds the limit.|
|iot.messagebroker.TopicTemplateIsNotFound|The topic category is not found.|
|iot.messagebroker.UpdateDeviceShadowMessageFailed|Failed to update device shadow.|
|iot.messagebroker.UpdateTopicTemplateException|An exception occurred when updating the topic category.|
|iot.messagebroker.UpdateTopicTemplateFailed|Failed to update the topic category.|

## Error codes related to rule engine. {#section_erm_31h_t2b .section}

Error codes beginning with `iot.rule` and `iot.ruleng`, and some `iot.messagebroker` started error codes are related to rule engine. 

|Error code|Description|
|:---------|:----------|
|iot.rule.CreateRuleException|An exception occurred when creating the rule.|
|iot.rule.DeleteRuleFailed|Failed to delete the rule.|
|iot.rule.IncorrentRuleActionId|The rule action ID is not correct.|
|iot.rule.IncorrentRuleActionType|The type of the rule action is not correct.|
|iot.rule.IncorrentRuleId|The rule ID is not correct.|
|iot.rule.NullForwardDestForRule|The data forwarding destination cannot be empty.|
|iot.rule.NullSqlForRule|SQL statement for the rule cannot be empty.|
|iot.rule.NotFoundRule|The specified rule is not found.|
|iot.rule.NotFoundRuleAction|The specified rule action is not found.|
|iot.rule.ParseRuleActionConfigError|Failed to parse the rule action configuration.|
|iot.rule.QueryRuleActionListError|Failed to query the rule action list.|
|iot.rule.QueryRulePageActionError|Failed to query the rule list.|
|iot.rule.RuleActionIsAlreadyCreated|The same rule action already exists.|
|iot.rule.RuleCountExceedMax|The total number of rules exceeds the limit.|
|iot.rule.RuleNameIsAlreadyExisted|The rule name already exists.|
|iot.rule.StartRuleFailed|Failed to start the rule.|
|iot.rule.StopRuleFailed|Failed to stop the rule.|
|iot.rule.TooManyRuleAction|The number of rule actions exceeds the limit.|
|iot.rule.UpdateRuleFailed|Failed to update the rule.|
|iot.ruleng.CreateRuleActionFailed|Failed to create the rule action.|
|iot.ruleng.DeleteRuleActionFailed|Failed to delete the rule action.|
|iot.ruleng.IncorrectType|The topic type is incorrect.|
|iot.ruleng.IncorrectSysTopic|The system topic is incorrect.|
|iot.ruleng.InvalidRamRole|The RAM role is invalid.|
|iot.ruleng.QueryRuleActionFailed|Failed to query the rule action.|
|iot.ruleng.RuleActionConfigurationIsNotJson|The rule action configuration is not in JSON format.|
|iot.ruleng.RuleAlreadyIsStarted|The rule is already in a started status.|
|iot.ruleng.NullRamRoleArn|roleArn cannot be empty.|
|iot.ruleng.NullRamRoleName|roleName cannot be empty.|
|iot.ruleng.NullRuleActionConfig|Rule action configuration cannot be empty.|
|iot.ruleng.NullRuleActionType|Rule action type cannot be empty.|
|iot.messagebroker.IncorrectRuleSql|The SQL statement of the rule is not correct.|
|iot.messagebroker.QueryRuleConfigActionException|An exception occurred when configuring the rule.|

The following tables list the error codes related to different message targets.

|Error code|Description|
|:---------|:----------|
|iot.messagebroker.InvalidFormattedTopicName|The theme name format is invalid.|
|iot.prod.NotExistedProduct|The specified product does not exist.|
|iot.common.QueryProductActionError|Failed to query the product.|
|iot.ruleng.IncorrectSysTopic|The system topic is not correct.|
|iot.messagebroker.NullTopicName|Topic name cannot be empty.|

|Error code|Description|
|:---------|:----------|
|iot.ruleng.NullOtsInstanceName|The name of the Table Store instance cannot be empty.|
|iot.ruleng.NullTableNameInOtsInstance|The table name of the Table Store instance cannot be empty.|
|iot.ruleng.NullPrimaryKeyInOtsTable|Primary key of the table cannot not be empty.|
|iot.ruleng.NullPrimaryKeyNameInOts|The name of the primary key cannot be empty.|
|iot.ruleng.NullPrimaryKeyValueInOts|The value of the primary key cannot be empty.|
|iot.ruleng.IncorrectPrimaryKeyValueInOtsTable|The value of the primary key is incorrect.|

|Error code|Description|
|:---------|:----------|
|iot.ruleng.NullTopicNameInMns|Name of the target theme cannot be empty.|
|iot.ruleng.NotFoundTopicInMns|The specified theme name is not found.|
|iot.ruleng.QueryMnsTopicListActionError|Failed to query the theme list in Message Service.|

|Error code|Description|
|:---------|:----------|
|iot.ruleng.NullServiceNameInFc|The service name in Function Compute cannot be empty.|
|iot.ruleng.NullFunctionNameInFc|The function name in Function Compute cannot be empty.|
|iot.ruleng.NotFoundServiceInFc|The specified service is not found in Function Compute.|

|Error code|Description|
|:---------|:----------|
|iot.messagebroker.NullTopicName|The target topic in Message Queue cannot be empty.|

