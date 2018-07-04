# Use RAM users {#concept_xxr_nw4_tdb .concept}

RAM users \(sub-accounts\) can log on to the IOT Platform console to manage IoT resources, and use the corresponding AccessKeyId and AccessKeySecret to use IoT application programming interface \(API\).

You need to create a RAM user first, and assign the permissions for accessing IoT Platform to this RAM user by using authorization policies. For more information about customizing authorization policies, see [Custom permissions](intl.en-US/User Guide/Accounts and logon/Resource Access Management (RAM)/Custom permissions.md#).

## Create a RAM user {#section_osg_gsc_5db .section}

Skip this step if you already have a RAM user.

1.  Log on to the [RAM console](https://ram.console.aliyun.com/).
2.  In the left-side navigation pane, click **Users**.
3.  Click **Create User**.
4.  Enter user information, select **Automatically generate an AccessKey for this user.**, and then click **OK**.

    **Note:** The system prompts you to save the AccessKey after you click **OK**. You can download this AccessKey only at this moment. You need to save this AccessKey and secure it immediately. The system requires the AccessKey when the corresponding RAM user calls API operations.

5.  Set the initial login password.
    1.  On the User Management page, click **Manage** of the created RAM user to enter the User Details page.
    2.  Click **Enable Console Logon**.
    3.  Set an initial password for this RAM user, select **On your next logon you must reset the password.**, and then click **OK**.
6.  Enable multi-factor authentication \(MFA\). \(Optional\)

    On the User Details page, click **Enable VMFA Device**.


After you create the RAM user, the RAM user can log on to the Alibaba Cloud official website and the IoT Platform console by using the Resource Access Management \(RAM\) user logon link. To obtain the RAM user logon link, go to the RAM Overview page in the RAM console.

However, the RAM user cannot access your Alibaba Cloud resources before you grant permissions to the RAM user. Therefore, you need to assign permissions for accessing IoT Platform to this RAM user.

## Authorize the RAM user to access IoT Platform {#section_zkb_ydd_5db .section}

In the RAM console, assign permissions to a RAM user on the User Management page, or assign the same permissions to a group on the Group Management page. To assign permissions to a RAM user, follow these steps:

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) using the primary account.
2.  In the left-side navigation pane, click **Users**.
3.  Click **Authorize** next to the RAM user that you want to assign permissions to.
4.  In the authorization dialog box, select the authorization policy that you want to apply to this RAM user, click the right arrow in the middle of the page to move the selected authorization policy to **Selected Authorization Policy Name**, and then click **OK**.

    **Note:** To assign custom permissions to the RAM user, you need to create an authorization policy first. For more information about customizing an authorization policy, see [Custom permissions](intl.en-US/User Guide/Accounts and logon/Resource Access Management (RAM)/Custom permissions.md#).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7494/4853_en-US.jpg)


The authorized RAM user can access the resources defined in the authorization policy, and perform the specified operations.

## Logon to the console using a RAM user {#section_jrd_4rw_5db .section}

The Alibaba Cloud primary account user can log on to the console from the Alibaba Cloud official website. However, the RAM user needs to log on to the console on the RAM User Logon page.

1.  Obtain the link for logging on to the RAM User Logon page.

    Log on to the RAM console using the primary account, view the **RAM User Logon Link** on the RAM Overview page, and then send this logon link to the RAM user.

2.  The RAM user accesses the RAM User Logon page, and logs on to the console using the RAM user name and password.

    **Note:** The RAM user follows this logon format: RAM user name@company alias, such as username@company-alias. The RAM user also needs to change the logon password after logon for the first time.

3.  Click **Console** in the upper-right corner of the page to go to the Home page.
4.  Click **Products**, and select **IoT Platform** to go to the IoT Platform console.

Then, the RAM user can perform authorized operations in the console.

