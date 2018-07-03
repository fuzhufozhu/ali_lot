# SQL statements {#concept_vc2_d4n_vdb .concept}

When using the rules engine, if your data is in the JSON format, your data can be parsed and you can write SQL statements to process the parsing result. This section describes SQL statements.

## SQL statements {#section_ehn_l1x_wdb .section}

JSON data can be mapped to a virtual table. Keys in a JSON data record correspond to the column names, and values in a JSON data record correspond to the column values. Once mapped to a virtual table the JSON data record can be processed using SQL. To assist your understanding, the following section provides an example of abstracting a rule from the rules engine into a SQL statement.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7487/3123_en-US.png)

```

For example, an environmental sensor is used for fire warning and collecting temperature, humidity, and atmospheric pressure data. The sensor reports the following data:
{
"temperature":25.1
"humidity":65
"pressure":101.5
"location":"xxx,xxx"
}
Assuming that an alarm needs to be triggered if the temperature is higher than 38 degrees Celsius and the humidity is less than 40% RH, you can write the following SQL statement as a rule: SELECT temperature as t, deviceName() as deviceName, location FROM /ProductA/+/update WHERE temperature > 38 and humidity < 40
Then if the reported data indicates that the temperature is higher than 38 degrees Celsius and the humidity is less than 40% RH, this rule is triggered, and the temperature, device name, and location in the data record are parsed for further processing.
```

## FROM {#section_r34_k1x_wdb .section}

Following FROM, you need to specify topic wildcards here to match the rule with the topics that contain the messages to be processed. When a message that belongs to the specified topics arrives, only the message payload that is in the JSON format can be parsed and then processed by the SQL statement that you have defined. If the message format is invalid, the message will be ignored. You can use `topic()` to reference a specific topic.

```
In this example, the "FROM /ProductA/+/update" statement indicates that only messages that match /ProductA/+/update will be processed. For more information about how messages match topics, see [Topic](intl.en-US/User Guide/Create products and devices/Topics/System-defined topics.md#).

```

## SELECT {#section_jnk_j1x_wdb .section}

-   JSON data

    In the SELECT statement, you can specify the result of parsing the payload of the reported message, which represents the keys and values in the JSON data. You can also use built-in functions in the SELECT statement, such as `deviceName()`. SQL subqueries are not supported.

    The reported JSON data can be an array or nested JSON data. You can also use a JSONPath expression to obtain the key values in the reported data record. For example, there is a payload `{a:{key1:v1, key2:v2}}`, and you can obtain the value `v2` by specifying `a.key2` as the jsonpath.           When specifying variables in SQL statements, pay attention to the difference between using single quotes \('\) and double quotes \("\). Single quotes \('\) enclose constants. Double quotes \("\) enclose variables. Variables may also be written without being enclosed by quotes. If you use single quotes \('\) around a variable, such as `'a.key2'`, a.key2 will be taken as a constant.

    For more information about built-in functions, see [Functions](intl.en-US/User Guide/Rules engine/Functions.md#).

    ```
    In the statement "SELECT temperature as t, deviceName() as deviceName, location" that is provided in the previous example, temperature and loaction are the fields in the reported message, and deviceName() is a built-in function.
    
    ```

-   Binary data

    If the reported message is composed of binary data, fields in the payload cannot be parsed. The SELECT statement you need to use is `SELECT  *`, indicating that the binary data will be passed through.


## WHERE {#section_mnk_j1x_wdb .section}

-   JSON data

    The WHERE clause is used as the condition for triggering the rule. SQL subqueries are not supported. The fields that can be used in the WHERE clause are the same as those that can be used in the SELECT statement. The result obtained using the WHERE clause will be used to determine whether a rule will be triggered. For more information about WHERE expressions, see the following table.

    ```
    In the previous example, "WHERE temperature > 38 and humidity < 40" indicates that the rule will be triggered when the temperature is higher than 38 degrees Celsius and the humidity is less than 40% RH.
    ```

-   Binary data

    If the reported message is composed of binary data, you can only use built-in functions and WHERE expressions in the WHERE clause. You cannot use the fields in the payload of the reported message.


## SQL results {#section_y3t_rbx_wdb .section}

The SQL result returned after the SQL statement is executed will be forwarded. If an error occurs while parsing the payload of the reported message, the rule execution fails. In the expression used for data forwarding, you need to use $\{expression\} to specify the data you want to forward.  

```
For the previous example, when configuring the data forwarding action, you can use ${t}, ${deviceName}, and ${loaction} to reference the SQL result. For example, if you want to forward the SQL result to Table Store, you can use ${t}, ${deviceName}, and ${loaction}.

```

## Notes about using arrays {#section_z3t_rbx_wdb .section}

You need to use double quotes \("\) when referencing arrays in SQL statements. For example, if a message is `｛a:[{v:1},{v:2}]｝` and you want to reference `a[0]` in the SELECT statement, you need to express the SELECT statement as `select ".a[0]" data1,".a[1].v" data2`, in which `data1=［{v:1}］` and `data2=2`.

## Supported WHERE expressions {#section_fgb_5bx_wdb .section}

|**Operator**|**Description**|**Example**|
|=|Equal to|color = ‘red’|
|<\>|Not equal to|color <\> ‘red’|
|AND|Logic AND|color = ‘red’ AND siren = ‘on’|
|OR|Logic OR|color = ‘red’ OR siren = ‘on’|
|\( \)|Parentheses enclose the conditions that will be evaluated as a whole.|color = ‘red’ AND \(siren = ‘on’ OR isTest\)|
|+|Addition|4 + 5|
|-|Subtraction|5-4|
|/|Division|20 / 4|
|`*`|Multiplication|5 `*` 4|
|%|Return the remainder|20% 6|
|<|Less than|5 < 6|
|<=|Less than or equal to|5 <= 6|
|\>|Greater than|6 \> 5|
|\>=|Greater than or equal to|6 \>= 5|
|Function call|For more information about supported functions, see [Functions](intl.en-US/User Guide/Rules engine/Functions.md#).|deviceId\(\)|
|Attributes expressed in the JSON format|You can extract attributes from the message payload and express them in the JSON format.|state.desired.color,a.b.c\[0\].d|
|CASE … WHEN … THEN … ELSE … END|CASE expression|CASE col WHEN 1 THEN ‘Y’ WHEN 0 THEN ‘N’ ELSE ‘’ END as flag|
|IN|Only listing is supported. Subqueries are not supported.|For example, you can use WHERE a IN\(1, 2, 3 \). However, you cannot use WHERE a IN\(select xxx\).|
|LIKE|The LIKE operator is used to search for a specified pattern. When you use a LIKE operator, you can only use the % wildcard character to represent a string of any characters.|For example, you can use the LIKE operator as in WHERE c1 LIKE ‘%abc’ and WHERE c1 not LIKE ‘%def%’.|

