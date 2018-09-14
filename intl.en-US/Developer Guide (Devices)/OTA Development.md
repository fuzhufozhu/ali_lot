# OTA Development {#concept_sgj_yts_zdb .concept}

## Update firmware {#section_qqd_r5s_zdb .section}

In this example, IoT Platform uses the MQTT protocol to update the firmware.  [Figure 1](#fig_xk2_bd1_ydb) shows the update process as follows:

 ![](images/11336_en-US.png "Firmware update") 

Topics for firmware update

-   The device publishes a message to this topic to report the firmware version to IoT Platform.

    ```
    /ota/device/inform/${productKey}/${deviceName}
    ```

-   The device subscribes to this topic to receive a notification of the firmware update from IoT Platform.

    ```
    /ota/device/upgrade/${productKey}/${deviceName}
    ```

-   The device publishes a message to this topic to report the progress of the firmware update to IoT Platform.

    ```
    /ota/device/progress/${productKey}/${deviceName}
    ```


**Note:** 

-   The device does not periodically send the firmware version to IoT Platform. Instead, the device sends the firmware version to IoT Platform only when the device starts.
-   You can view the firmware version to check if the OTA update is successful.
-   After you have configured the firmware update for multiple devices in the console of an OTA server, the update status of each device becomes Pending.

    When the OTA system receives the update progress from the device, the update status of the device changes to Updating.

-   An offline device cannot receive any update notifications from the OTA server.

    When the device comes online again, the device notifies the OTA server that it is online. When the server receives the online notification, the server determines whether the device requires an update. If an update is required, the server sends an update notification to the device.


## OTA code description {#section_ifq_q5s_zdb .section}

1.  Install the firmware on a device, and start the device.

    The initialization code for OTA is as follows:

    ```
    
    h_ota = IOT_OTA_Init(PRODUCT_KEY, DEVICE_NAME, pclient);
    if (NULL == h_ota) {
      rc = -1;
      printf("initialize OTA failed\n");
    }
    ```

    **Note:** The MQTT connection \(the obtained MQTT client handle pclient\) is used to initialize the OTA module.

    The function is declared as follows:

    ```
    
    /**
    * @brief Initialize OTA module, and return handle.
    * You must construct the MQTT client before you canll this interface.
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

2.  The device downloads the firmware from the received URL.

    -   IOT\_OTA\_IsFetching\(\): Identifies whether firmware is available for download.
    -   IOT\_OTA\_FetchYield\(\): Downloads a firmware package.
    -   IOT\_OTA\_IsFetchFinish\(\): Identifies whether the download has completed or not.
    An example code is as follows:

    ```
    
    // Identifies whether firmware is available for download.
    if (IOT_OTA_IsFetching(h_ota)) {
      unsigned char buf_ota[OTA_BUF_LEN];
      uint32_t len, size_downloaded, size_file;
      do {
        //Iteratively downloads firmware.
        len = IOT_OTA_FetchYield(h_ota, buf_ota, OTA_BUF_LEN, 1); 
        if (len > 0) {
          //Writes the firmware into the storage such as the flash.
        }
      } while (! IOT_OTA_IsFetchFinish(h_ota)); //Identifies whether the firmware download has completed or not.
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
    * @ Brief fetch firmware from remote server with specific timeout value.
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

    **Note:** If you have insufficient device memory, you need to write the firmware into the system OTA partition while downloading the firmware.

3.  Call IOT\_OTA\_ReportProgress\(\) to report the download status.

    Example code:

    ```
    
    if (percent - last_percent > 0) {
      IOT_OTA_ReportProgress(h_ota, percent, NULL);
    }
    IOT_MQTT_Yield(pclient, 100); //
    ```

    You can upload the update progress to IoT Platform. The update progress \(1% to 100%\) is displayed in real time in the progress column of the updating list in the console.

    You can also upload the following error codes:

    -   -1: Failed to update the firmware.
    -   -2: Failed to download the firmware.
    -   -3: Failed to verify the firmware.
    -   -4: Failed to write the firmware into flash.
4.  Call IOT\_OTA\_Ioctl\(\) to identify whether the downloaded firmware is valid.  If the firmware is valid, the device will run with the new firmware at the next startup.

    Example code:

    ```
    
    int32_t firmware_valid;
    IOT_OTA_Ioctl(h_ota, IOT_OTAG_CHECK_FIRMWARE, &firmware_valid, 4);
      if (0 == firmware_valid) {
      printf("The firmware is invalid\n");
    } else {
      printf("The firmware is valid\n");
    }
    ```

    If the firmware is valid, modify the system boot parameters to make the hardware system run with the new firmware at the next startup. The modification method varies by hardware system.

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

5.  Call IOT\_OTA\_Deinit to terminate a connection and release the memory.

    ```
    
    /**
    * @brief Deinitialize OTA module specified by the 'handle', and release the related resource.
    * You must call this operation to release resource if reboot is not invoked after downloading.
    *
    * @param [in] handle: specify the OTA module.
    *
    * @retval 0 : Successful.
    * @retval < 0 : Failed, the value is error code.
    * @see None.
    */
    int IOT_OTA_Deinit(void *handle);
    ```

6.  After the device restarts, the device runs with the new firmware and reports the new firmware version to IoT Platform. After the OTA module is initialized, call IOT\_OTA\_ReportVersion\(\) to report the current firmware version.  The code is as follows:

    ```
    
    if (0 ! = IOT_OTA_ReportVersion(h_ota, "version2.0")) {
      rc = -1;
      printf("report OTA version failed\n");
    }
    ```


