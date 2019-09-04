# 通过服务端订阅（HTTP/2）获取设备状态 {#task_1715084 .task}

物联网很多业务场景中，时常需要获取设备的实时状态，以便根据不同的状态（在线或离线）做不同的处理。阿里云物联网平台提供服务端订阅功能来获取设备的状态信息。本文介绍通过HTTP/2通道订阅获取设备状态的方法

## 原理 {#section_91q_ki9_9zs .section}

在物联网平台控制台，设置服务端订阅设备状态变化通知消息后，物联网平台会将该产品下的设备上下线消息推送到您的服务端。服务端通过HTTP/2 SDK接收设备状态变化消息。

-   流程图

    ![服务端订阅](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1358792/156756270055747_zh-CN.png)

    **说明：** 步骤1.1和2.1物联网平台判断是否已配控制台已配置服务端订阅设备状态变化通知。

-   设备状态变化通知消息的数据格式

    ``` {#codeblock_gn5_xuz_i1y}
    {
        "status":"online|offline",
        "productKey":"al123455****",
        "deviceName":"deviceName1234",
        "time":"2018-08-31 15:32:28.205",
        "utcTime":"2018-08-31T07:32:28.205Z",
        "lastTime":"2018-08-31 15:32:28.195",
        "utcLastTime":"2018-08-31T07:32:28.195Z",
        "clientIp":"123.123.***.***"
    }
    ```


## 订阅 {#section_ovo_p0i_xz8 .section}

在物联网平台控制台，设置HTTP/2服务端订阅设备状态变化通知。

1.  登录[物联网平台控制台](https://iot.console.aliyun.com/product/region/cn-shanghai)。
2.  左侧导航栏选择**设备管理** \> **产品**。
3.  在产品列表中，搜索到要配置服务端订阅的产品，并单击该产品对应的**查看**按钮。
4.  在产品详情页，单击**服务端订阅** \> **设置**。
5.  选择推送的消息类型为**设备状态变化通知**，单击**保存**。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1358792/156756270055783_zh-CN.png)


## 接收 {#section_bi5_q0z_cnx .section}

服务端通过HTTP/2 SDK接收设备状态消息。本示例以配置Java HTTP/2 SDK为例。

**说明：** 

-   仅支持JDK8。
-   如果同一个阿里云账号信息用于启动了多个HTTP/2 SDK，物联网平台会将设备状态消息随机推送到其中一个客户端。

在Maven项目中，添加以下pom依赖，安装阿里云IoT SDK。

``` {#codeblock_rw3_inn_jmj}
<dependency>
  <groupId>com.aliyun.openservices</groupId>
  <artifactId>iot-client-message</artifactId>
  <version>1.1.3</version>
</dependency>
<dependency>
  <groupId>com.aliyun</groupId>
  <artifactId>aliyun-java-sdk-core</artifactId>
  <version>3.7.1</version>
</dependency>
```

Config.\*参数值中，需传入您的阿里云账号AccessKey信息和设备信息。

``` {#codeblock_zgo_dhn_rt9}
  // 您的阿里云账号AccessKey ID
  private static String accessKeyID = "Config.accessKey";
  // 您的阿里云账号AccesseKey Secret
  private static String accessKeySecret = "Config.accessKeySecret";
  // 您的阿里云账号ID
  private static String uid = "Config.uid";
  // regionId
  private static String regionId = "cn-shanghai";
  // endPoint: https://${uid}.iot-as-http2.${region}.aliyuncs.com
  private static String endPoint = "https://" + uid + ".iot-as-http2." + regionId + ".aliyuncs.com";
```

完整示例代码如下：

``` {#codeblock_5bo_keq_vc7}
/*   
 * Copyright © 2019 Alibaba. All rights reserved.
 */
package com.aliyun.iot.demo.checkstatus;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.LinkedBlockingQueue;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.aliyun.openservices.iot.api.Profile;
import com.aliyun.openservices.iot.api.message.MessageClientFactory;
import com.aliyun.openservices.iot.api.message.api.MessageClient;
import com.aliyun.openservices.iot.api.message.callback.MessageCallback;
import com.aliyun.openservices.iot.api.message.entity.MessageToken;
import com.google.common.util.concurrent.ThreadFactoryBuilder;

public class GetDeviceStatusByH2 {

  // ===================需要用户填写的参数开始===========================
  // 修改Config.*的参数为您的账号信息
  // 各项信息的获取方式请参考 https://help.aliyun.com/document_detail/89227.html
  // 用户账号AccessKey
  private static String accessKeyID = "Config.accessKey";
  // 用户账号AccesseKeySecret
  private static String accessKeySecret = "Config.accessKeySecret";
  // 用户uid
  private static String uid = "Config.uid";
  // regionId
  private static String regionId = "cn-shanghai";
  // endPoint: https://${uid}.iot-as-http2.${region}.aliyuncs.com
  private static String endPoint = "https://" + uid + ".iot-as-http2." + regionId + ".aliyuncs.com";
  // ===================需要用户填写的参数结束===========================

  private static ExecutorService executorService = new ThreadPoolExecutor(Runtime.getRuntime().availableProcessors(),
      Runtime.getRuntime().availableProcessors() * 2, 60, TimeUnit.SECONDS, new LinkedBlockingQueue<>(100),
      new ThreadFactoryBuilder().setDaemon(true).setNameFormat("http2-downlink-handle-%d").build(),
      new ThreadPoolExecutor.AbortPolicy());

  /**
   * 1、设置服务端订阅
   * 2、启动本程序
   * 3、启动您的设备，使设备在线
   * 4、关闭您的设备，使设备离线
   * 5、查看本程序打印的日志
   * 
   * @param args
   */
  public static void main(String[] args) {

    // 连接配置
    Profile profile = Profile.getAccessKeyProfile(endPoint, regionId, accessKeyID, accessKeySecret);

    // 构造客户端
    MessageClient client = MessageClientFactory.messageClient(profile);

    // 消息回调处理
    MessageCallback messageCallback = new MessageCallback() {
      @Override
      public Action consume(MessageToken messageToken) {
        // 返回消费成功，业务另起线程处理，以免堵塞回调
        executorService.submit(() -> handleDownLinkMessage(messageToken));
        // 收到消息应该尽快return commitSuccess
        return MessageCallback.Action.CommitSuccess;
      }
    };

    // 本地过滤。物联网平台会将已订阅的全部设备消息推送下来，这里通过匹配Topic，过滤出要处理的消息
    // 这里只处理Topic为 /as/mqtt/status 开头的消息，其他消息也会收到但不处理
    // Topic及其对应的消息格式请参考 https://help.aliyun.com/document_detail/73736.html
    client.setMessageListener("/as/mqtt/status/#", messageCallback);

    // 通用回调，setMessageListener中未匹配的消息在这里处理
    MessageCallback messageCallbackCommon = new MessageCallback() {

      @Override
      public Action consume(MessageToken messageToken) {
        // 如果有业务处理，建议另起线程处理
        return MessageCallback.Action.CommitSuccess;
      }
    };

    // 数据接收
    client.connect(messageCallbackCommon);
  }

  private static void handleDownLinkMessage(MessageToken messageToken) {
    // 整个消息体内容是JSON
    String message = new String(messageToken.getMessage().getPayload());
    // 获取其中的status字段，就是设备在线状态
    JSONObject json = (JSONObject) JSON.parse(message);
    String deviceName = json.getString("deviceName");
    String status = json.getString("status");
    System.out.println("消息原始内容: " + message);
    System.out.println(deviceName + " 在线状态: " + status);
    // 其他自定义实现
  }
}
```

服务端接收订阅消息的Java HTTP/2 SDK配置详细说明，请参见[开发指南（Java）](../../../../cn.zh-CN/用户指南/产品与设备/服务端订阅/开发指南（Java）.md#)。

