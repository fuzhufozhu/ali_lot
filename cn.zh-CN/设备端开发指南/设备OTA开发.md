# 设备OTA开发 {#concept_sgj_yts_zdb .concept}

## 升级流程 {#section_qqd_r5s_zdb .section}

以MQTT协议为例，固件升级流程如[图 1](#fig_xk2_bd1_ydb)所示。

![](images/11336_zh-CN.png "固件升级")

固件升级Topic：

-   设备端上报固件版本给云端

    ```
    /ota/device/inform/${YourProductKey}/${YourDeviceName}
    ```

-   设备端订阅该topic接收云端固件升级通知

    ```
    /ota/device/upgrade/${YourProductKey}/${YourDeviceName}
    ```

-   设备端上报固件升级进度

    ```
    /ota/device/progress/${YourProductKey}/${YourDeviceName}
    ```

-   设备端请求是否固件升级

    ```
    /ota/device/request/${YourProductKey}/${YourDeviceName}
    ```


**说明：** 

-   设备固件版本号只需要在系统启动过程中上报一次即可，不需要周期循环上报。
-   根据版本号来判断设备端OTA是否升级成功。
-   从OTA服务端控制台发起批量升级，设备升级操作记录状态是待升级。

    实际升级以OTA系统接收到设备上报的升级进度开始，设备升级操作记录状态是升级中。

-   设备离线时，接收不到服务端推送的升级消息。

    当设备上线后，主动通知服务端上线消息，OTA服务端收到设备上线消息，验证该设备是否需要升级，如果需要，再次推送升级消息给设备， 否则，不推送消息。


## 数据格式说明 {#section_nm2_m4c_r2b .section}

1.  设备连接OTA服务，必须上报版本号。

    设备端通过MQTT协议pub当前设备固件版本号到Topic：`/ota/device/inform/${YourProductKey}/${YourDeviceName}`，内容格式如下：

    ```
    
    {
      "id": 1,
      "params": {
        "version": "xxxxxxxx"
      }
    }
    ```

    -   id：消息ID号，保留值。
    -   version：设备当前固件版本号。
2.  在物联网平台的控制台上添加固件和对指定版本号进行升级，具体请参考上面流程图。

    升级分为几种：验证固件、批量升级、再次升级。

    批量升级的前提是：该固件已通过验证，未验证的固件不可以进行批量升级。

    -   验证固件：执行该功能后，控制台会返回此次验证的结果，选取的设备升级成功或者失败，升级记录可以在控制台的设备升级列表中查看。
    -   批量升级：执行批量升级后，可以在控制台上设备升级列表中查看升级结果。
    -   再次升级：该功能主要针对升级失败的设备进行操作，对于待升级，正在升级，升级成功的设备，是无效的，不可操作的。
3.  触发升级操作之后，设备会收到OTA服务推送的固件的URL地址。

    设备端订阅Topic： `/ota/device/upgrade/${YourProductKey}/${YourDeviceName}`，控制台对设备发起固件升级请求后，设备端会通过该Topic收到固件的URL，格式如下：

    ```
    
    {
      "code": "1000",
      "data": {
        "size": 432945,
        "version": "2.0.0",
        "url": "https://iotx-ota-pre.oss-cn-shanghai.aliyuncs.com/nopoll_0.4.4.tar.gz?    Expires=1502955804&OSSAccessKeyId=XXXXXXXXXXXXXXXXXXXX&Signature=XfgJu7P6DWWejstKJgXJEH0qAKU%3D&security-  token=CAISuQJ1q6Ft5B2yfSjIpK6MGsyN1Jx5jo6mVnfBglIPTvlvt5D50Tz2IHtIf3NpAusdsv03nWxT7v4flqFyTINVAEvYZJOPKGrGR0DzDbDasumZsJbo4f%2FMQBqEaXPS2MvVfJ%2BzLrf0ceusbFbpjzJ6xaCAGxypQ12iN%2B%2Fr6%2F5gdc9FcQSkL0B8ZrFsKxBltdUROFbIKP%2BpKWSKuGfLC1dysQcO1wEP4K%2BkkMqH8Uic3h%2Boy%2BgJt8H2PpHhd9NhXuV2WMzn2%2FdtJOiTknxR7ARasaBqhelc4zqA%2FPPlWgAKvkXba7aIoo01fV4jN5JXQfAU8KLO8tRjofHWmojNzBJAAPpYSSy3Rvr7m5efQrrybY1lLO6iZy%2BVio2VSZDxshI5Z3McKARWct06MWV9ABA2TTXXOi40BOxuq%2B3JGoABXC54TOlo7%2F1wTLTsCUqzzeIiXVOK8CfNOkfTucMGHkeYeCdFkm%2FkADhXAnrnGf5a4FbmKMQph2cKsr8y8UfWLC6IzvJsClXTnbJBMeuWIqo5zIynS1pm7gf%2F9N3hVc6%2BEeIk0xfl2tycsUpbL2FoaGk6BAF8hWSWYUXsv59d5Uk%3D",
        "md5": "93230c3bde425a9d7984a594ac55ea1e"
      },
      "id": 1507707025,
      "message": "success"
    }
    ```

    -   size：固件文件内容大小。
    -   md5：固件内容的MD5值，32位的hex String。
    -   url：固件的URL，带有时效30分钟，超过这个时间后再下载，服务端拒绝访问。
    -   version：固件的目的版本号。
4.  设备收到URL之后，通过HTTPS协议根据URL下载固件。

    **说明：** URL是有时效限制的，目前限制时间是30分钟，超过这个时间后，会拒绝访问。

    下载固件过程中，设备端必须pub升级进度到Topic： `/ota/device/progress/${YourProductKey}/${YourDeviceName}`，内容格式如下：

    ```
    
    {
      "id": 1
      "params": {
        "step":"1", 
        "desc":" xxxxxxxx "
      }   
    }
    ```

    -   id：消息ID号，保留值。
    -   step：\[1, 100\] 下载进度比。
        -   -1：代表升级失败
        -   -2：代表下载失败
        -   -3：代表校验失败
        -   -4：代表烧写失败
    -   desc: 当前步骤的描述信息，如果发生异常可以用此字段承载错误信息。
5.  设备端完成固件升级后，需要pub最新的固件版本到Topic：`/ota/device/inform/${YourProductKey}/${YourDeviceName}`，如果上报的版本与OTA服务要求的版本一致就认为升级成功，反之失败。

    **说明：** 升级成功的唯一判断标志是设备上报正确的版本号。即使升级进度上报为100%，如果不上报新固件版本号，也视为升级失败。


**常见下载固件错误**

-   签名错误，如果设备端获取的固件的URL不全或者手动修改了URL内容，就会出现如下错误：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13905/15362009023964_zh-CN.png)

-   访问拒绝，URL过期导致，目前过期时间为30分钟。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13905/15362009023967_zh-CN.PNG)


## OTA代码说明 {#section_ifq_q5s_zdb .section}

1.  将固件烧录到设备中，并启动运行设备。

    OTA相应的初始代码如下：

    ```
    
    h_ota = IOT_OTA_Init(PRODUCT_KEY, DEVICE_NAME, pclient);
    if (NULL == h_ota) {
      rc = -1;
      printf("initialize OTA failed\n");
    }
    ```

    **说明：** OTA模块的初始化依赖于MQTT连接，即先获得的MQTT客户端句柄pclient。

    函数原型：

    ```
    
    /**
    * @brief Initialize OTA module, and return handle.
    * The MQTT client must be construct before calling this interface.
    *
    * @param [in] product_key: specify the product key.
    * @param [in] device_name: specify the device name.
    * @param [in] ch_signal: specify the signal channel.
    *
    * @retval 0 : Successful.
    * @retval -1 : Failed.
    * @see None.
    */
    void *IOT_OTA_Init(const char *product_key, const char *device_name, void *ch_signal);
    /**
    * @brief Report firmware version information to OTA server (optional).
    * NOTE: please
    *
    * @param [in] handle: specify the OTA module.
    * @param [in] version: specify the firmware version in string format.
    *
    * @retval 0 : Successful.
    * @retval < 0 : Failed, the value is error code.
    * @see None.
    */
    int IOT_OTA_ReportVersion(void *handle, const char *version);
    ```

2.  设备端收到URL之后，通过下载通道下载固件。

    -   IOT\_OTA\_IsFetching\(\)接口：用于判断是否有固件可下载。
    -   IOT\_OTA\_FetchYield\(\)接口：用于下载一个固件块。
    -   IOT\_OTA\_IsFetchFinish\(\)接口：用于判断是否已下载完成。
    具体参考代码如下：

    ```
    
    //判断是否有固件可下载
    if (IOT_OTA_IsFetching(h_ota)) {
      unsigned char buf_ota[OTA_BUF_LEN];
      uint32_t len, size_downloaded, size_file;
      do {
        //循环下载固件
        len = IOT_OTA_FetchYield(h_ota, buf_ota, OTA_BUF_LEN, 1); 
        if (len > 0) {
          //写入到Flash等存储器中
        }
      } while (!IOT_OTA_IsFetchFinish(h_ota)); //判断固件是否下载完毕
    }
    exit: Ctrl ↩
    /**
    * @brief Check whether is on fetching state
    *
    * @param [in] handle: specify the OTA module.
    *
    * @retval 1 : Yes.
    * @retval 0 : No.
    * @see None.
    */
    int IOT_OTA_IsFetching(void *handle);
    /**
    * @brief fetch firmware from remote server with specific timeout value.
    * NOTE: If you want to download more faster, the bigger 'buf' should be given.
    *
    * @param [in] handle: specify the OTA module.
    * @param [out] buf: specify the space for storing firmware data.
    * @param [in] buf_len: specify the length of 'buf' in bytes.
    * @param [in] timeout_s: specify the timeout value in second.
    *
    * @retval < 0 : Error occur..
    * @retval 0 : No any data be downloaded in 'timeout_s' timeout period.
    * @retval (0, len] : The length of data be downloaded in 'timeout_s' timeout period in bytes.
    * @see None.
    */
    int IOT_OTA_FetchYield(void *handle, char *buf, uint32_t buf_len, uint32_t timeout_s);
    /**
    * @brief Check whether is on end-of-fetch state.
    *
    * @param [in] handle: specify the OTA module.
    *
    * @retval 1 : Yes.
    * @retval 0 : False.
    * @see None.
    */
    int IOT_OTA_IsFetchFinish(void *handle);
    ```

    **说明：** 一般情况下，由于RAM不足，在下载的同时需要写入到系统OTA区。

3.  通过IOT\_OTA\_ReportProgress\(\)接口上报下载状态。

    参考代码如下：

    ```
    
    if (percent - last_percent > 0) {
      IOT_OTA_ReportProgress(h_ota, percent, NULL);
    }
    IOT_MQTT_Yield(pclient, 100); //
    ```

    升级进度可以上传，升级进度百分比（1%~100%），对应的进度会实时显示在控制台正在升级列表的进度列。

    升级进度也可以上传失败码，目前失败码有以下四种：

    -   -1：升级失败（fail to upgrade）
    -   -2：下载失败（fail to download）
    -   -3：校验失败（fail to verify）
    -   -4：烧写失败（fail to flash）
4.  通过IOT\_OTA\_Ioctl\(\)接口，获取固件是否合法，硬件系统下次启动时，运行新固件。

    参考代码如下：

    ```
    
    int32_t firmware_valid;
    IOT_OTA_Ioctl(h_ota, IOT_OTAG_CHECK_FIRMWARE, &firmware_valid, 4);
      if (0 == firmware_valid) {
      printf("The firmware is invalid\n");
    } else {
      printf("The firmware is valid\n");
    }
    ```

    如果固件合法，则需要通过修改系统启动参数等方式，使硬件系统下一次启动时运行新固件，不同系统的修改方式可能不一样。

    ```
    
    /**
    * @brief Get OTA information specified by 'type'.
    * By this interface, you can get information like state, size of file, md5 of file, etc.
    *
    * @param [in] handle: handle of the specific OTA
    * @param [in] type: specify what information you want, see detail 'IOT_OTA_CmdType_t'
    * @param [out] buf: specify buffer for data exchange
    * @param [in] buf_len: specify the length of 'buf' in byte.
    * @return
    @verbatim
    NOTE:
    1) When type is IOT_OTAG_FETCHED_SIZE, 'buf' should be pointer of uint32_t, and 'buf_len' should be 4.
    2) When type is IOT_OTAG_FILE_SIZE, 'buf' should be pointer of uint32_t, and 'buf_len' should be 4.
    3) When type is IOT_OTAG_MD5SUM, 'buf' should be a buffer, and 'buf_len' should be 33.
    4) When type is IOT_OTAG_VERSION, 'buf' should be a buffer, and 'buf_len' should be OTA_VERSION_LEN_MAX.
    5) When type is IOT_OTAG_CHECK_FIRMWARE, 'buf' should be pointer of uint32_t, and 'buf_len' should be 4.
    0, firmware is invalid; 1, firmware is valid.
    @endverbatim
    *
    * @retval 0 : Successful.
    * @retval < 0 : Failed, the value is error code.
    * @see None.
    */
    int IOT_OTA_Ioctl(void *handle, IOT_OTA_CmdType_t type, void *buf, size_t buf_len);
    ```

5.  使用IOT\_OTA\_Deinit销毁连接，释放内存。

    ```
    
    /**
    * @brief Deinitialize OTA module specified by the 'handle', and release the related resource.
    * You must call this interface to release resource if reboot is not invoked after downloading.
    *
    * @param [in] handle: specify the OTA module.
    *
    * @retval 0 : Successful.
    * @retval < 0 : Failed, the value is error code.
    * @see None.
    */
    int IOT_OTA_Deinit(void *handle);
    ```

6.  设备重启时运行新固件，并向云端上报新版本号。在OTA模块初始化之后，调用IOT\_OTA\_ReportVersion\(\)接口上报当前固件的版本号，具体代码如下:

    ```
    
    if (0 != IOT_OTA_ReportVersion(h_ota, "version2.0")) {
      rc = -1;
      printf("report OTA version failed\n");
    }
    ```


