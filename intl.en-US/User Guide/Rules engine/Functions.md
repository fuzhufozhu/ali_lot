# Functions {#concept_mdn_mss_vdb .concept}

The rules engine provides functions that allow you to handle data when writing a SQL script.

## Call functions {#section_mml_cbt_vdb .section}

You can call functions to get or handle data.

For example, the following code uses the functions: deviceName\(\), abs\(number\), and topic\(number\).

```
SELECT case flag when 1 then 'Light On' when 2 then 'Light Off' else '' end flag，deviceName(),abs(temperature) tmr FROM "/topic/#" WHERE temperature>10 and topic(2)='123'
```

**Note:** Exercise caution when you call functions. Constants are enclosed with apostrophes \('\). Variables are not enclosed or are enclosed with quotation marks \("\). For example, in `select “a” a1, ‘a’ a2, a a3`, `a1` and `a3`represent a variable, and `a2` represents constant `a`.

|**Function**|**Description**|
|abs\(number\)|Returns the absolute value of a number.|
|asin\(number\)|Returns the asin of a number.|
|attribute\(key\)|Returns the device tag that corresponds with the key. If the device is a dependent device and you use SQL to call this function, the returned result is empty.|
|concat\(string1, string2\)| Concatenates two strings.

 Example: concat\(field,’a’\).

 |
|cos\(number\)|Returns the cosine of a number.|
|cosh\(number\)|Returns the hyperbolic cosine of a number.|
|crypto\(field,String\)| Encrypts the value of a field.

 The String parameter represents an algorithm. Available algorithms include MD2, MD5, SHA1, SHA-256, SHA-384, and SHA-512.

 |
|deviceName\(\)|Returns the name of the current device. If the device is a dependent device and you use SQL to call this function, the returned result is empty.|
|endswith\(input, suffix\)|Validates whether the input string ends with the suffix string.|
|exp\(number\)|Returns a specific value raised to the power of a number.|
|floor\(number\)|Rounds a number down, toward zero, to the nearest multiple of significance. Returns an integer that is equal|
|log\(n, m\)| Returns the logarithm of a number to the base that you have specified.

 If you do not specify m, log\(n\) is returned.

 |
|lower\(string\)|Returns a lower-case string.|
|mod\(n, m\)|Returns the remainder after a number has been divided by a divisor.|
|nanvl\(value, default\)| Returns the value of a property.

 If the value of the property is null, the function returns default.

 |
|newuuid\(\)|Returns a random UUID.|
|payload\(textEncoding\)| Returns the payload of encoding the text in the message sent by a device.

 The default encoding is UTF-8, which means that payload\(\) and payload\(‘utf-8’\) will return the same result.

 |
|power\(n,m\)|Raises number n to power m.|
|rand\(\)|Returns a random number greater than or equal to 0 and less than 1.|
|replace\(source, substring, replacement\)| Replaces a specific column.

 Example: replace\(field,’a’,’1’\).

 |
|sin\(n\)|Returns the sine of n.|
|sinh\(n\)|Returns the hyperbolic sine of n.|
|tan\(n\)|Returns the tangent of n.|
|tanh\(n\)|Returns the hyperbolic tangent of n.|
|timestamp\(format\)| Returns the current system time.

 The format parameter is optional. If you do not set this parameter, the function returns the current system timestamp in milliseconds. Example: timestamp\(\) = 1232323233, timestamp\(‘yyyy-MM-dd HH:mm:ss.SSS’\)=2016-05-30 12:00:00.000.

 |
|topic\(number\)| Returns a segment of a topic.

 For example, for topic /abcdef/ghi, function topic \(\) returns “ /abcdef/ghi”. Function topic \(1\) returns “abcdef”. Function topic\(2\) returns “ghi”.

 |
|upper\(string\)|Returns an upper-case string.|
|to\_base64\(\*\)|If the original payload data is binary data, you can call this function to convert the binary data to a base64 string.|

