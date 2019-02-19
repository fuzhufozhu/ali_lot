# Custom permissions {#concept_r3y_lw4_tdb .concept}

Permissions define the conditions in which the system allows or denies some specified actions on target resources.

Permissions are defined in authorization policies. Custom permissions allow you to define certain permissions by using custom authorization policies. In the Resource Access Management \(RAM\) console, click Create Authorization Policy on the **Policies** page to customize an authorization policy. Select a blank template when customizing an authorization policy.

An authorization policy is a JSON string that requires the following parameters:

-   Action: Indicates the action that you want to authorize. IoT actions start with iot:. For more information about actions and examples, see Define actions.

-   Effect: Indicates the authorization type, which can be Allow or Deny.

-   Resource: Because IoT Platform does not support resource authorization, enter an asterisk `*` instead.

-   Condition: Indicates the authentication condition. For more information, see Define conditions.


## Define actions {#section_fpg_jrb_5db .section}

Action is an application programming interface \(API\) operation name. When creating an authorization policy, use iot: as the prefix for each action, and separate multiple actions with commas \(,\). You can also use an asterisk \(\*\) as a wildcard character. For more information about API name definitions that are used on IoT Platform, see[API permissions](intl.en-US/User Guide/Accounts and logon/Resource Access Management (RAM)/API permissions.md#) .

The following are some examples of action definitions.

-   Define a single API operation.

    ```
    "Action": "iot:CreateProduct"
    ```

-   Define multiple API operations.

    ```
    "Action": [
    "iot:UpdateProduct",
    "iot:QueryProduct"
    ]
    ```

-   Define all read-only API operations.

    ```
    {
      "Version": "1", 
      "Statement": [
        {
          "Action": [
            "iot:Query*", 
            "iot:List*", 
            "iot:Get*", 
            "iot:BatchGet*", 
            "iot:Check*"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
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
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "mns:ListTopic", 
            "mns:GetTopicRef"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "ots:ListInstance", 
            "ots:GetInstance", 
            "ots:ListTable", 
            "ots:DescribeTable"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "fc:ListServices", 
            "fc:GetService", 
            "fc:GetFunction", 
            "fc:ListFunctions"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "log:ListShards", 
            "log:ListLogStores", 
            "log:ListProject"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "cms:QueryMetricList"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }
      ]
    }
    ```

-   Define all read-write API operations.

    ```
    {
      "Version": "1", 
      "Statement": [
        {
          "Action": "iot:*", 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "rds:DescribeDBInstances", 
            "rds:DescribeDatabases", 
            "rds:DescribeAccounts", 
            "rds:DescribeDBInstanceNetInfo", 
            "rds:ModifySecurityIps"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": "ram:ListRoles", 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "mns:ListTopic", 
            "mns:GetTopicRef"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "ots:ListInstance", 
            "ots:ListTable", 
            "ots:DescribeTable", 
            "ots:GetInstance"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "fc:ListServices", 
            "fc:GetService", 
            "fc:GetFunction", 
            "fc:ListFunctions"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": [
            "log:ListShards", 
            "log:ListLogStores", 
            "log:ListProject"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }, 
        {
          "Action": "ram:PassRole", 
          "Resource": "*", 
          "Effect": "Allow", 
          "Condition": {
            "StringEquals": {
              "acs:Service": "iot.aliyuncs.com"
            }
          }
        }, 
        {
          "Action": [
            "cms:QueryMetricList"
          ], 
          "Resource": "*", 
          "Effect": "Allow"
        }
      ]
    }
    ```


## Define conditions {#section_hxt_fdc_5db .section}

RAM authorization policies currently support multiple authentication conditions, such as the access IP address restrictions, the Hypertext Transfer Protocol Secure \(HTTPS\)-based access enabler, the multi-factor authentication \(MFA\)-based access enabler, and access time restrictions. All API operations on IoT Platform support these authentication conditions.

Access control based on source IP addresses

This access control restricts source IP addresses that can access IoT Platform, and supports filtering by Classless Inter-Domain Routing \(CIDR\) blocks. Typical scenarios are described as follows:

-   Apply access control rules to a single IP address or CIDR blocks. For example, the following code indicates that only access requests from IP address 10.101.168.111 or 10.101.169.111/24 are allowed.

    ```
    {
    "Statement": [
    {
    "Effect": "Allow",
    "Action": "iot:*",
    "Resource": "*",
    "Condition": {
    "IpAddress": {
    "acs:SourceIp": [
    "10.101.168.111",
    "10.101.169.111/24"
    ]
    }
    }
    }
    ],
    "Version": "1"
    }
    ```

-   Apply access control rules to multiple IP addresses. For example, the following code indicates that only access requests from IP addresses 10.101.168.111 and 10.101.169.111 are allowed.

    ```
    {
    "Statement": [
    {
    "Effect": "Allow",
    "Action": "iot:*",
    "Resource": "*",
    "Condition": {
    "IPaddress ":{
    "acs:SourceIp": [
    "10.101.168.111",
    "10.101.169.111"
    ]
    }
    }
    }
    ],
    "Version": "1"
    }
    
    
    ```


HTTPS-based access control

This access control allows you to enable or disable HTTPS-based access.

For example, the following code indicates that only HTTPS-based access is allowed.

```
{
"Statement": [
{
"Effect": "Allow",
"Action": "iot:*",
"Resource": "*",
"Condition": {
"Bool": {
"acs:SecureTransport": "true"
}
}
}
],
"Version": "1"
}
```

MFA-based access control

This access control allows you to enable or disable MFA-based access.

For example, the following code indicates that only MFA-based access is allowed.

```
{
"Statement": [
{
"Effect": "Allow",
"Action": "iot:*",
"Resource": "*",
"Condition": {
"Bool": {
"acs:MFAPresent ": "true"
}
}
}
],
"Version": "1"
}
```

Access time restrictions

This access control allows you to limit the access time of requests. Access requests earlier than the specified time are allowed or rejected.

For example, the following code indicates that only access requests earlier than 00:00:00 Beijing Time \(UTC+8\) on January 1, 2019 are allowed.

```
{
"Statement": [
{
"Effect": "Allow",
"Action": "iot:*",
"Resource": "*",
"Condition": {
"DateLessThan": {
"acs:CurrentTime": "2019-01-01T00:00:00+08:00"
}
}
}
],
"Version": "1"
}
```

## Typical scenarios {#section_tpt_ydc_5db .section}

Based on these definitions of actions, resources, and conditions, authorization policies are described in the following typical scenarios.

The following is an example of authorization policy that allows access.

Scenario: Assigns IoT Platform access permissions to the IP address 10.101.168.111/24, and only allows HTTPS-based access before 00:00:00 Beijing Time \(UTC+8\) on January 1, 2019.

```
{
"Statement": [
{
"Effect": "Allow",
"Action": "iot:*",
"Resource": "*",
"Condition": {
"IPaddress ":{
"acs:SourceIp": [
"10.101.168.111/24"
]
},
"DateLessThan": {
"acs:CurrentTime": "2019-01-01T00:00:00+08:00"
},
"Bool": {
"acs:SecureTransport": "true"
}
}
}
],
"Version": "1"
}
```

The following is an example of authorization policy to specify denied access.

Scenario: Rejects read requests from IP address 10.101.169.111.

```
{
"Statement": [
{
"Effect": "Deny",
"Action": [
"iot:Query*",
"iot:List*",
"iot:Get*",
"iot:BatchGet*" 
],
"Resource": "*",
"Condition": {
"IpAddress": {
"acs:SourceIp": [
"10.101.169.111"
]
}
}
}
],
"Version": "1"
}
```

After creating the authorization policy, apply this policy to the RAM users on the User Management page in the RAM console. Authorized RAM users can perform the operations defined in this policy. For more information about creating RAM users and granting permissions, see [Use RAM users](intl.en-US/User Guide/Accounts and logon/Resource Access Management (RAM)/Use RAM users.md#).

