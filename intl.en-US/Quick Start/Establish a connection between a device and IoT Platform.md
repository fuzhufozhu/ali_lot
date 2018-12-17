# Establish a connection between a device and IoT Platform {#task_c5t_ktf_vdb .task}

Alibaba Cloud IoT Platform provides device SDKs that allow devices to connect to IoT Platform. This article uses a sample program provided by IoT Platform to introduce how to connect the device to IoT Platform using the provided SDK.

-   The SDK used in this example is a C SDK for Linux system. We recommend that you develop this SDK on Ubuntu16.04 \(64-bit\)
-   Software used in the development of the SDK: `make-4.1`, `git-2.7.4`, `gcc-5.4.0`, `gcov-5.4.0`, `lcov-1.12`, `bash-4.3.48`, `tar-1.28`, and `mingw-5.3.1` Using the following command to install the software:

    `apt-get install -y build-essential make git gcc`


1.   Log on to your Linux VM instance. 
2.  Download the C SDK 2.3.0. 

    `wget https://github.com/aliyun/iotkit-embedded/archive/v2.3.0.zip?spm=a2c4g. 11186623.2.13.1f41492b5WHpzV&file=v2.3.0.zip`

3.  Use the unzip command to extract files from the package. 
4.  Open the demo program `vi iotkit-embedded-2.3.0/examples/linkkit/linkkit_example_solo.c` 
5.  Change the values of ProductKey, DeviceName, and DeviceSecret in the demo to be your device certificate information, and then save the file. 

    See the following example:

    ```
    // for demo only
    #define PRODUCT_KEY      "a1I1nn8vPf4"
    #define DEVICE_NAME      "Light00"
    #define DEVICE_SECRET    "n27gKXTxrUx*********QZEmoUX8TceM"
    ```

6.  In the top level directory, use make command to compile the sample program. 

    ```
    $ make distclean
    $ make
    ```

7.  Run the sample program to connect the device to IoT Platform. In the IoT Platform console, you see that the device status is online, indicating that the device has been connected to IoT Platform successfully. 

    Once the device has been connected to IoT Platform, it automatically report messages to IoT Platform. You see the device logs for message contents.


