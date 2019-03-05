# 什么是RRPC {#concept_zlp_gsl_cfb .concept}

MQTT协议是基于PUB/SUB的异步通信模式，不适用于服务端同步控制设备端返回结果的场景。物联网平台基于MQTT协议制定了一套请求和响应的同步机制，无需改动MQTT协议即可实现同步通信。物联网平台提供API给服务端，设备端只需要按照固定的格式回复PUB消息，服务端使用API，即可同步获取设备端的响应结果。

## 名词解释 {#section_lkn_qsl_cfb .section}

-   RRPC：远程同步调用。
-   RRPC 请求消息：云端下发给设备端的消息。
-   RRPC 响应消息：设备端回复给云端的消息。
-   RRPC 消息id：云端为每次RRPC调用生成的唯一消息id。
-   RRPC 订阅Topic：设备端订阅RRPC消息时传递的Topic，含有通配符。

## RRPC原理 {#section_yf1_rsl_cfb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21225/155175346611774_zh-CN.png)

1.  物联网平台收到来自用户服务器的RRPC调用，下发一条RRPC请求消息给设备。消息体为用户传入的数据，Topic为物联网平台定义的Topic，其中含有唯一的RRPC消息ID。
2.  设备收到下行消息后，按照指定Topic格式（包含之前云端下发的唯一的RRPC消息id）回复一条RRPC响应消息给云端，云端提取出Topic中的消息ID，和之前的RRPC请求消息匹配上，然后回复给用户服务器。
3.  如果调用时设备不在线，云端会给用户服务器返回设备离线的错误；如果设备没有在超时时间内（5秒内）回复RRPC响应消息，云端会给用户服务器返回超时错误。

## Topic格式 {#section_zpj_tsl_cfb .section}

不同Topic格式使用方法不同。

-   系统Topic使用方法参见[系统Topic](intl.zh-CN/用户指南/RRPC/系统Topic.md#)。
-   自定义Topic使用方法参见[自定义Topic](intl.zh-CN/用户指南/RRPC/自定义Topic.md#)。

