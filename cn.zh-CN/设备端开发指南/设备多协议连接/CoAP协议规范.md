# CoAP协议规范 {#concept_tcf_yq5_wdb .concept}

## 协议版本 {#section_rpv_zq5_wdb .section}

支持 RFC 7252 Constrained Application Protocol协议，具体请参考：[RFC 7252](http://tools.ietf.org/html/rfc7252)。

## 通道安全 {#section_gvl_br5_wdb .section}

使用 DTLS v1.2保证通道安全，具体请参考：[DTLS v1.2](https://tools.ietf.org/html/rfc6347)。

## 开源客户端参考 {#section_fc5_cr5_wdb .section}

[http://coap.technology/impls.html](http://coap.technology/impls.html)。

**说明：** 若使用第三方代码, 阿里云不提供技术支持

## 阿里CoAP约定 {#section_m5z_fr5_wdb .section}

-   暂时不支持资源发现。
-   仅支持UDP协议，目前支持DTLS和对称加密两种安全模式。
-   URI规范，CoAP的URI资源和MQTT Topic保持一致，参考[MQTT规范](cn.zh-CN/设备端开发指南/设备多协议连接/MQTT协议规范.md#)。

## 说明与限制 {#section_f55_r4h_ffb .section}

-   Topic规范和MQTT Topic一致，CoAP协议内 `coap://host:port/topic/${topic}`接口对于所有`${topic}`和MQTT Topic可以复用。
-   客户端缓存认证返回的token是请求的令牌。
-   传输的数据大小依赖于MTU的大小，建议在1 KB以内。
-   仅华东2（上海）地域支持CoAP通信。

