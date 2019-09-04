# 调用API获取设备状态 {#task_1715084 .task}

物联网很多业务场景中，时常需要获取设备的实时状态，以便根据不同状态（在线或离线）做不同处理。阿里云物联网平台提供多个云端API来获取设备的状态信息。本文介绍这些API的调用方法。

## 原理 {#section_570_d0n_p1g .section}

以下五个API可以获得设备状态。请根据业务需要，选择调用的接口。

|API|描述|优缺点|
|---|--|---|
|[GetDeviceStatus](../../../../cn.zh-CN/云端开发指南/云端API参考/设备管理/GetDeviceStatus.md#)|获取单个设备的状态。|通过会话来获取设备状态，返回结果可能会因为网络和心跳包延迟而延时更新。|
|[BatchGetDeviceState](../../../../cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchGetDeviceState.md#)|批量获取多个设备的状态。|
|[QueryDeviceDetail](../../../../cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceDetail.md#)|查询单个设备的详细信息 除设备状态外，还可以获得其他设备信息。

 |
|[BatchQueryDeviceDetail](../../../../cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchQueryDeviceDetail.md#)|批量查询多个设备的详细信息 除设备状态外，还可以获得其他设备信息。

 |
|[RRpc](../../../../cn.zh-CN/云端开发指南/云端API参考/消息通信/RRpc.md#)|向指定设备发送查询状态的请求消息，并同步返回响应。|该接口查询到的设备状态信息准确度高。 **说明：** 本文示例中，只介绍调用RRpc查询设备状态的服务端SDK配置；更完整的设备状态查询配置方法，请参见[服务端检测设备是否在线](https://help.aliyun.com/document_detail/101133.html)。

 |

## 实现 {#section_n7t_ezs_90h .section}

本文示例使用Java SDK，需准备Java开发环境。

在Maven项目中，需添加如下pom依赖，安装阿里云IoT SDK。

``` {#codeblock_0ap_kyw_b4w}
<dependency>
  <groupId>com.aliyun</groupId>
  <artifactId>aliyun-java-sdk-core</artifactId>
  <version>3.5.1</version>
</dependency>
<dependency>
  <groupId>com.aliyun</groupId>
  <artifactId>aliyun-java-sdk-iot</artifactId>
  <version>6.11.0</version>
</dependency>
<dependency>
  <groupId>commons-codec</groupId>
  <artifactId>commons-codec</artifactId>
  <version>1.13</version>
</dependency>
```

Config.\*参数值中，需传入您的阿里云账号AccessKey信息和设备信息。

``` {#codeblock_g4i_gt2_zpm}
  // 地域ID，根据您的物联网平台服务地域获取对应ID，https://help.aliyun.com/document_detail/40654.html
  private static String regionId = "cn-shanghai";
  // 您的阿里云账号AccessKey ID
  private static String accessKeyID = "Config.accessKey";
  // 您的阿里云账号AccesseKey Secret
  private static String accessKeySecret = "Config.accessKeySecret";
  // 要查询的设备所属产品的ProductKey
  private static String productKey = "Config.productKey";
  // 要查询的设备的名称DeviceName
  private static String deviceName = "Config.deviceName";
```

完整代码示例如下：

``` {#codeblock_ebl_h1z_p4b}
/*   
 * Copyright © 2019 Alibaba. All rights reserved.
 */
package com.aliyun.iot.demo.checkstatus;

import java.util.ArrayList;
import java.util.List;

import org.apache.commons.codec.binary.Base64;

import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.iot.model.v20180120.BatchGetDeviceStateRequest;
import com.aliyuncs.iot.model.v20180120.BatchGetDeviceStateResponse;
import com.aliyuncs.iot.model.v20180120.BatchGetDeviceStateResponse.DeviceStatus;
import com.aliyuncs.iot.model.v20180120.BatchQueryDeviceDetailRequest;
import com.aliyuncs.iot.model.v20180120.BatchQueryDeviceDetailResponse;
import com.aliyuncs.iot.model.v20180120.BatchQueryDeviceDetailResponse.DataItem;
import com.aliyuncs.iot.model.v20180120.GetDeviceStatusRequest;
import com.aliyuncs.iot.model.v20180120.GetDeviceStatusResponse;
import com.aliyuncs.iot.model.v20180120.QueryDeviceDetailRequest;
import com.aliyuncs.iot.model.v20180120.QueryDeviceDetailResponse;
import com.aliyuncs.iot.model.v20180120.RRpcRequest;
import com.aliyuncs.iot.model.v20180120.RRpcResponse;
import com.aliyuncs.profile.DefaultProfile;
import com.aliyuncs.profile.IClientProfile;

public class GetDeviceStatusByApi {

  // ===================需要用户填写的参数开始===========================
  // 修改Config.*的参数为您的实际信息
  // 地域ID，根据您的物联网平台服务地域获取对应ID，https://help.aliyun.com/document_detail/40654.html
  private static String regionId = "cn-shanghai";
  // 用户账号AccessKey ID
  private static String accessKeyID = "Config.accessKey";
  // 用户账号AccesseKey Secret
  private static String accessKeySecret = "Config.accessKeySecret";
  // 要查询的设备所属的产品ProductKey
  private static String productKey = "Config.productKey";
  // 要查询的设备名称deviceName
  private static String deviceName = "Config.deviceName";
  // ===================需要用户填写的参数结束===========================

  private static DefaultAcsClient client = null;

  private static DefaultAcsClient getClient(String accessKeyID, String accessKeySecret) {

    if (client != null) {
      return client;
    }

    try {
      IClientProfile profile = DefaultProfile.getProfile(regionId, accessKeyID, accessKeySecret);
      DefaultProfile.addEndpoint(regionId, regionId, "Iot", "iot." + regionId + ".aliyuncs.com");
      client = new DefaultAcsClient(profile);
    } catch (Exception e) {
      System.out.println("create Open API Client failed !! exception:" + e.getMessage());
    }

    return client;
  }

  /**
   * 设备状态获取
   * 方法一、二、三、四是基于状态查询的，获取的状态值可能会因为网络和心跳包延迟而延时更新；
   * 方法五是基于同步通信的，结果比较精准
   * 
   * @param args
   * @throws ServerException
   * @throws ClientException
   */
  public static void main(String[] args) throws ServerException, ClientException {

    // 获取服务端请求客户端
    DefaultAcsClient client = getClient(accessKeyID, accessKeySecret);

    GetDeviceStatusByApi api = new GetDeviceStatusByApi();

    // 方法一
    api.ByGetDeviceStatus(client);

    // 方法二
    api.ByBatchGetDeviceState(client);

    // 方法三
    api.ByQueryDeviceDetail(client);

    // 方法四
    api.ByBatchQueryDeviceDetail(client);

    // 方法五
    api.ByRRpc(client);
  }

  /**
   * 查询单设备运行状态
   * GetDeviceStatus https://help.aliyun.com/document_detail/69617.html
   * 
   * @param client 服务端请求客户端
   * @throws ServerException
   * @throws ClientException
   */
  public void ByGetDeviceStatus(DefaultAcsClient client) throws ServerException, ClientException {

    // 填充请求
    GetDeviceStatusRequest request = new GetDeviceStatusRequest();
    request.setProductKey(productKey); // 目标设备产品key
    request.setDeviceName(deviceName); // 目标设备名

    // 获取结果
    GetDeviceStatusResponse response = (GetDeviceStatusResponse) client.getAcsResponse(request);
    if (response != null && response.getSuccess()) {
      GetDeviceStatusResponse.Data data = response.getData();
      // ONLINE：设备在线。
      // OFFLINE：设备离线。
      // UNACTIVE：设备未激活。
      // DISABLE：设备已禁用。
      if ("ONLINE".equals(data.getStatus())) {
        System.out.println("GetDeviceStatus 检测：" + deviceName + " 设备在线");
      } else { // 其他状态归结为设备不在线，也可以根据业务情况自行区分处理
        System.out.println("GetDeviceStatus 检测：" + deviceName + " 设备不在线");
      }
    } else {
      System.out.println("GetDeviceStatus 检测：" + "接口调用不成功，可能设备 " + deviceName + " 不存在");
    }
  }

  /**
   * 批量查询设备运行状态
   * BatchGetDeviceState https://help.aliyun.com/document_detail/69906.html
   * 
   * @param client 服务端请求客户端
   * @throws ServerException
   * @throws ClientException
   */
  public void ByBatchGetDeviceState(DefaultAcsClient client) throws ServerException, ClientException {

    // 填充请求
    BatchGetDeviceStateRequest request = new BatchGetDeviceStateRequest();
    request.setProductKey(productKey); // 目标设备产品key
    List<String> deviceNames = new ArrayList<String>(); // 目标设备名列表
    deviceNames.add(deviceName);
    request.setDeviceNames(deviceNames);

    // 获取结果
    BatchGetDeviceStateResponse response = (BatchGetDeviceStateResponse) client.getAcsResponse(request);
    if (response != null && response.getSuccess()) {
      List<DeviceStatus> dsList = response.getDeviceStatusList();
      for (DeviceStatus ds : dsList) {
        // ONLINE：设备在线。
        // OFFLINE：设备离线。
        // UNACTIVE：设备未激活。
        // DISABLE：设备已禁用。
        if ("ONLINE".equals(ds.getStatus())) {
          System.out.println("BatchGetDeviceState 检测：" + ds.getDeviceName() + " 设备在线");
        } else { // 其他状态归结为设备不在线，也可以根据业务情况自行区分处理
          System.out.println("BatchGetDeviceState 检测：" + ds.getDeviceName() + " 设备不在线");
        }
      }
    } else {
      System.out.println("BatchGetDeviceState 检测：" + "接口调用不成功，可能设备 " + deviceName + " 不存在");
    }
  }

  /**
   * 查询单设备详细信息
   * QueryDeviceDetail https://help.aliyun.com/document_detail/69594.html
   * 
   * @param client 服务端请求客户端
   * @throws ServerException
   * @throws ClientException
   */
  public void ByQueryDeviceDetail(DefaultAcsClient client) throws ServerException, ClientException {

    // 填充请求
    QueryDeviceDetailRequest request = new QueryDeviceDetailRequest();
    request.setProductKey(productKey); // 目标设备产品key
    request.setDeviceName(deviceName); // 目标设备名

    // 获取结果
    QueryDeviceDetailResponse response = (QueryDeviceDetailResponse) client.getAcsResponse(request);
    if (response != null && response.getSuccess()) {
      QueryDeviceDetailResponse.Data data = response.getData();
      // ONLINE：设备在线。
      // OFFLINE：设备离线。
      // UNACTIVE：设备未激活。
      // DISABLE：设备已禁用。
      if ("ONLINE".equals(data.getStatus())) {
        System.out.println("QueryDeviceDetail 检测：" + deviceName + " 设备在线");
      } else { // 其他状态归结为设备不在线，也可以根据业务情况自行区分处理
        System.out.println("QueryDeviceDetail 检测：" + deviceName + " 设备不在线");
      }
    } else {
      System.out.println("QueryDeviceDetail 检测：" + "接口调用不成功，可能设备 " + deviceName + " 不存在");
    }
  }

  /**
   * 批量查询设备详细信息
   * BatchQueryDeviceDetai https://help.aliyun.com/document_detail/123470.html
   * 
   * @param client 服务端请求客户端
   * @throws ServerException
   * @throws ClientException
   */
  public void ByBatchQueryDeviceDetail(DefaultAcsClient client) throws ServerException, ClientException {

    // 填充请求
    BatchQueryDeviceDetailRequest request = new BatchQueryDeviceDetailRequest();
    request.setProductKey(productKey); // 目标设备产品key
    List<String> deviceNames = new ArrayList<String>(); // 目标设备名列表
    deviceNames.add(deviceName);
    request.setDeviceNames(deviceNames);

    // 获取结果
    BatchQueryDeviceDetailResponse response = (BatchQueryDeviceDetailResponse) client.getAcsResponse(request);
    if (response != null && response.getSuccess()) {
      List<DataItem> diList = response.getData();
      for (DataItem di : diList) {
        // ONLINE：设备在线。
        // OFFLINE：设备离线。
        // UNACTIVE：设备未激活。
        // DISABLE：设备已禁用。
        if ("ONLINE".equals(di.getStatus())) {
          System.out.println("BatchQueryDeviceDetail 检测：" + di.getDeviceName() + " 设备在线");
        } else { // 其他状态归结为设备不在线，也可以根据业务情况自行区分处理
          System.out.println("BatchQueryDeviceDetail 检测：" + di.getDeviceName() + " 设备不在线");
        }
      }
    } else {
      System.out.println("BatchQueryDeviceDetail 检测：" + "接口调用不成功，可能设备 " + deviceName + " 不存在");
    }
  }

  /**
   * RRPC 检测设备状态
   * RRpc https://help.aliyun.com/document_detail/69797.html
   * 
   * @param client 服务端请求客户端
   * @throws ServerException
   * @throws ClientException
   */
  public void ByRRpc(DefaultAcsClient client) throws ServerException, ClientException {

    // 填充请求
    RRpcRequest request = new RRpcRequest();
    request.setProductKey(productKey);// 目标设备产品key
    request.setDeviceName(deviceName);// 目标设备名
    request.setRequestBase64Byte(Base64.encodeBase64String("Hello World".getBytes())); // 消息内容，必须base64编码字符串
    request.setTimeout(5000); // 响应超时设置，可根据实际需要填写数值

    // 获取结果
    RRpcResponse response = (RRpcResponse) client.getAcsResponse(request);
    if (response != null) { // 不要使用response.getSuccess()判断，非SUCCESS都是false；直接使用RrpcCode判断即可
      // UNKNOWN：系统异常
      // SUCCESS：成功
      // TIMEOUT：设备响应超时
      // OFFLINE：设备离线
      // HALFCONN：设备离线（设备连接断开，但是断开时间未超过一个心跳周期）
      if ("SUCCESS".equals(response.getRrpcCode())) {
        System.out.println("RRPC 检测：" + deviceName + " 设备在线");
      } else if (response.getRrpcCode() == null) {
        System.out.println("RRPC 检测：" + deviceName + " 设备可能不存在");
      } else { // 其他状态归结为设备不在线，也可以根据业务情况自行区分处理
        System.out.println("RRPC 检测：" + deviceName + " 设备不在线:");
      }
      // RRPC 还可以实现更复杂的设备状态检测方法
      // 请参考 https://help.aliyun.com/document_detail/101133.html
    } else {
      System.out.println("RRPC 检测：" + "接口调用不成功");
    }
  }
}
```

