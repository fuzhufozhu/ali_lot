# SQL表达式 {#concept_vc2_d4n_vdb .concept}

使用规则引擎时，若您的数据为JSON格式，可以编写SQL来解析和处理数据。规则引擎对二进制格式的数据不做解析，直接透传。本文主要讲解SQL表达式。

## SQL表达式 {#section_ehn_l1x_wdb .section}

JSON数据可以映射为虚拟的表，其中Key对应表的列，Value对应列值，这样就可以使用SQL处理。为便于理解，我们将规则引擎的一条规则抽象为一条SQL表达（类试MySQL语法）：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7487/15348344213123_zh-CN.png)

```

例如某环境传感器用于火灾预警，可以采集温度、湿度及气压数据，上报数据内容如下：
{
"temperature":25.1
"humidity":65
"pressure":101.5
"location":"xxx,xxx"
}
假定温度大于38，湿度小于40时，需要触发报警，可以编写如下的SQL语句：SELECT temperature as t, deviceName() as deviceName, location FROM /ProductA/+/update WHERE temperature > 38 and humidity < 40
当上报的数据中，温度大于38且湿度小于40时，会触发该规则，并且解析数据中的温度、设备名称、位置，用于进一步处理。
```

## FROM {#section_r34_k1x_wdb .section}

FROM 需要填写Topic通配符，用于匹配需要处理的消息Topic。当有符合Topic规则的消息到达时，消息的payload数据以JSON格式解析，并根据SQL语句进行处理（如果消息格式不合法，将忽略此消息）。您可以使用`topic()`函数引用具体的Topic值。

```
上文例子中，"FROM /ProductA/+/update"语句表示该SQL仅处理符合/ProductA/+/update格式的消息，具体匹配参考 [Topic](intl.zh-CN/用户指南/创建产品与设备/Topic/Topic列表.md#)。

```

## SELECT {#section_jnk_j1x_wdb .section}

-   JSON数据格式

    SELECT语句中的字段，可以使用上报消息的payload解析结果，即JSON中的键值，也可以使用SQL内置的函数，比如`deviceName()`。不支持子SQL查询。

    上报的JSON数据格式，可以是数组或者嵌套的JSON，SQL语句支持使用JSONPath获取其中的属性值，如对于`{a:{key1:v1, key2:v2}}`，可以通过`a.key2` 获取到值`v2`。使用变量时，需要注意单双引号区别：单引号表示常量，双引号或不加引号表示变量。如使用单引号`'a.key2'`，值为`a.key2`。

    内置的SQL函数可以参考[函数列表](intl.zh-CN/用户指南/规则引擎/函数列表.md#)。

    ```
    例如上文，"SELECT temperature as t, deviceName() as deviceName, location"语句，其中temperature和loaction来自于上报数据中的字段，deviceName()则使用了内置的SQL函数。
    
    ```

-   二进制数据格式

    目前二进制数据不支持解析payload中的字段，SELECT语句固定为`SELECT *`，表示透传二进制数据。


## WHERE {#section_mnk_j1x_wdb .section}

-   JSON数据格式

    规则触发条件，条件表达式。不支持子SQL查询。WHERE中可以使用的字段和SELECT语句一致，当接收到对应Topic的消息时，WHERE语句的结果会作为是否触发规则的判断条件。具体条件表达式列表见下方表格。

    ```
    上文例子中， "WHERE temperature > 38 and humidity < 40" 表示温度大于38且湿度小于40时，才会触发该规则，执行配置。
    ```

-   二进制数据格式

    目前二进制格式WHERE语句中仅支持内置函数及条件表达式，无法使用payload中的字段。


## SQL结果 {#section_y3t_rbx_wdb .section}

SQL语句执行完成后，会得到对应的SQL结果，用于下一步转发处理。如果payload数据解析过程中出错会导致规则运行失败。 转发数据动作中的表达式需要使用 `${表达式}` 引用对应的值。

```
对于上文例子，配置转发动作时，可以${t}、${deviceName}和${loaction}获取SQL解析结果，如果要将数据存储到TableStore，配置中可以使用${t}、${deviceName}和${loaction}。

```

## 数组使用说明 {#section_z3t_rbx_wdb .section}

数组表达式需要使用双引号，比如设备消息为：`｛a:[{v:1},{v:2},{v:3}]｝`，那么SQL语句中的SELECT写法为：`select "$.a[0]" data1,".a[1].v" data2,".a[2]" data3`，则`data1={v:1}`，`data2=2`，`data3=[`{v:3}`]`。

## 条件表达式支持列表 {#section_fgb_5bx_wdb .section}

|**操作符**|**描述**|**举例**|
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
|函数调用|支持函数，详细列表请参考[函数列表](intl.zh-CN/用户指南/规则引擎/函数列表.md#)。|deviceId\(\)|
|JSON属性表达式|可以从消息payload以JSON表达式提取属性。|state.desired.color,a.b.c\[0\].d|
|CASE … WHEN … THEN … ELSE …END|Case 表达式|CASE col WHEN 1 THEN ‘Y’ WHEN 0 THEN ‘N’ ELSE ‘’ END as flag|
|IN|仅支持枚举，不支持子查询。|比如， where a in\(1,2,3\)。不支持以下形式： where a in\(select xxx\)|
|like|匹配某个字符， 仅支持`%`通配符，代表匹配任意字符串。|比如， where c1 like ‘%abc’, where c1 not like ‘%def%’|

