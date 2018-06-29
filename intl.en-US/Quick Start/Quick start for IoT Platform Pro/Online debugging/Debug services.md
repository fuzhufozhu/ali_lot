# Debug services {#task_c1w_ktf_vdb .task}

This topic describes how to debug a remotely triggered device service.

In this task, the sprinkling interval is 50 minutes, and the sprinkling amount is 600 milliliters.

1.   Select Automatic Sprinkler as the service to be debugged. 
2.   Enter valid parameter values. 

    Example:

    ```
    {"SprinkleTime":50,"SprinkleVolume":600}
    ```

3.   Click **Dispatch Command**. The device will receive the command from the cloud, as shown in [Figure 1](#fig_hpn_jgj_vdb). 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12798/2128_en-US.png "Automatic sprinkler")

    **Note:** 

    -   Make sure that the parameters are in the exact format that was specified when you created the product. Otherwise, the cloud cannot send the command.
    -   Make sure that the parameter values are valid. Otherwise, the device cannot parse the command and returns an error.

