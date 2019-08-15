# SQL表达式 {#concept_vc2_d4n_vdb .concept}

创建数据流转规则时，需编写SQL来解析和处理设备上报的数据。二进制格式的数据不做解析，直接透传。本文主要介绍如何编写数据流转规则的SQL表达式。

## SQL表达式 {#section_ehn_l1x_wdb .section}

JSON数据可以映射为虚拟的表，其中Key对应表的列，Value对应列值，因此可以使用SQL来进行数据处理。数据流转规则的SQL表达示意图如下：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7487/15658590343123_zh-CN.png)

数据流转规则SQL示例：

-   处理自定义Topic数据的SQL示例。

    某环境传感器可以采集温度、湿度及气压数据。设备上报到自定义Topic：/a1hRrzD\*\*\*\*/+/user/update的数据如下：

    ``` {#codeblock_en1_uyd_1yp}
    {
    "temperature":25.1
    "humidity":65
    "pressure":101.5
    "location":"xxx,xxx"
    }
    ```

    设置温度大于38时触发规则，并筛选出设备名称、温度和位置信息，SQL语句如下：

    ``` {#codeblock_sme_z45_vhw}
    SELECT temperature as t, deviceName() as deviceName, location FROM "/a1hRrzD****/+/user/update" WHERE temperature > 38
    ```

-   处理系统Topic数据的SQL示例。 系统Topic中的数据流转到规则引擎后，规则引擎会对数据进行解析，解析后的数据格式请参见[数据格式](intl.zh-CN/用户指南/规则引擎/数据流转/数据格式.md#)。

    某温湿度传感器的属性如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7487/156585903554522_zh-CN.png)

    温湿度传感器上报的温度和湿度属性数据经规则引擎解析后，数据格式如下：

    ``` {#codeblock_aoq_4s8_r0l}
    {
        "deviceType": "TemperatureHumidityDetector", 
        "iotId": "N5KURkKdibnZvSls****000100", 
        "productKey": "a15NNfl****", 
        "gmtCreate": 1564569974224, 
        "deviceName": "N5KURkKdibnZvSls3Yfx", 
        "items": {
            "CurrentHumidity": {
                "value": 70, 
                "time": 1564569974229
            }, 
            "CurrentTemperature": {
                "value": 23.5, 
                "time": 1564569974229
            }
        }
    }
    ```

    设置温度大于38时触发规则，并筛选出设备名称、当前温度和当前湿度，SQL语句如下：

    ``` {#codeblock_u1p_6ou_mlx}
    SELECT deviceName() as deviceName, items.CurrentHumidity.value as Humidity, items.CurrentTemperature.value as Temperature FROM "/sysa15NNfl****/N5KURkKdibnZvSls3Yfx/thing/event/property/post" WHERE items.CurrentTemperature.value > 38
    ```


## SELECT {#section_cg3_sq2_kto .section}

-   JSON数据格式

    SELECT语句中的字段，可以使用上报消息的payload解析结果，即JSON中的键值，也可以使用SQL内置的函数，如`deviceName()`。规则引擎内置的SQL函数，请参见[函数列表](intl.zh-CN/用户指南/规则引擎/数据流转/函数列表.md#)。

    支持`*`和函数的组合。不支持子SQL查询。

    上报的JSON数据格式，可以是数组或者嵌套的JSON。SQL语句支持使用JSONPath获取其中的属性值，如对于`{a:{key1:v1, key2:v2}}`，可以通过`a.key2` 获取到值`v2`。使用变量时，需要注意单双引号区别：单引号表示常量，双引号或不加引号表示变量。如使用单引号`'a.key2'`，值为`a.key2`。

    以上SQL示例中，

    -   处理自定义Topic数据SQL的SELECT语句`SELECT temperature as t, deviceName() as deviceName, location`中，`temperature`和`loaction`来自于上报数据中的字段，`deviceName()`则使用了内置的SQL函数。
    -   处理上报属性Topic数据SQL的SELECT语句`SELECT deviceName() as deviceName, items.CurrentHumidity.value as Humidity, items.CurrentTemperature.value as Temperature`中，`items.CurrentHumidity.value`和 `items.CurrentTemperature.value`来自于上报的属性数据中的字段，`deviceName()`则使用了内置的SQL函数。
-   二进制数据格式
    -   可填`*`直接透传数据。`*`后不能再使用函数。
    -   可使用内置函数，如`to_base64(*)`函数，将原始Payload二进制数据转成base64String提取出来；`deviceName()`函数，将设备名称信息提取出来。

**说明：** SELECT语句中最多可包含50个字段。

## FROM {#section_r34_k1x_wdb .section}

FROM 可以填写为Topic，用于匹配需要处理的设备消息来源Topic。Topic中的设备名（deviceName）类目可以填写为通配符+，代表当前层级所有类目，即产品下的所有设备；指定为自定义Topic时，还可以使用统配符\#，代表Topic中当前层级及之后的所有类目。通配符说明，请参见[自定义Topic](intl.zh-CN/用户指南/产品与设备/Topic/自定义Topic.md#)。

当来自指定Topic的消息到达时，消息的payload数据以JSON格式解析，并根据SQL语句进行处理。如果消息格式不合法，将忽略此条消息。您可以使用`topic()`函数引用具体的Topic值。

以上SQL示例中，

-   `FROM "/a1hRrzD****/+/user/update"`语句表示该SQL仅处理自定义Topic： /a1hRrzD\*\*\*\*/+/user/update的消息。
-   `FROM "/sys/a15NNfl****/N5KURkKdibnZvSls3Yfx/thing/event/property/post"`语句表示该SQL仅处理设备N5KURkKdibnZvSls3Yfx上报属性的Topic中的消息。

## WHERE {#section_mnk_j1x_wdb .section}

-   JSON数据格式

    规则触发条件，条件表达式。不支持子SQL查询。WHERE中可以使用的字段和SELECT语句一致，当接收到对应Topic的消息时，WHERE语句的结果会作为是否触发规则的判断条件。具体条件表达式，请参见以下章节：条件表达式支持列表。

    上文两个示例中，条件语句`WHERE temperature > 38`表示温度大于38时，才会触发该规则。

-   二进制数据格式

    目前二进制格式WHERE语句中，仅支持内置函数及条件表达式，无法使用payload中的字段。


## SQL结果 {#section_y3t_rbx_wdb .section}

SQL语句执行完成后，会得到对应的SQL结果，用于下一步转发处理。如果payload数据解析过程中出错，会导致规则运行失败。

如果要将数据流转到表格存储Table Store中，设置转发数据目的地时，需要使用变量格式 `${表达式}` 引用对应的值。

如果以上两个示例SQL对应的规则需将数据流转到表格存储的数据表中，主键对应的值可分别配置为

-   $\{t\}、$\{deviceName\}和$\{loaction\}。
-   $\{deviceName\}、$\{Humidity\}和$\{Temperature\}。

## 数组使用说明 {#section_z3t_rbx_wdb .section}

数组表达式需要使用双引号，用`$.`表示取jsonObject，`$.`可以省略，`.`表示取jsonArray。

例如设备消息为：`{"a":[{"v":0},{"v":1},{"v":2}]}`，不同表达式结果如下：

-   `"a[0]"`结果为 `{"v":0}`
-   `"$.a[0]"` 结果为`{"v":0}`
-   `".a[0]"` 结果为 `[{"v":0}]`

-   `"a[1].v"`结果为`1`
-   `"$.a[1].v"`结果为`1`
-   `".a[1].v"` 结果为`[1]`

## 条件表达式支持列表 {#section_fgb_5bx_wdb .section}

|操作符|描述|举例|
|=|相等|color = ‘red’|
|<\>|不等于|color <\> ‘red’|
|AND|逻辑与|color = ‘red’ AND siren = ‘on’|
|OR|逻辑或|color = ‘red’ OR siren = ‘on’|
|\( \)|括号代表一个整体|color = ‘red’ AND \(siren = ‘on’ OR isTest\)|
|+|算术加法|4 + 5|
|-|算术减|5 - 4|
|/|除|20 / 4|
|\*|乘|5 \* 4|
|%|取余数|20 % 6|
|<|小于|5 < 6|
|<=|小于或等于|5 <= 6|
|\>|大于|6 \> 5|
|\>=|大于或等于|6 \>= 5|
|函数调用|支持函数，详细列表请参见[函数列表](intl.zh-CN/用户指南/规则引擎/数据流转/函数列表.md#)。|deviceId\(\)|
|JSON属性表达式|可以从消息payload以JSON表达式提取属性。|state.desired.color,a.b.c\[0\].d|
|CASE … WHEN … THEN … ELSE …END|Case 表达式（不支持嵌套）|CASE col WHEN 1 THEN ‘Y’ WHEN 0 THEN ‘N’ ELSE ‘’ END as flag|
|IN|仅支持枚举，不支持子查询。|例如， where a in\(1,2,3\)。不支持以下形式： where a in\(select xxx\)|
|like|匹配某个字符， 仅支持`%`通配符，代表匹配任意字符串。|例如， where c1 like ‘%abc’, where c1 not like ‘%def%’|

