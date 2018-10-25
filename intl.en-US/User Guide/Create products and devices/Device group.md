# Device group {#task_ejm_qp2_cfb .task}

IoT Platform supports device groups. You can assign devices from different products to the same group. This article introduces how to create and manage device groups on the IoT Platform console.

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com/). 
2.  Click **Devices** \> **Group**. 
3.  On the group management page, click **Create Group**, enter group information, and then click **Save**. 

    **Note:** You can create up to 1,000 groups \(including parent groups and subgroups\) .

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21141/154046057411979_en-US.png)

    The parameters are as follows:

    -   **Parent Group**: Select a group type.
        -   Group: Indicates that the group to be created is a parent group.
        -   Select an existing group: Specifies a group as the parent group and creates a subgroup for it.
    -   **Group Name**: Enter a name for the group. A group name can be 4 to 30 characters in length and can include Chinese characters, English letters, digits and underscores \(\_\) . The group name must be unique among the groups for an account, and cannot be modified once the group has been created.
    -   **Group Description**: Describes the group. Can be left empty.
4.  On the Group Management page, click **View** to view the Group Details page of the corresponding group. 
5.  \(Optional\) Add tags for the group. Tags can be used as group identifiers when you manage your groups. 
    1.  Click **Add** under Tag Information, and then enter keys and values of tags. 
    2.  Click **OK** to create all the entered tags. 

        **Note:** You can add up to 100 tags for a group.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21141/154046057411991_en-US.png)

6.  Click **Device List** \> **Add Device to Group**. Select the devices that you want to add to the group. 

    **Note:** 

    -   You can add up to 200 devices at a time. You can add up to 20,000 devices for a group in total.
    -   A device can be included in a maximum of 10 groups.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21141/154046057412058_en-US.png)

    There are two buttons at the upper-right corner of the Add Device to Group page.

    -   Click **All** to display all the devices.
    -   Click **You have selected** to display the devices you have selected.
7.  \(Optional\) Click **Subgroups** \> **Create Group** to add a subgroup for the group. 

    Subgroups are used to manage devices in a more specific manner. For example, you can create subgroups such as "SmartKitchen" and "SmartBedroom" for a parent group "SmartHome", and then you can manage your kitchen devices and bedroom devices separately. The procedure is as follows:

    1.  Select the parent group, enter a group name and description, and click **Save**. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21141/154046057412059_en-US.png)

    2.  On the Subgroups page of the parent group , click **View** to view the corresponding Group Details page. 
    3.  Click **Device List** \> **Add Device to Group**, and then add devices for the subgroup. 
    After creating the subgroup and adding devices for it, you can then manage it. You can also create sub-subgroups within the subgroup.

    **Note:** 

    -   A group can include up to 100 subgroups.
    -   Only three layers of groups are supported: parent group\>subgroup\>sub-subgroup.
    -   A group can only be a subgroup of one parent group.
    -   You can not change the relationships between a parent group and its subgroups once they have been created. If you want to change the relationships, delete the existing subgroups and create new ones.
    -   You cannot delete a group that has subgroups. You must delete all its subgroups before deleting the parent group.

