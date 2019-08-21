# 进阶使用 STS {#concept_axc_sw4_tdb .concept}

STS 权限管理系统是比访问控制（RAM）更为严格的权限管理系统。使用 STS权限管理系统进行资源访问控制，需通过复杂的授权流程，授予子账号用户临时访问资源的权限。

子账号和授予子账号的权限均长期有效。删除子账号或解除子账号权限，均需手动操作。发生子账号信息泄露后，如果无法及时删除该子账号或解除权限，可能给您的阿里云资源和重要信息带来危险。所以，对于关键性权限或子账号无需长期使用的权限，您可以通过STS 权限管理系统来进行控制。

![STS](images/5054_zh-CN.jpg "子账号获得临时访问权限的操作流程")

## 步骤一：创建角色 {#section_aqw_zqd_5db .section}

RAM 角色是一种虚拟用户，是承载操作权限的虚拟概念。

1.  使用阿里云主账号登录[访问控制 RAM 控制台](https://ram.console.aliyun.com/)。
2.  单击**角色管理** \> **新建角色**，进入角色创建流程。
3.  选择**用户角色**。
4.  填写信息类型步骤中，直接使用默认的当前云账号信息，单击**下一步**。
5.  输入角色名称和备注后，单击**创建**。
6.  单击**关闭**或**授权**。

    如果您已创建要授予该角色的授权策略，则单击**授权**，并对角色进行授权。

    如果您还没有创建授权策略，则单击**关闭**。然后，在策略管理中为该角色新建授权策略。


## 步骤二：创建角色授权策略 {#section_kvv_2td_5db .section}

角色授权策略，即定义要授予角色的资源访问权限。

1.  在[访问控制 RAM 控制台](https://ram.console.aliyun.com/)主页，单击**策略管理** \> **新建授权策略**。
2.  选择空白模板。
3.  输入授权策略名称和策略内容，再单击**新建授权策略**。

    授权策略内容的编写方法，请单击**授权策略格式定义**查看。

    授权策略内容示例：IoT 资源只读权限。

    ``` {#codeblock_ii2_6a9_mc7}
    {
        "Version":"1",
        "Statement":[
            {
                "Action":[
                    "rds:DescribeDBInstances",
                    "rds:DescribeDatabases",
                    "rds:DescribeAccounts",
                    "rds:DescribeDBInstanceNetInfo"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Action":"ram:ListRoles",
                "Effect":"Allow",
                "Resource":"*"
            },
            {
                "Action":[
                    "mns:ListTopic"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Action":[
                    "dhs:ListProject",
                    "dhs:ListTopic",
                    "dhs:GetTopic"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Action":[
                    "ots:ListInstance",
                    "ots:ListTable",
                    "ots:DescribeTable"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Action":[
                    "log:ListShards",
                    "log:ListLogStores",
                    "log:ListProject"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Effect":"Allow",
                "Action":[
                    "iot:Query*",
                    "iot:List*",
                    "iot:Get*",
                    "iot:BatchGet*"
                ],
                "Resource":"*"
            }
        ]
    }
    ```

    授权策略内容示例：IoT 资源读写权限。

    ``` {#codeblock_c7f_ddt_mbl}
    {
        "Version":"1",
        "Statement":[
            {
                "Action":[
                    "rds:DescribeDBInstances",
                    "rds:DescribeDatabases",
                    "rds:DescribeAccounts",
                    "rds:DescribeDBInstanceNetInfo"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Action":"ram:ListRoles",
                "Effect":"Allow",
                "Resource":"*"
            },
            {
                "Action":[
                    "mns:ListTopic"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Action":[
                    "dhs:ListProject",
                    "dhs:ListTopic",
                    "dhs:GetTopic"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Action":[
                    "ots:ListInstance",
                    "ots:ListTable",
                    "ots:DescribeTable"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Action":[
                    "log:ListShards",
                    "log:ListLogStores",
                    "log:ListProject"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Effect":"Allow",
                "Action":"iot:*",
                "Resource":"*"
            }
        ]
    }
    ```


授权策略创建成功后，您就可以将该授权策略中定义的权限授予角色。

## 步骤三：为角色授权 {#section_nx2_nzd_5db .section}

角色获得授权后，才具有资源访问权限。

1.  在[访问控制 RAM 控制台](https://ram.console.aliyun.com/)主页，单击**角色管理**。
2.  找到要授权的角色，单击**授权**。
3.  在授权对话框中，选择要授予角色的自定义授权策略，单击中间的向右箭头，将选中的授权策略移至**已选授权策略名称**下，再单击**确定**。

    ![STS](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7494/15663670504853_zh-CN.jpg)


授权完成后，该角色就具有了授权策略定义的权限。您可以单击该角色对应的**管理**操作按钮，进入角色详情页，查看该角色的基本信息和权限信息。

下一步，为子账号授予可以扮演该角色的权限。

## 步骤四：授予子账号角色扮演权限 {#section_qyn_tfk_5db .section}

虽然经过授权后，该角色已拥有了授权策略定义的访问权限，但角色本身只是虚拟用户，需要子账号用户扮演该角色，才能进行权限允许的操作。若任意子账号都可以扮演该角色，也会带来风险，因此只有获得角色扮演权限的子账号用户才能扮演角色。

授权子账号扮演角色的方法：先新建一个Resource参数值为角色 ID 的自定义授权策略，然后用该授权策略为子账号授权。

1.  在[访问控制 RAM 控制台](https://ram.console.aliyun.com/)主页，单击**策略管理** \> **新建授权策略**。
2.  选择空白模板。
3.  输入授权策略名称和策略内容，再单击**新建授权策略**。

    **说明：** 授权策略内容中，参数Resource 的值需为角色 Arn。在**角色管理**页面，单击角色对应的**管理**按钮，进入角色详情页面中，查看角色的 Arn 。

    角色授权策略示例：

    ``` {#codeblock_p49_4xo_242}
    {
        "Version":"1",
        "Statement":[
            {
                "Effect":"Allow",
                "Action":"iot:QueryProduct",
                "Resource":"角色Arn"
            }
        ]
    }
    ```

4.  授权策略创建成功后，返回访问控制 RAM 控制台主页。
5.  单击左侧导航栏中的**用户管理**按钮，进入子账号管理页面。
6.  找到要授权的子账号，并单击对应的**授权**按钮。
7.  在授权对话框中，选择刚新建的角色授权策略，单击中间的向右箭头将授权策略移至**已选授权策略名称**下，再单击**确定**。

授权完成后，子账号便有了可以扮演该角色的权限，就可以使用 STS 获取扮演角色的临时身份凭证，和进行资源访问。

## 步骤五：子账号获取临时身份凭证 {#section_lwx_nxk_5db .section}

获得角色授权的子账号用户，可以通过直接调用 API 或使用 SDK 来获取扮演角色的临时身份凭证：AccessKeyId、AccessKeySecret、和 SecurityToken。STS API 和 STS SDK 详情，请参见访问控制文档中[STS API](../../../../intl.zh-CN/API 参考（STS）/什么是STS.md#)和[STS SDK](../../../../intl.zh-CN/SDK参考/SDK参考（STS）/Java SDK.md#)。

使用 API 和 SDK 获取扮演角色的临时身份凭证需传入以下参数：

-   RoleArn：需要扮演的角色 Arn。
-   RoleSessionName：临时凭证的名称（自定义参数）。
-   Policy：授权策略，即为角色增加一个权限限制。通过此参数限制生成的Token的权限。不指定此参数，则返回的 Token 将拥有指定角色的所有权限。
-   DurationSeconds：临时凭证的有效期。单位是秒，最小为 900，最大为 3600，默认值是 3600。
-   id 和 secret：指需要扮演该角色的子账号的 AccessKeyId 和 AccessKeySecret。

获取临时身份凭证示例

API 示例：子账号用户通过调用 STS 的AssumeRole接口获得扮演该角色的临时身份凭证。

``` {#codeblock_v6h_e07_h5q}
https://sts.aliyuncs.com?Action=AssumeRole
&RoleArn=acs:ram::1234567890123456:role/iotstsrole
&RoleSessionName=iotreadonlyrole
&DurationSeconds=3600
&Policy=<url_encoded_policy>
&<公共请求参数>
```

SDK 示例：子账号用户使用 STS 的 Python 命令行工具接口获得扮演该角色的临时身份凭证。

``` {#codeblock_6ak_gmv_era}
$python ./sts.py AssumeRole RoleArn=acs:ram::1234567890123456:role/iotstsrole RoleSessionName=iotreadonlyrole Policy='{"Version":"1","Statement":[{"Effect":"Allow","Action":"iot:*","Resource":"*"}]}' DurationSeconds=3600 --id=id --secret=secret
```

请求成功后，将返回扮演该角色的临时身份凭证：AccessKeyId、AccessKeySecret、和 SecurityToken。

## 步骤六：子账号临时访问资源 {#section_onl_lpk_zdb .section}

获得扮演角色的临时身份凭证后，子账号用户便可以在调用 SDK 的请求中传入该临时身份凭证信息，扮演角色。

Java SDK 示例：子账号用户在调用请求中，传入临时身份凭证的 AccessKeyId、AccessKeySecret 和 SecurityToken 参数，创建 IAcsClient对象。

``` {#codeblock_k63_6fa_vqx}
IClientProfile  profile = DefaultProfile.getProfile("cn-hangzhou", AccessKeyId,AccessSecret）;
RpcAcsRequest request.putQueryParameter("SecurityToken", Token);
IAcsClient client = new DefaultAcsClient(profile);
AcsResponse response = client.getAcsResponse(request);
```

