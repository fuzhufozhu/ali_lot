# Use the advanced features {#concept_187338 .concept}

This topic describes how to use the advanced features of the generic protocol SDK. The advanced features include customizing the configuration file path, configuring dynamic bridge registration, calling the data reporting interfaces encapsulated in the generic protocol SDK to report properties, events, and tags.

## Customize configurations {#section_hxt_r8a_r6r .section}

By default, the configuration file of a bridge device and the mapping configuration file of the device certificate are read from application.conf and devices.conf, respectively, under a fixed path. The generic protocol SDK allows you to customize configurations. Before you call bootstrap, call the ConfigFactory.init method to customize the path of a configuration file. You can also customize an instance to implement the corresponding interface.

Sample code to customize configurations:

``` {#codeblock_4c0_4ok_5rh}
//Define config
//You can specify the location path of config files 
//or you can create an instance and implement the corresponding interface
//Config.init() must be called before bridgeBootstrap.bootstrap()
ConfigFactory.init(
    ConfigFactory.getBridgeConfigManager("application-self-define.conf"),
    selfDefineDeviceConfigManager);
bridgeBootstrap.bootstrap();

private static DeviceConfigManager selfDefineDeviceConfigManager = new DeviceConfigManager() {
    @Override
    public DeviceIdentity getDeviceIdentity(String originalIdentity) {
        //Suppose you dynamically get deviceInfo in other ways
        return devicesMap.get(originalIdentity);
    }

    @Override
    public String getOriginalIdentity(String productKey, String deviceName) {
        //you can ignore this
        return null;
    }
};
```

## Dynamically register a bridge device {#section_xmx_dyi_nok .section}

When you need to deploy a bridge application on a large number of servers, it is cumbersome to specify different bridge devices for different bridge servers. You can configure the bridge information file application.conf to dynamically register bridge devices with IoT Platform. You must provide the productKey and popClientProfile parameters in the configuration file. The generic protocol SDK will call the IoT Platform API and use the bridge servers' MAC codes as the device names to register bridge devices.

**Note:** 

-   To dynamically register bridge devices, you only need to modify the bridge configuration file. The call code is the same as [Use the basic features](reseller.en-US/User Guide/General protocols/Use the basic features.md#).
-   If the bridge information is already specified in the bridge configuration file, no device is created. The generic protocol SDK calls the IoT Platform API and uses the bridge server's MAC code as the device name to register a bridge device only if the following conditions are met: The deviceName and deviceSecret parameters are left empty in the configuration file; all parameters in popClientprofile are specified. If a device is already registered using the current MAC code, the device is directly used as the bridge device.
-   If a bridge is configured by using this method, we recommend that you do not perform debugging on a local client by using the configurations for the production environment. Each time the program is debugged on a local client, the generic protocol SDK uses the MAC code of the client to register a bridge device, and associates all devices in the device configuration file devices.conf with the bridge. We recommend that you use dedicated devices for testing to perform debugging to avoid interference with the production environment.

|Parameter|Required|Description|
|:--------|:-------|:----------|
|productKey|Yes|The ProductKey of the product to which the bridge device belongs.|
|http2Endpoint|Yes| The endpoint of the HTTP/2 gateway service. The bridge device and IoT Platform establish a persistent connection over the HTTP/2 protocol. The endpoint is in the format of `${productKey}.iot-as-http2 .${RegionId}.aliyuncs.com:443`.

 Replace $\{productKey\} with the ProductKey of the product to which your bridge device belongs.

 Replace $\{RegionId\} with the ID of the region where your service is located. For more information about regions, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

 For example, if the ProductKey of the bridge device is alabcabc123, the region is China \(Shanghai\), then the HTTP/2 gateway service endpoint is `alabcabc123.iot-as-http2.cn-shanghai.aliyuncs.com:443`.

 |
|authEndpoint|Yes| The service URL for device authentication. The device authentication service URL is in the format of `https://iot-auth .${RegionId}.aliyuncs.com/auth/bridge`.

 Replace $\{RegionId\} with the ID of the region where your service is located. For more information about regions, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

 For example, if the region is China \(Shanghai\), then the device authentication service address is `https://iot-auth.cn-shanghai.aliyuncs.com/auth/bridge`.

 |
|popClientProfile|Yes|After this parameter is configured, the generic protocol SDK calls the IoT Platform API to automatically register bridge devices. For more information, see the following table: Parameters in popClientProfile.

 |

|Parameter|Required|Description|
|:--------|:-------|:----------|
|accessKey|Yes|The AccessKey ID of your Alibaba Cloud account. Log on to the Alibaba Cloud console and click your account avatar to go to the Account Management page. You can create or view the AccessKey information.

 |
|accessSecret|Yes|The AccessKey Secret of your Alibaba Cloud account.|
|name|Yes|The IoT Platform service region to which the bridge device connects. This parameter indicates the region to which the product identified by productKey belongs. For more information about regions, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

 |
|region|Yes|The ID of the IoT Platform service region to which the bridge device connects. This parameter indicates the region to which the product identified by productKey belongs. This parameter is expressed in the same way as the name parameter.

 |
|product|Yes|The product name. Set the value to Iot.|
|endpoint|Yes|The endpoint of the API. The endpoint is in the format of `iot. ${RegionId}.aliyuncs.com`. Replace $\{RegionId\} with the ID of the region where your service is located. For more information about regions, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).

 For example, If the region is China \(Shanghai\), the endpoint is `iot.cn-shanghai.aliyuncs.com`.

 |

Sample code to dynamically register bridge devices:

``` {#codeblock_u6q_ee1_pnh}
# Server endpoint
http2Endpoint = "https://${YourProductKey}.iot-as-http2.cn-shanghai.aliyuncs.com:443"
authEndpoint = "https://iot-auth.cn-shanghai.aliyuncs.com/auth/bridge"

# Gateway device info
# You can also specify productKey only, and dynamic register deviceName & deviceSecret in runtime
productKey = ${YourProductKey}

# If you dynamic register gateway device using your mac address, you have to specify 'popClientProfile'
# otherwise you can ignore it
popClientProfile = {
    accessKey = ${YourAliyunAccessKey}
    accessSecret = ${YourAliyunAccessSecret}
    name = cn-shanghai
    region = cn-shanghai
    product = Iot
    endpoint = iot.cn-shanghai.aliyuncs.com
}
```

## Call interfaces to report TSL data {#section_fix_ifz_m15 .section}

To facilitate use and reduce your encapsulation operations, the generic protocol SDK encapsulates data reporting interfaces. They are reportProperty, fireEvent, and updateDeviceTag. The device can use these interfaces to report properties, report events, and update device tags.

Prerequisites and usage guidelines:

-   Before you call reportProperty and fireEvent to report properties and events, log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot) and go to the Product Details page of the corresponding product. Then, click the Define Feature tab and define properties and events. For more information, see [Define features](reseller.en-US/User Guide/Create products and devices/TSL/Define features.md#).
-   If the tag that is specified in updateDeviceTag already exists, the tag value is updated. If the tag does not exist, the tag is automatically created. To check the call result, you can log on to the IoT Platform console and go to the Device Details page of the corresponding device.

Sample code:

``` {#codeblock_3ue_hj5_7wc}
TslUplinkHandler tslUplinkHandler = new TslUplinkHandler();
//report property
//Property 'testProp' is defined in IoT Platform Web Console
String requestId = String.valueOf(random.nextInt(1000));
tslUplinkHandler.reportProperty(requestId, originalIdentity, "testProp", random.nextInt(100));

//fire event
//Event 'testEvent' is defined in IoT Platform Web Console
requestId = String.valueOf(random.nextInt(1000));
HashMap<String, Object> params = new HashMap<String, Object>();
params.put("testEventParam", 123);
tslUplinkHandler.fireEvent(originalIdentity, "testEvent", ThingEventTypes.INFO, params);

//update device tag
//'testDeviceTag' is a tag key defined in IoT Platform Web Console
requestId = String.valueOf(random.nextInt(1000));
tslUplinkHandler.updateDeviceTag(requestId, originalIdentity, "testDeviceTag", String.valueOf(random.nextInt(1000)));
```

The parameters in this example are described as follows:

|Parameter|Description|
|:--------|:----------|
|requestId|The request ID.|
|originalIdentity|The original identifier of the device.|
|testProp|The identifier of the property. For this example, make sure that you have defined a property with the identifier as testProp in the IoT Platform console. This sample code indicates to report the value of property testProp.|
|random.nextInt\(100\)|The property value to be reported. The value range of the property value is also defined in the IoT Platform console. In this example, use `random.nextInt(100)` to indicate a random number less than 100.|
|testEvent|The identifier of the event. For this example, make sure that you have defined an event with the identifier as testEvent in the IoT Platform console. This sample code indicates to report event testEvent.|
|ThingEventTypes.INFO|The event type. ThingEventTypes specifies the event type. A value of INFO indicates that the event type is Info. For this example, make sure that you have selected Info as the event type when you defined event testEvent in the IoT Platform console.

 |
|params|The output parameters of the event. The identifier, data type, and value range of output parameters are also defined in the IoT Platform console. In this example, the identifier of the output parameter is testEventParam, and the value is 123.|
|testDeviceTag|The key of the tag. The data type is String. In this example, the key is testDeviceTag. Set the key of the tag as instructed based on your requirements. For more information, see [Device tags](reseller.en-US/User Guide/Create products and devices/Tags.md#section_igy_bvb_wdb).|
|String.valueOf\(random.nextInt\(1000\)\)|The value of the tag. The data type is String. In this example, `String.valueOf(random.nextInt(1000))` indicates a random number less than 1000. Set the value of the tag as instructed based on your requirements. For more information, see [Device tags](reseller.en-US/User Guide/Create products and devices/Tags.md#section_igy_bvb_wdb).|

