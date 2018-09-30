# COTA 远程配置 {#concept_qwg_j4n_hfb .concept}

使用该功能前，需在云端开启产品的[远程配置](../../../../cn.zh-CN/用户指南/扩展服务/远程配置.md#)功能。远程配置可以用于更新设备的配置信息，包括设备的系统参数、网络参数或者本地策略等等。

**说明：** 远程配置相关接口参见 设备[IDeviceCOTA](http://gaic.alicdn.com/ztms/android-iot-device-sdk-api-reference-v2/html/com/aliyun/alink/dm/api/IDeviceCOTA.html)。

## 数据上行 {#section_pk2_z4n_hfb .section}

设备端主动获取远程配置信息

```
String COTA_get = "{" + "  \"id\": 123," + "  \"version\": \"1.0\"," +
        "  \"params\": {" + "\"configScope\": \"product\"," + "\"getType\": \"file\"" +
        "  }," + "  \"method\": \"thing.config.get\"" + "}";

RequestModel<Map> requestModel = JSONObject.parseObject(COTA_get, new TypeReference<RequestModel<Map>>() {
    }.getType());
LinkKit.getInstance().getDeviceCOTA().COTAGet(requestModel, new IConnectSendListener() {
    @Override
    public void onResponse(ARequest aRequest, AResponse aResponse) {
        // 获取远程配置结果成功
    }

    @Override
    public void onFailure(ARequest aRequest, AError aError) {
        // 获取远程配置失败
    }
});
```

## 数据下行 {#section_wcf_dpn_hfb .section}

设备端通过订阅获取远程配置信息

```
// 注意：云端目前有做限制，一个小时只能全品类下发一次 COTA 更新
LinkKit.getInstance().getDeviceCOTA().setCOTAChangeListener(new IConnectRrpcListener() {
    @Override
    public void onSubscribeSuccess(ARequest aRequest) {
        // 订阅成功
    }

    @Override
    public void onSubscribeFailed(ARequest aRequest, AError aError) {
        // 订阅失败
    }

    @Override
    public void onReceived(ARequest aRequest, IConnectRrpcHandle iConnectRrpcHandle) {
        // 接收到远程配置下行数据    
        if (aRequest instanceof MqttRrpcRequest){
            // 云端下行数据 返回数据示例参见 Demo
            // ((MqttRrpcRequest) aRequest).payloadObj;
        }
    }

    @Override
    public void onResponseSuccess(ARequest aRequest) {
        // 回复远程配置成功
    }

    @Override
    public void onResponseFailed(ARequest aRequest, AError aError) {
        // 回复远程配置失败
    }
});
```

