# 数据转发到HiTSDB {#concept_fcz_znc_wdb .concept}

您可以配置规则引擎将处理过的数据转发到时间序列数据库（[HiTSDB](https://help.aliyun.com/product/54825.html)）的实例中。

## 使用须知 {#section_xgt_2nj_wdb .section}

-   目前转发至HiTSDB仅发布在华东2节点。
-   只支持专有网络下HiTSDB实例.
-   只支持同region转发。例如：华东2节点的套件数据只能转发到华东2的HiTSDB中。
-   只支持JSON格式数据转发。

## 准备工作 { .section}

在设置转发之前，您需要参考[设置规则引擎](intl.zh-CN/用户指南/规则引擎/设置规则引擎.md#)编写SQL完成对数据的处理。

## 操作步骤 {#section_frh_gnj_wdb .section}

1.  单击**数据转发**一栏的**添加操作**，出现添加操作页面。选择**存储到高性能时间序列数据库\(HiTSDB\)中**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7549/15331170463031_zh-CN.png)

2.  按照界面提示，设置参数。
    -   选择操作：此处选择高性能时间序列数据库\(HiTSDB\)
    -   地域、实例：选择处理后的数据将要存入哪个数据库实例中。
    -   timestamp：此处的值必须为Unix时间戳，如1404955893000。有如下两种配置方式：
        -   使用转义符`${}`表达式，例如`${time}`。此时timestamp的值为Topic中time字段对应的值。建议使用此方式。
        -   使用规则引擎函数`timestamp()`，此时timestamp的值为规则引擎服务器的时间戳。
    -   tag：必须使用常量配置，值有三种配置方式：
        -   使用转义符`${}`表达式，例如`${city}`，此时值为Topic中city字段对应的值。建议使用此方式。
        -   使用规则引擎函数规定的一些函数，例如`deviceName()`，此时值为DeviceName。
        -   使用常量配置，例如beijing，此时值为beijing。
    -   授权：勾选同意物联网平台向HiTSDB写数据。此时，规则引擎会向时间序列数据库实例中添加网络白名单，用于IoT访问您的数据库， 请勿删除这些IP段。

## 示例 {#section_j3r_tqj_wdb .section}

规则引擎的SQL：

```
SELECT time,city,power,distance, FROM "/myproduct/myDevice/update"`；
```

配置的规则如[操作步骤](#section_frh_gnj_wdb)所示。

发送的消息：

```

{
"time": 1513677897,
"city": "beijing",
"distance": 8545,
"power": 93.0
}
```

规则引擎会向高性能时间序列数据库中写入的两条数据：

```

数据：timestamp:1513677897, [metric:distance value:8545]
tag： device=myDevice,product=bikes,cityName=beijing
```

```

数据：timestamp:1513677897, [metric:power value:93.0]
tag:device=myDevice,product=bikes,cityName=beijing
```

## 注意 {#section_mlf_tsj_wdb .section}

-   发送的消息中除了在规则引擎中配置为timestamp或者tag值的字段外，其他字段都将作为metric写入时间序列数据库。如上例所示除time和city以外，distance和power作为两个metric写入数据库。
-   metric的值只能为**数值类型**，否则会导致写入数据库失败。
-   用户要保证在规则引擎中配置的tag-值键值对能够获取到，如果获取不到任意一个tag-值键值对，会导致写入数据库失败。
-   tag-值键值对限制最多输入8个。
-   metric、tag和tag值只可包含大小写英文字母、中文、数字，以及特殊字符`-_./():,[]=‘`
-   规则引擎在HiTSDB的白名单中添加以下IP段用以访问数据库。若IP未出现，请手动添加。

    华东2：100.104.76.0/24

    HiTSDB控制台白名单示例如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7549/15331170463032_zh-CN.png)


