# Set desired property values to control the light bulb status {#concept_p3z_p12_zgb .concept}

You can set a desired property value for a device on the cloud. IoT Platform caches the value and pushes the value to the device when it is online .

## Background information {#section_dk2_bb2_zgb .section}

A light bulb does not have storage capability. If you want to control the light bulb from IoT Platform, the light bulb must stay online always. However, the light bulb is rarely online.

To resolve this issue, IoT Platform allows you to set desired property values.

When the light bulb is connected to IoT Platform, it reads the desired property value stored in IoT Platform. The device then updates the actual property based on the desired value and reports the updated property to IoT Platform. The reported property value will be stored as the latest property value.

## Create a product and a device in the IoT Platform console {#section_kld_t22_zgb .section}

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  Create a product named **Street Lamp** and a light bulb device named **Lamp**.
3.  Go to the **Device Details** page of the device, and click the **Status** tab.

    For a newly created device, the latest value and the desired value of a property are both "null", and the desired value version is 0.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134519/155703987139879_en-US.png)

4.  Record the device certificate information, including ProductKey, DeviceName, and DeviceSecret, and the region information \(RegionId\).

## Develop on the cloud {#section_jpc_cg2_zgb .section}

You call the related APIs to obtain the latest property values of a device and set desired property values to control the device.

For more information, see [API reference \(cloud\)](../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Overview.md#). This example uses the [Java SDK \(cloud\)](../../../../reseller.en-US/Developer Guide (Cloud)/SDK reference/Java SDK.md#).

-   Call QueryDeviceDesiredProperty to query the desired property values of the device.

    ```
    DefaultProfile profile = DefaultProfile.getProfile(
            "<your-region-id>", //Your region ID
            "<your-access-key-id>", //AccessKey ID of your Alibaba Cloud account
            "<your-access-key-secret>"); //AccessKey Secret of your Alibaba Cloud account
    IAcsClient client = new DefaultAcsClient(profile);
    
    //Set parameters to call the API
    QueryDeviceDesiredPropertyRequest request = new QueryDeviceDesiredPropertyRequest();
    request.setProductKey(productKey);
    request.setDeviceName(deviceName);
    //The list of property identifiers that you want to query If you do not specify the property identifier list, the desired values of all properties except the read-only properties will be queried.
    request.setIdentifiers(Arrays.asList("LightStatus"));
    
    //Initiate the request and handle the response or exception
    QueryDeviceDesiredPropertyResponse response;
    try {
        response = client.getAcsResponse(request);
    
        QueryDeviceDesiredPropertyResponse.Data data = response.getData();
        for (QueryDeviceDesiredPropertyResponse.Data.DesiredPropertyInfo propInfo :
                data.getList()) {
            //Return the desired value and version of each property, such as property=LightStatus desired=1 version=5
            System.out.println("property=" + propInfo.getIdentifier());
            System.out.println("desired=" + propInfo.getValue());
            System.out.println("version=" + propInfo.getBizVersion());
        }
    } catch (ServerException e) {
        e.printStackTrace();
    } catch (ClientException e) {
        e.printStackTrace();
    }
    ```

-   Call SetDeviceDesiredProperty to set desired property values for a device.

    ```
    DefaultProfile profile = DefaultProfile.getProfile(
            "<your-region-id>", //Your region ID
            "<your-access-key-id>",     //AccessKey ID of your Alibaba Cloud account
            "<your-access-key-secret>"); //AccessKey Secret of your Alibaba Cloud account
    IAcsClient client = new DefaultAcsClient(profile);
    
    //Set parameters to call the API
    SetDeviceDesiredPropertyRequest request = new SetDeviceDesiredPropertyRequest();
    request.setProductKey(productKey);
    request.setDeviceName(deviceName);
    //The identifiers and values of the properties that you want to set.
    request.setItems("{\"LightStatus\": 1}");
    //You can set the version of a desired property value if other RAM users also set desired values for the property for distinguishment. Typically, you can choose not to specify the version.
    request.setVersions("{}");
    
    //Initiate the request and handle the response or exception.
    SetDeviceDesiredPropertyResponse response;
    try {
        response = client.getAcsResponse(request);
    
        SetDeviceDesiredPropertyResponse.Data data = response.getData();
        //If the operation is successful, the version of the desired property value is returned, such as {"LightStatus":5}
        System.out.println(data.getVersions());
    } catch (ServerException e) {
        e.printStackTrace();
    } catch (ClientException e) {
        e.printStackTrace();
    }
    ```


## Develop on the device {#section_osk_jf2_zgb .section}

When the light bulb is connected to IoT Platform, it requests the desired property values from IoT Platform. If a desired property value changes while the light bulb is on, IoT Platform pushes the change in real time to the light bulb.

For more information, see [Developer Guide \(Devices\)](https://www.alibabacloud.com/help/product/93051.htm). This example uses the Java SDK.

The last section of this topic provides a complete set of device demo code for your reference.

1.  Enter the device certificate and region information.

    ```
    /**
    * ProductKey, DeviceName, and DeviceSecret
    */
    private static String productKey = "******";
    private static String deviceName = "********";
    private static String deviceSecret = "**************";
    /**
    * MQTT connection information
    */
    private static String productKey = "******";
    ```

2.  Add the following methods. These methods can be used to change the actual property value of the light bulb and automatically report the changes to IoT Platform. The latest property value in IoT Platform will be replaced.

    ```
    /**
     * When the device handles a property change, the methods will be called in the following scenarios:
     * Scenario 1 Pull mode: After the device is connected to IoT Platform, the device automatically requests the latest desired property value from IoT Platform.
     * Scenario 2 Push mode: While the device is online, it receives the desired property value that IoT Platform pushes to the topic property.set.
     * @param identifier   The property identifier
     * @param value       The desired property value
     * @param needReport  Indicates whether to subscribe to the  topic property.post to send the property value to IoT Platform
     * In scenario 2, the property report capability is already integrated into the processing function and the needReport parameter will be set to "false".
     * @return
     */
    private boolean handlePropertySet(String identifier, ValueWrapper value, boolean needReport) {
        ALog.d(TAG, "The device handled property changes= [" + identifier + "], value = [" + value + "]");
        //Identify whether the property settings are successful according to the response. In this example, a success message is returned.
        boolean success = true;
        if (needReport) {
            reportProperty(identifier, value);
        }
        return success;
    }
    
    private void reportProperty(String identifier, ValueWrapper value){
        if (StringUtils.isEmptyString(identifier) || value == null) {
            return;
        }
    
        ALog.d(TAG, "Reported property identity=" + identifier);
    
        Map<String, ValueWrapper> reportData = new HashMap<>();
        reportData.put(identifier, value);
        LinkKit.getInstance().getDeviceThing().thingPropertyPost(reportData, new IPublishResourceListener() {
    
            public void onSuccess(String s, Object o) {
                //The property is reported
                ALog.d(TAG, "Report success onSuccess() called with: s = [" + s + "], o = [" + o + "]");
            }
    
            public void onError(String s, AError aError) {
                //The property fails to be reported
                ALog.d(TAG, "Report failure onError() called with: s = [" + s + "], aError = [" + JSON.toJSONString(aError) + "]");
            }
        });
    }
    ```

3.  When the light bulb is online, if a desired property value is set for the light bulb in IoT Platform, the value will be pushed to the light bulb. The light bulb then processes the message and changes the status accordingly.

    If the device is offline when the desired value is set, use the `connectNotifyListener` method to register a function to respond to service calls and property settings. For information about the Alink protocol, see [Devices report properties](../../../../reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Device properties, events, and services.md#section_g4j_5zg_12b).

    After the device receives an asynchronous message from IoT Platform, the device calls the `mCommonHandler` method and then the `handlePropertySet` method to update the property value.

    ```
    /**
     * Register a function to respond to service calls and property settings.
     * When IoT Platform calls a service from the device, the device must respond to the call and return a response.
     */
    public void connectNotifyListener() {
        List<Service> serviceList = LinkKit.getInstance().getDeviceThing().getServices();
        for (int i = 0; serviceList ! = null && i < serviceList.size(); i++) {
            Service service = serviceList.get(i);
            LinkKit.getInstance().getDeviceThing().setServiceHandler(service.getIdentifier(), mCommonHandler);
        }
    }
    
    private ITResRequestHandler mCommonHandler = new ITResRequestHandler() {
        public void onProcess(String serviceIdentifier, Object result, ITResResponseCallback itResResponseCallback) {
            ALog.d(TAG, "onProcess() called with: s = [" + serviceIdentifier + "]," +
                    " o = [" + result + "], itResResponseCallback = [" + itResResponseCallback + "]");
            ALog.d(TAG, "Received an asynchronous service call from IoT Platform " + serviceIdentifier);
            try {
                if (SERVICE_SET.equals(serviceIdentifier)) {
                    Map<String, ValueWrapper> data = (Map<String, ValueWrapper>)((InputParams)result).getData();
                    ALog.d(TAG, "Received asynchronous downstream data " + data);
                    //Set the property value of the device and then report the property value to IoT Platform.
                    boolean isSetPropertySuccess =
                            handlePropertySet("LightStatus", data.get("LightStatus"), false);
                    if (isSetPropertySuccess) {
                        if (result instanceof InputParams) {
                            //Return a response to IoT Platform
                            itResResponseCallback.onComplete(serviceIdentifier, null, null);
                        } else {
                            itResResponseCallback.onComplete(serviceIdentifier, null, null);
                        }
                    } else {
                        AError error = new AError();
                        error.setCode(100);
                        error.setMsg("setPropertyFailed.");
                        itResResponseCallback.onComplete(serviceIdentifier, new ErrorInfo(error), null);
                    }
                } else if (SERVICE_GET.equals(serviceIdentifier)) {
                } else {
                    //The device operation varies by service.
                    ALog.d(TAG, "The returned values corresponding to the service.");
                    OutputParams outputParams = new OutputParams();
                    // outputParams.put("op", new ValueWrapper.IntValueWrapper(20));
                    itResResponseCallback.onComplete(serviceIdentifier, null, outputParams);
                }
            } catch (Exception e) {
                e.printStackTrace();
                ALog.d(TAG, "The data returned from IoT Platform is incorrectly formatted");
            }
        }
    
        public void onSuccess(Object o, OutputParams outputParams) {
            ALog.d(TAG, "onSuccess() called with: o = [" + o + "], outputParams = [" + outputParams + "]");
            ALog.d(TAG, "The service has been registered");
        }
    
        public void onFail(Object o, ErrorInfo errorInfo) {
            ALog.d(TAG, "onFail() called with: o = [" + o + "], errorInfo = [" + errorInfo + "]");
            ALog.d(TAG, "Service registration failed.");
        }
    };
    ```

4.  When the light bulb is offline, if a desired property value is set for the light bulb in IoT Platform, this value will be stored in IoT Platform.

    After the light bulb comes online again, it will request for the desired property value from IoT Platform and call the `handlePropertySet` method to update the property value.

    ```
    LinkKit.getInstance().init(params, new ILinkKitConnectListener() {
        public void onError(AError aError) {
            ALog.e(TAG, "Init Error error=" + aError);
        }
        public void onInitDone(InitResult initResult) {
            ALog.i(TAG, "onInitDone result=" + initResult);
    
            connectNotifyListener();
    
            //Obtain the latest desired property value
            getDesiredProperty(deviceInfo, Arrays.asList("LightStatus"), new IConnectSendListener() {
                public void onResponse(ARequest aRequest, AResponse aResponse) {
                    if(aRequest instanceof MqttPublishRequest && aResponse.data ! = null) {
                        JSONObject jsonObject = JSONObject.parseObject(aResponse.data.toString());
                        ALog.i(TAG, "onResponse result=" + jsonObject);
                        JSONObject dataObj = jsonObject.getJSONObject("data");
                        if (dataObj ! = null) {
                            if (dataObj.getJSONObject("LightStatus") == null) {
                                //No desired value is set
                            } else {
                                Integer value = dataObj.getJSONObject("LightStatus").getInteger("value");
                                handlePropertySet("LightStatus", new ValueWrapper.IntValueWrapper(value), true);
                            }
                        }
                    }
                }
                public void onFailure(ARequest aRequest, AError aError) {
                    ALog.d(TAG, "onFailure() called with: aRequest = [" + aRequest + "], aError = [" + aError + "]");
                }
            });
        }
    });
    
    private void getDesiredProperty(BaseInfo info, List<String> properties, IConnectSendListener listener) {
        ALog.d(TAG, "getDesiredProperty() called with: info = [" + info + "], listener = [" + listener + "]");
        if(info ! = null && ! StringUtils.isEmptyString(info.productKey) && ! StringUtils.isEmptyString(info.deviceName)) {
            MqttPublishRequest request = new MqttPublishRequest();
            request.topic = DESIRED_PROPERTY_GET.replace("{productKey}", info.productKey).replace("{deviceName}", info.deviceName);
            request.replyTopic = DESIRED_PROPERTY_GET_REPLY.replace("{productKey}", info.productKey).replace("{deviceName}", info.deviceName);
            request.isRPC = true;
            RequestModel<List<String>> model = new RequestModel<>();
            model.id = String.valueOf(IDGeneraterUtils.getId());
            model.method = METHOD_GET_DESIRED_PROPERTY;
            model.params = properties;
            model.version = "1.0";
            request.payloadObj = model.toString();
            ALog.d(TAG, "getDesiredProperty: payloadObj=" + request.payloadObj);
            ConnectSDK.getInstance().send(request, listener);
        } else {
            ALog.w(TAG, "getDesiredProperty failed, baseInfo Empty.");
            if(listener ! = null) {
                AError error = new AError();
                error.setMsg("BaseInfoEmpty.");
                listener.onFailure(null, error);
            }
        }
    }
    ```


## Verify the configuration {#section_tk5_dg2_zgb .section}

Verify that you can change the light bulb status by setting the desired value in IoT Platform regardless of whether the light bulb is online or not.

-   When the light bulb is online, you change the light bulb status in IoT Platform and the light bulb responds to the change in real time.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134519/155703987139883_en-US.png)

-   When the light bulb is offline, you change the light bulb status in IoT Platform. As a result, the desired value and the latest property value displayed in IoT Platform console are different.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134519/155703987139884_en-US.png)

-   After the light bulb comes online again, the light bulb requests the desired property value. The latest property value will be immediately synchronized from the desired value.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134519/155703987139885_en-US.png)


Conslusion: To change the light bulb status, you can simply modify the desired property value. You can use the desired value to control the status of the light bulb that has no storage capability.

## SDK demo code {#section_qyj_wb2_zgb .section}

```
package com.aliyun.alink.devicesdk.demo;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.aliyun.alink.apiclient.utils.StringUtils;
import com.aliyun.alink.dm.api.BaseInfo;
import com.aliyun.alink.dm.api.DeviceInfo;
import com.aliyun.alink.dm.api.InitResult;
import com.aliyun.alink.dm.model.RequestModel;
import com.aliyun.alink.dm.utils.IDGeneraterUtils;
import com.aliyun.alink.linkkit.api.ILinkKitConnectListener;
import com.aliyun.alink.linkkit.api.IoTMqttClientConfig;
import com.aliyun.alink.linkkit.api.LinkKit;
import com.aliyun.alink.linkkit.api.LinkKitInitParams;
import com.aliyun.alink.linksdk.cmp.api.ConnectSDK;
import com.aliyun.alink.linksdk.cmp.connect.channel.MqttPublishRequest;
import com.aliyun.alink.linksdk.cmp.core.base.ARequest;
import com.aliyun.alink.linksdk.cmp.core.base.AResponse;
import com.aliyun.alink.linksdk.cmp.core.listener.IConnectSendListener;
import com.aliyun.alink.linksdk.tmp.api.InputParams;
import com.aliyun.alink.linksdk.tmp.api.OutputParams;
import com.aliyun.alink.linksdk.tmp.device.payload.ValueWrapper;
import com.aliyun.alink.linksdk.tmp.devicemodel.Service;
import com.aliyun.alink.linksdk.tmp.listener.IPublishResourceListener;
import com.aliyun.alink.linksdk.tmp.listener.ITResRequestHandler;
import com.aliyun.alink.linksdk.tmp.listener.ITResResponseCallback;
import com.aliyun.alink.linksdk.tmp.utils.ErrorInfo;
import com.aliyun.alink.linksdk.tools.AError;
import com.aliyun.alink.linksdk.tools.ALog;

import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class LampDemo {
    private static final String TAG = "LampDemo";

    private final static String SERVICE_SET = "set";
    private final static String SERVICE_GET = "get";

    public static String DESIRED_PROPERTY_GET = "/sys/{productKey}/{deviceName}/thing/property/desired/get";
    public static String DESIRED_PROPERTY_GET_REPLY = "/sys/{productKey}/{deviceName}/thing/property/desired/get_reply";
    public static String METHOD_GET_DESIRED_PROPERTY = "thing.property.desired.get";

    public static void main(String[] args) {
        /**
         * ProductKey, DeviceName, and DeviceSecret
         */
        String productKey = "****";
        String deviceName = "Lamp";
        String deviceSecret = "****";
        /**
         * MQTT connection information
         */
        String regionId = "cn-shanghai";

        LampDemo manager = new LampDemo();

        DeviceInfo deviceInfo = new DeviceInfo();
        deviceInfo.productKey = productKey;
        deviceInfo.deviceName = deviceName;
        deviceInfo.deviceSecret = deviceSecret;

        manager.init(deviceInfo, regionId);
    }

    public void init(final DeviceInfo deviceInfo, String region) {
        LinkKitInitParams params = new LinkKitInitParams();
        /**
         * Set the MQTT initialization parameters
         */
        IoTMqttClientConfig config = new IoTMqttClientConfig();
        config.productKey = deviceInfo.productKey;
        config.deviceName = deviceInfo.deviceName;
        config.deviceSecret = deviceInfo.deviceSecret;
        config.channelHost = deviceInfo.productKey + ".iot-as-mqtt." + region + ".aliyuncs.com:1883";
        /**
         * Indicates whether to receive offline messages
         * The cleanSession field corresponding to the MQTT connection
         */
        config.receiveOfflineMsg = false;
        params.mqttClientConfig = config;

        /**
         * Pass in the device certificate information for initialization
         */
        params.deviceInfo = deviceInfo;

        LinkKit.getInstance().init(params, new ILinkKitConnectListener() {
            public void onError(AError aError) {
                ALog.e(TAG, "Init Error error=" + aError);
            }
            public void onInitDone(InitResult initResult) {
                ALog.i(TAG, "onInitDone result=" + initResult);

                connectNotifyListener();

                //Obtain the latest desired property value from IoT Platform
                getDesiredProperty(deviceInfo, Arrays.asList("LightStatus"), new IConnectSendListener() {
                    public void onResponse(ARequest aRequest, AResponse aResponse) {
                        if(aRequest instanceof MqttPublishRequest && aResponse.data ! = null) {
                            JSONObject jsonObject = JSONObject.parseObject(aResponse.data.toString());
                            ALog.i(TAG, "onResponse result=" + jsonObject);
                            JSONObject dataObj = jsonObject.getJSONObject("data");
                            if (dataObj ! = null) {
                                if (dataObj.getJSONObject("LightStatus") == null) {
                                    //No desired value is set
                                } else {
                                    Integer value = dataObj.getJSONObject("LightStatus").getInteger("value");
                                    handlePropertySet("LightStatus", new ValueWrapper.IntValueWrapper(value), true);
                                }
                            }
                        }
                    }
                    public void onFailure(ARequest aRequest, AError aError) {
                        ALog.d(TAG, "onFailure() called with: aRequest = [" + aRequest + "], aError = [" + aError + "]");
                    }
                });
            }
        });
    }

    /**
     * When the device handles a property change, the methods will be called in the following scenarios:
     * Scenario 1. Pull mode: After the device is connected to IoT Platform again, the device automatically requests the latest desired property value from IoT Platform.
     * Scenario 2. Push mode: While the device is online, it receives the desired property value that IoT Platform pushes to the property.set topic.
     * @param identifier  The property identifier
     * @param value       The desired property value
     * @param needReport  Indicates whether to subscribe to the property.post topic to send the property value to IoT Platform
     * In scenario 2, the property report capability is already integrated into the processing function and the needReport parameter will be set to "false".
     * @return
     */
    private boolean handlePropertySet(String identifier, ValueWrapper value, boolean needReport) {
        ALog.d(TAG, "The device handled property changes= [" + identifier + "], value = [" + value + "]");
        //Identify whether the property settings are successful according to the response. In this example, a success message is returned.
        boolean success = true;
        if (needReport) {
            reportProperty(identifier, value);
        }
        return success;
    }

    private void reportProperty(String identifier, ValueWrapper value){
        if (StringUtils.isEmptyString(identifier) || value == null) {
            return;
        }

        ALog.d(TAG, "Reported property identity=" + identifier);

        Map<String, ValueWrapper> reportData = new HashMap<>();
        reportData.put(identifier, value);
        LinkKit.getInstance().getDeviceThing().thingPropertyPost(reportData, new IPublishResourceListener() {

            public void onSuccess(String s, Object o) {
                //The property is reported
                ALog.d(TAG, "Report success onSuccess() called with: s = [" + s + "], o = [" + o + "]");
            }

            public void onError(String s, AError aError) {
                //The property fails to be reported
                ALog.d(TAG, "Report failure onError() called with: s = [" + s + "], aError = [" + JSON.toJSONString(aError) + "]");
            }
        });
    }

    /**
     * Register a function to respond to service calls and property settings.
     * When IoT Platform calls a service from the device, the device must respond to the call and return a response.
     */
    public void connectNotifyListener() {
        List<Service> serviceList = LinkKit.getInstance().getDeviceThing().getServices();
        for (int i = 0; serviceList ! = null && i < serviceList.size(); i++) {
            Service service = serviceList.get(i);
            LinkKit.getInstance().getDeviceThing().setServiceHandler(service.getIdentifier(), mCommonHandler);
        }
    }

    private ITResRequestHandler mCommonHandler = new ITResRequestHandler() {
        public void onProcess(String serviceIdentifier, Object result, ITResResponseCallback itResResponseCallback) {
            ALog.d(TAG, "onProcess() called with: s = [" + serviceIdentifier + "]," +
                    " o = [" + result + "], itResResponseCallback = [" + itResResponseCallback + "]");
            ALog.d(TAG, "Received an asynchronous service call from IoT Platform " + serviceIdentifier);
            try {
                if (SERVICE_SET.equals(serviceIdentifier)) {
                    Map<String, ValueWrapper> data = (Map<String, ValueWrapper>)((InputParams)result).getData();
                    ALog.d(TAG, "Received asynchronous downstream data " + data);
                    //Set the property of the device and then report the property value to IoT Platform.
                    boolean isSetPropertySuccess =
                            handlePropertySet("LightStatus", data.get("LightStatus"), false);
                    if (isSetPropertySuccess) {
                        if (result instanceof InputParams) {
                            //Return a response to IoT Platform
                            itResResponseCallback.onComplete(serviceIdentifier, null, null);
                        } else {
                            itResResponseCallback.onComplete(serviceIdentifier, null, null);
                        }
                    } else {
                        AError error = new AError();
                        error.setCode(100);
                        error.setMsg("setPropertyFailed.");
                        itResResponseCallback.onComplete(serviceIdentifier, new ErrorInfo(error), null);
                    }
                } else if (SERVICE_GET.equals(serviceIdentifier)) {
                } else {
                    //The device operation varies by service.
                    ALog.d(TAG, "The returned values corresponding to the service.");
                    OutputParams outputParams = new OutputParams();
                    // outputParams.put("op", new ValueWrapper.IntValueWrapper(20));
                    itResResponseCallback.onComplete(serviceIdentifier, null, outputParams);
                }
            } catch (Exception e) {
                e.printStackTrace();
                ALog.d(TAG, "The data returned from IoT Platform is incorrectly formatted");
            }
        }

        public void onSuccess(Object o, OutputParams outputParams) {
            ALog.d(TAG, "onSuccess() called with: o = [" + o + "], outputParams = [" + outputParams + "]");
            ALog.d(TAG, "The service was registered");
        }

        public void onFail(Object o, ErrorInfo errorInfo) {
            ALog.d(TAG, "onFail() called with: o = [" + o + "], errorInfo = [" + errorInfo + "]");
            ALog.d(TAG, "Service registration failed.");
        }
    };

    private void getDesiredProperty(BaseInfo info, List<String> properties, IConnectSendListener listener) {
        ALog.d(TAG, "getDesiredProperty() called with: info = [" + info + "], listener = [" + listener + "]");
        if(info ! = null && ! StringUtils.isEmptyString(info.productKey) && ! StringUtils.isEmptyString(info.deviceName)) {
            MqttPublishRequest request = new MqttPublishRequest();
            request.topic = DESIRED_PROPERTY_GET.replace("{productKey}", info.productKey).replace("{deviceName}", info.deviceName);
            request.replyTopic = DESIRED_PROPERTY_GET_REPLY.replace("{productKey}", info.productKey).replace("{deviceName}", info.deviceName);
            request.isRPC = true;
            RequestModel<List<String>> model = new RequestModel<>();
            model.id = String.valueOf(IDGeneraterUtils.getId());
            model.method = METHOD_GET_DESIRED_PROPERTY;
            model.params = properties;
            model.version = "1.0";
            request.payloadObj = model.toString();
            ALog.d(TAG, "getDesiredProperty: payloadObj=" + request.payloadObj);
            ConnectSDK.getInstance().send(request, listener);
        } else {
            ALog.w(TAG, "getDesiredProperty failed, baseInfo Empty.");
            if(listener ! = null) {
                AError error = new AError();
                error.setMsg("BaseInfoEmpty.");
                listener.onFailure(null, error);
            }
        }
    }
}
```

