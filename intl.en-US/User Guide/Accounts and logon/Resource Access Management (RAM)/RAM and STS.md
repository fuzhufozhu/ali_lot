# RAM and STS {#concept_gsv_kw4_tdb .concept}

Resource Access Management \(RAM\) and Security Token Service \(STS\) are access control systems provided by Alibaba Cloud. For more information about RAM and STS, see [RAM help documentation](https://www.alibabacloud.com/help/product/28625.htm).

RAM is used to control the permissions of accounts. By using RAM, you can create and manage RAM users. You can control what resources RAM users can access by granting different permissions to them.

STS is a security token management system. It is used to manage the short-term permissions granted to RAM users. You can use STS to grant permissions to temporary users.

## Background {#section_iv3_z25_tdb .section}

RAM and STS enable you to securely grant permissions to users without exposing your Alibaba Cloud account AccessKey. Once your Alibaba Cloud account AccessKey is exposed, your resources will be exposed to major security risks. Individuals who obtain your AccessKey can perform any operation on the resources under your Alibaba Cloud account and steal personal information.

RAM is a mechanism used to control long-term permissions. After creating RAM users, you can grant them different permissions. AccessKeys of RAM users if exposed do not have the same risk as an Alibaba Cloud account AccessKey being exposed. If the AccessKey of any RAM user is exposed, information potentially exposed is limited. RAM users are valid for a long term.

Unlike RAM, which allows you to grant long-term permissions to users, STS enables you to grant users temporary access. By calling the STS API, you can obtain temporary AccessKeys and tokens. You can assign the temporary AccessKeys and tokens to RAM users so they can access specific resources. Permissions obtained from STS are strictly restricted and have limited validity. Therefore, even if information is unexpectedly exposed, your system will not be severely compromised.

For details about how to use RAM and STS, see Examples.

## Concepts {#section_p4v_qg5_tdb .section}

Before you use RAM and STS, we recommend that you have a basic understanding of the following concepts:

-   RAM user: A user that is created using the RAM console. During or after the creation of a RAM User, an AccessKey can be generated for the RAM user.   After creating a RAM user, you need to configure the password and grant permissions to it. Once this is completed the RAM user can perform authorized operations. A RAM user can be considered a user with specific operation permissions.
-   Role: A virtual entity that represents a group of permissions. Roles do not have their own logon password or AccessKey. A RAM user can assume roles. When roles are assumed the RAM user has the associated role privileges.
-   Policy: A policy defines permissions. For example, a policy defines the permission of a RAM user to read or write to specific resources.
-   Resource: Cloud resources that are accessible to a RAM user, such as all Table Store instances, a Table Store instance, or a table in a Table Store instance.  

The relationship between RAM users and their roles is similar to the relationship between individuals and their identities. For example, the roles of a person might be an employee at work and a father at home.  A person plays different roles in different scenarios. When playing a specific role, the person has the privileges of that role. A role itself is not an operational entity. Only after the user has assumed this role is it a complete operational entity. A role can be assumed by multiple users.

## Examples {#section_vdn_hgv_tdb .section}

To prevent an Alibaba Cloud account from being exposed to security risks if the account AccessKey is exposed, an Alibaba Cloud account administrator creates two RAM users. These RAM users are named A and B. An AccessKey is generated for each of them.   A has the read permission, and B has the write permission. The administrator can revoke the permissions from the RAM users at any time in the RAM console.

Additional, individuals need to be granted temporary access to the API of IoT Platform. In this case, the AccessKey of A must not be disclosed. Instead, the administrator needs to create a role, C, and grant this role access to the API of IoT Platform. Note that C cannot be directly used currently because there is no AccessKey for C, and C is only a virtual entity that owns access to the IoT Platform API.

The administrator needs to call the AssumeRole API operation of STS to obtain temporary security credentials that can be used to access the IoT Platform API. In the AssumeRole call, the value of RoleArn must be the Alibaba Cloud resource name \(ARN\) of C. If the AssumeRole call is successful, STS will return a temporary AccessKeyId, AccessKeySecret, and SecurityToken as security credentials. The validity period of these credentials can be specified when AssumeRole is called. The account administrator can deliver these credentials to users who need access to the API of the IoT Platform. This access to the API is temporary.

## Why is it complicated to use RAM and STS? {#section_ndc_zkv_tdb .section}

The concepts and use of RAM and STS are complicated. This ensures account security and flexible access control at the cost of service ease of use.

RAM users and roles are separated in order to keep the entity that performs operation separate from the virtual entity that represents a group of permissions. If a user needs multiple permissions, such as the read and the write permissions, but in fact the user only needs one permission at a time, you can create two roles. Grant the read permission and the write permission to these two roles, respectively. Then create a RAM user and assign both roles to the RAM user. When the RAM user needs the read permission, assume the role that includes the read permission. When the RAM user needs the write permission, assume the role that includes the write permission. This reduces the risk of a permission leak occurring in each operation. Additionally, you can assign roles to other Alibaba Cloud accounts and RAM users to grant them the permissions included in the roles. This makes it easier for users to use the role permissions.

STS allows more flexible access control. For example, you can configure the validity period for credentials. However, if long-term credentials are required, you can only use RAM to manage RAM users.

The following sections provide guidelines for using RAM and STS and examples for using them. For more information about APIs provided by RAM and STS, see [API Reference - RAM](https://www.alibabacloud.com/help/doc-detail/28672.htm) and [API Reference - STS](https://www.alibabacloud.com/help/doc-detail/28756.htm).

