# Advanced guide to STS {#concept_axc_sw4_tdb .concept}

Security Token Service \(STS\) enables more strict permission management than Resource Access Management \(RAM\). Using STS to implement resource access control involves a complicated authorization process.You can use STS to grant RAM users temporary permissions to access resources.

RAM users and the permissions granted to RAM users have long-term validity. You need to manually delete a RAM user or revoke permissions from RAM users. After the account information of a RAM user has been leaked, if you fail to timely delete this user or revoke related permissions, your Alibaba Cloud resources and important information may be compromised. Therefore, we recommend that you use STS to manage key permissions or permissions that do not require long-term validity.

![](images/5054_en-US.jpg "Process for granting temporary permissions to RAM users.")

## Step 1: Create a role {#section_aqw_zqd_5db .section}

A role is a virtual entity that represents a virtual user with a group of permissions.

1.  Log on to the [RAM console](https://ram.console.aliyun.com/).
2.  Select**Roles** \> **Create Role** to create a role.
3.  Select **User Role**.
4.  Use the default account information, and click **Next**.
5.  Specify the role name and description, and click **Create**.
6.  Click **Close** or **Authorize**.

    If you have created the authorization policy that is to be granted to this role, click**Authorize** to authorize this user.

    If you have not created the authorization policy, click **Close**. You can create an authorization policy for this role by clicking Policies.


## Step 2: Create an authorization policy {#section_kvv_2td_5db .section}

An authorization policy defines the resource permissions that are to be granted to roles.

1.  In the [RAM console](https://ram.console.aliyun.com/), click**Policies** \> **Create Authorization Policy** .
2.  Select the blank template.
3.  Specify the authorization policy name and policy content, and click **Create Authorization Policy**.

    For more information about writing the policy content, click **Authorization Policy Format**.

    Authorization policy example:Read-only permission of IoT resources.

    ```
    
    {
    "Version": "1",
    "Statement": [
    {
    "Action": [
    "rds:DescribeDBInstances",
    "rds:DescribeDatabases",
    "rds:DescribeAccounts",
    "rds:DescribeDBInstanceNetInfo"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Action": "ram:ListRoles",
    "Effect": "Allow",
    "Resource": "*"
    },
    {
    "Action":[
    "mns:ListTopic"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Action": [
    "dhs:ListProject",
    "dhs:ListTopic",
    "dhs:GetTopic"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Action": [
    "ots:ListInstance",
    "ots:ListTable",
    "ots:DescribeTable"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Action":[
    "log:ListShards",
    "log:ListLogStores",
    "log:ListProject"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Effect": "Allow",
    "Action": [
    "iot:Query*",
    "iot:List*",
    "iot:Get*",
    "iot:BatchGet*" 
    ],
    "Resource": "*"
    }
    ]
    }
    ```

    Authorization policy example:Read-write permission of IoT resources.

    ```
    
    {
    "Version": "1",
    "Statement": [
    {
    "Action": [
    "rds:DescribeDBInstances",
    "rds:DescribeDatabases",
    "rds:DescribeAccounts",
    "rds:DescribeDBInstanceNetInfo"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Action": "ram:ListRoles",
    "Effect": "Allow",
    "Resource": "*"
    },
    {
    "Action":[
    "mns:ListTopic"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Action": [
    "dhs:ListProject",
    "dhs:ListTopic",
    "dhs:GetTopic"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Action": [
    "ots:ListInstance",
    "ots:ListTable",
    "ots:DescribeTable"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Action":[
    "log:ListShards",
    "log:ListLogStores",
    "log:ListProject"
    ],
    "Resource": "*",
    "Effect": "Allow"
    },
    {
    "Effect": "Allow",
    "Action": "iot:*",
    "Resource": "*"
    }
    ]
    }
    ```


After an authorization policy has been created, you can grant the permissions defined in this policy to roles.

## Step 3: Authorize a role {#section_nx2_nzd_5db .section}

A role can only have resource access permissions after it has been authorized.

1.  In the [RAM console](https://ram.console.aliyun.com/), click **Roles**.
2.  Select the role that you want to authorize, and click **Authorize**.
3.  In the dialog box that appears, select the custom authorization policy that you want to apply to the specified role, click the right arrow in the middle to move the specified authorization policy to the **Selected Authorization Policy Name** list, and then click **OK**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7494/15586035914853_en-US.jpg)


The role will have the permissions defined in the selected authorization policy after authorization is complete. You can click **Manage** to go to the Role Details page, and view basic information about this role and the permissions it has been granted.

Next, you need to grant a RAM user the permission to play this role.

## Step 4: Grant a RAM user the permission to play the role {#section_qyn_tfk_5db .section}

After authorization is complete, the role obtains the permissions that are defined in the authorization policy. However, the role is only a virtual user. You need a RAM user to play the role in order to perform the operations allowed by the permissions. If all RAM users are allowed to play the role, this causes security risks. You should only grant the permission to play this role to specified RAM users.

To grant a RAM user the permission to play this role, you need to create a custom authorization policy where the Resource parameter of this policy is set to the ID of the role. You then authorize the RAM user with this authorization policy.

1.  In the [RAM console](https://ram.console.aliyun.com/), click**Policies** \> **Create Authorization Policy** .
2.  Select the blank template.
3.  Specify the authorization policy name and policy content, and click **Create Authorization Policy**.

    **Note:** In the policy content, set the Resource parameter to the Arn of the role. On the **Roles** page, find the specified role, click **Manage** to go to the Role Details page, and then view the Arn of the role .

    Role authorization policy example:

    ```
    
    {
    "Version": "1",
    "Statement": [
    {
    "Effect": "Allow",
    "Action": "iot:QueryProduct",
    "Resource": "Role Arn"
    }
    ]
    }
    ```

4.  After the authorization policy has been created, go to the home page of the RAM console.
5.  Click **Users** in the left-side navigation pane to enter RAM user management page.
6.  Select the RAM user you want to authorize and click **Authorize**.
7.  In the dialog box that appears, select the authorization policy that you have just created, click the right arrow in the middle to move the specified authorization policy to the **Selected Authorization Policy Name** list, and then click **OK**.

After authorization is complete, the RAM user obtains the permission to play this role. You can then use STS to obtain the temporary identity credentials for accessing the resources.

## Step 5: The RAM user obtains temporary identity credentials {#section_lwx_nxk_5db .section}

Authorized RAM users can call the STS API operations or use the STS SDKs to obtain the temporary identity credentials for role play. The temporary credentials include an AccessKeyId, AccessKeySecret, and SecurityToken. For more information about the STS API and STS SDKs, see [API Reference \(STS\)](https://www.alibabacloud.com/help/doc-detail/28756.htm)and [SDK Reference \(STS\)](https://www.alibabacloud.com/help/doc-detail/28786.htm).

You need to specify the following parameters when using an STS API or SDK to obtain temporary identity credentials:

-   RoleArn: The Arn of the role that the RAM user is to play.
-   RoleSessionName: The name of the temporary credentials. This is a custom parameter.
-   Policy: The authorization policy. This parameter adds a restriction to the permissions of the role. You can use this parameter to restrict the permissions of the token. If you do not specify this parameter, a token possessing all permissions of the specified role is created.
-   DurationSeconds: The validity period of the temporary credentials. This parameter is measured in seconds. The default value is 3,600 and the value ranges from 900 to 3,600.
-   id and secret: The AccessKeyId and AccessKeySecret of the RAM user.

**Examples of obtaining temporary identity credentials**

API example: The RAM user calls the STS AssumeRole operation to obtain the temporary identity credentials for role play.

```


https://sts.aliyuncs.com?Action=AssumeRole
&RoleArn=acs:ram::1234567890123456:role/iotstsrole
&RoleSessionName=iotreadonlyrole
&DurationSeconds=3600
&Policy=<url_encoded_policy>
&<Common request parameters>
```

SDK example: The RAM user obtains the temporary identity credentials through the Python CLI interface for STS.

```
$python ./sts.py AssumeRole RoleArn=acs:ram::1234567890123456:role/iotstsrole RoleSessionName=iotreadonlyrole Policy='{"Version":"1","Statement":[{"Effect":"Allow","Action":"iot:*","Resource":"*"}]}' DurationSeconds=3600 --id=id --secret=secret
```

After the request has been received, the temporary identity credentials that are required to play the role are returned. The credentials include an AccessKeyId, AccessKeySecret, and SecurityToken.

## Step 6: The RAM user accesses the resources {#section_onl_lpk_zdb .section}

After obtaining the temporary identity credentials, the RAM user can pass in the credentials in the SDK requests to play the specified role.

Java SDK example: The RAM user passes in the AccessKeyId, AccessKeySecret, and SecurityToken parameters that are contained in the temporary identity credentials in the request and creates the IAcsClient object.

```
IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", AccessKeyId,AccessSecret);
RpcAcsRequest request.putQueryParameter("SecurityToken", Token);
IAcsClient client = new DefaultAcsClient(profile);
AcsResponse response = client.getAcsResponse(request);
```

