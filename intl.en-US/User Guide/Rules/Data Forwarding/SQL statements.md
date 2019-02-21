# SQL statements {#concept_vc2_d4n_vdb .concept}

You can write SQL statements to parse and process data when you create data forwarding rules. Binary data will not be parsed, but directly passed through to targets. This topic describes SQL statements.

## SQL statements {#section_ehn_l1x_wdb .section}

JSON data can be mapped to a virtual table. Keys in a JSON data record correspond to the column names. Values in a JSON data record correspond to the column values. After being mapped to a virtual table, a JSON data record can be processed using SQL. The following example demonstrates how to represent a data forwarding rule as a SQL statement.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7487/15507218273123_en-US.png)

```

For example, an environmental sensor that is typically used for fire detection and collecting temperature, humidity, and atmospheric pressure data, reports the following data:
{
"temperature":25.1
"humidity":65
"pressure":101.5
"location":"xxx,xxx"
}
Assume that you need to set an alarm that is triggered when the temperature is higher than 38°C and the humidity is lower than 40%, write the following SQL statement as a rule:
SELECT temperature as t, deviceName() as deviceName, location FROM /ProductA/+/update WHERE temperature > 38 and humidity < 40
If the reported data meets the rule parameters, the rule is triggered and the temperature data is parsed to obtain the information about temperature, device name, and location for further processing.
```

## FROM clause {#section_r34_k1x_wdb .section}

You can enter a topic in the FROM clause. You can enter a wildcard character `+` that includes all topics on the current category level to match the topic whose device messages need to be processed. When a message that matches the specified topic is received, only the message payload that is in the JSON format can be parsed and then processed by the SQL statement that you have defined. Invalid messages are ignored. You can use the `topic()` function to reference a specific topic.

```
In this example, the "FROM /ProductA/+/update" clause indicates that only messages that match the /ProductA/+/update format are processed. For more information about matching rules, see [Topic](reseller.en-US/User Guide/Create products and devices/IoT Platform Pro/System-defined topics.md#).

```

## SELECT statement {#section_jnk_j1x_wdb .section}

-   JSON data

    In the SELECT statement, you can use the result of parsing the payload of the reported message that represents the keys and values in the JSON data. You can also use built-in functions in the SQL statement, such as `deviceName()`.

    You can combine `*` with functions. SQL subqueries are not supported.

    The reported JSON data can be an array or nested JSON data. You can also use a JSONPath expression to obtain values in the reported data record. For example, for a payload `{a:{key1:v1, key2:v2}}`, you can obtain the value `v2` by specifying `a.key2` as the JSON path. When specifying variables in SQL statements, note the difference between single quotation marks \('\) and double quotation marks \("\). Constants are enclosed with single quotation marks \('\). Variables are enclosed with double quotation marks \("\). Variables may also be written without being enclosed by quotation marks. For example, `a.key2` represents a constant whose value is `a.key2`.

    For more information about built-in functions, see [Functions](reseller.en-US/User Guide/Rules/Data Forwarding/Functions.md#).

    ```
    In the statement "SELECT temperature as t, deviceName() as deviceName, location" that is provided in the previous example, temperature and location are the fields in the reported message, and deviceName() is a built-in function.
    
    ```

-   Binary data
    -   Enter `*` to pass through binary data directly. You cannot add a function after `*`.
    -   You can use built-in functions. The`to_base64(*)` function converts the payload that is binary data to a base64 string. The `deviceName()` function extracts the name information of a device.

**Note:** Each SELECT statement can contain up to fifty fields.

## WHERE clause {#section_mnk_j1x_wdb .section}

-   JSON data

    The WHERE clause is used as the condition for triggering the rule. SQL subqueries are not supported. The fields that can be used in the WHERE clause are the same as those that can be used in the SELECT statement. When a message of the corresponding topic is received, the results obtained using the WHERE clause will be used to determine whether a rule will be triggered. For more information about conditional expressions, see the following table: Supported conditional expressions.

    ```
    In the previous example, "WHERE temperature > 38 and humidity < 40" indicates that the rule is triggered when the temperature is higher than 38°C and the humidity is lower than 40%.
    ```

-   Binary data

    If the reported message is composed of binary data, you can only use built-in functions and conditional expressions in the WHERE clause. You cannot use the fields in the payload of the reported message.


## SQL results {#section_y3t_rbx_wdb .section}

The SQL result returned after the SQL statement is executed will be forwarded. If an error occurs while parsing the payload of the reported message, the rule execution fails. In the expression used for data forwarding, you must use `${expression}` to specify the data that you want to forward.

```
In the previous example, when configuring the data forwarding action, you can use ${t}, ${deviceName}, and ${loaction} to reference the SQL result. For example, if you want to forward the SQL result to Table Store, you can use ${t}, ${deviceName}, and ${loaction}.

```

## Notes on arrays {#section_z3t_rbx_wdb .section}

Array expressions are enclosed with double quotation marks \("\). Use `$.` to obtain a JSONObject. `$.` can be omitted. Use `.` to obtain a JSONArray.

If the device message is `{"a":[{"v":0},{"v":1},{"v":2}]}`, results of different expressions are as follows:

-   The result of `"a[0]"` is `{"v":0}`
-   The result of `"$.a[0]"` is `{"v":0}` 
-   The result of `".a[0]"` is `[{"v":0}]`

-   The result of `"a[1].v"` is `1`
-   The result of `"$.a[1].v"` is `1`
-   The result of `".a[1].v"` is `[1]`

## Supported WHERE expressions {#section_fgb_5bx_wdb .section}

|**Operator**|**Description**|**Example**|
|=|Equal to|color = ‘red’|
|<\>|Not equal to|color <\> ‘red’|
|AND|Logic AND|color = ‘red’ AND siren = ‘on’|
|OR|Logic OR|color = ‘red’ OR siren = ‘on’|
|\( \)|Conditions that are enclosed with parentheses \(\) are considered as a whole.|color = ‘red’ AND \(siren = ‘on’ OR isTest\)|
|+|Addition|4 + 5|
|-|Subtraction|5-4|
|/|Division|20 / 4|
|\*|Multiplication|5 \* 4|
|%|Return the remainder|20% 6|
|<|Less than|5 < 6|
|<=|Less than or equal to|5 <= 6|
|\>|Greater than|6 \> 5|
|\>=|Greater than or equal to|6 \>= 5|
|Function call|For more information about supported functions, see[Functions](reseller.en-US/User Guide/Rules/Data Forwarding/Functions.md#).|deviceId\(\)|
|Attributes expressed in the JSON format|You can extract attributes from the message payload and express them in the JSON format.|state.desired.color,a.b.c\[0\].d|
|CASE … WHEN … THEN … ELSE … END|CASE expression. Nested expressions are not supported.|CASE col WHEN 1 THEN ‘Y’ WHEN 0 THEN ‘N’ ELSE ‘’ END as flag|
|IN|Only listing is supported. Subqueries are not supported.|For example, you can use WHERE a IN\(1, 2, 3 \). However, you cannot use WHERE a IN\(select xxx\).|
|LIKE|This operator is used to match a specific character. When you use a LIKE operator, you can only use the `%` wildcard character to represent a character string.|For example, you can use the LIKE operator in WHERE c1 LIKE ‘%abc’ and WHERE c1 not LIKE ‘%def%’.|

