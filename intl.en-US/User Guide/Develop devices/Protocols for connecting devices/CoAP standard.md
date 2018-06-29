# CoAP standard {#concept_tcf_yq5_wdb .concept}

## Protocol version {#section_rpv_zq5_wdb .section}

IoT Platform supports the Constrained Application Protocol \(CoAP\) \[RFC7252\]. For more information, see [RFC 7252](http://tools.ietf.org/html/rfc7252).

## Channel security {#section_gvl_br5_wdb .section}

IoT Platform uses Datagram Transport Layer Security \(DTLS\) V1.2 to secure channels. For more information, see [DTLS v1.2](https://tools.ietf.org/html/rfc6347).

## Open-source client reference {#section_fc5_cr5_wdb .section}

For more information, see [http://coap.technology/impls.html](http://coap.technology/impls.html).

**Note:** If you use third-party code, Alibaba Cloud does not provide technical support.

## Alibaba Cloud CoAP agreement {#section_m5z_fr5_wdb .section}

-   Do not use a question mark \(?\) to set a parameter.
-   Resource discovery is not supported.
-   Only the User Datagram Protocol \(UDP\) is supported, and DTLS must be used.
-   Follow the Uniform Resource Identifier \(URI\) standard, and keep CoAP URI resources consistent with Message Queuing Telemetry Transport \(MQTT\)-based topics. For more information, see [MQTT standard](https://help.aliyun.com/document_detail/30540.html).

