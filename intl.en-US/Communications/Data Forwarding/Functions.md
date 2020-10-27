---
keyword: [Internet of Things, IoT Platform, IoT, rules engine, forward data, send data, data forwarding rules, SQL, filter data, function]
---

# Functions

The rules engine provides functions that you can use in SQL statements to process data.

## Functions supported by the data forwarding feature

|Function|Description|
|:-------|:----------|
|abs\(number\)|Returns the absolute value of a number.|
|asin\(number\)|Returns the arcsine of a number.|
|attribute\(key\)|Returns the device tag value of a tag key. If the tag with the specified key is not attached to a device, no tag value is returned. When you debug your SQL statement, a null string is returned because no real device or tag exists.|
|concat\(string1, string2\)|Concatenates strings and returns a result. Example: `concat(field,’a’)`. |
|cos\(number\)|Returns the cosine of a number.|
|cosh\(number\)|Returns the hyperbolic cosine of a number.|
|crypto\(field,String\)|Encrypts the value of a field.

The String parameter specifies an encryption algorithm. Available algorithms include MD2, MD5, SHA1, SHA-256, SHA-384, and SHA-512. |
|deviceName\(\)|Returns the name of a device. When you debug your SQL statement, a null string is returned because no real device exists.|
|endswith\(input, suffix\)|Checks whether the input string ends with the suffix string.|
|exp\(number\)|Returns the value of the mathematical constant e raised to the power of a specified number.|
|floor\(number\)|Returns the largest integer that is less than or equal to a specified number.|
|log\(n, m\)|Returns the logarithm of the number n to the base m.

If you do not specify m, the default base 10 is used. In this case, `log(n)` is returned. |
|lower\(string\)|Converts all letters in a specified string into lowercase letters and returns the lowercase string.|
|mod\(n, m\)|Returns the remainder after number n is divided by divisor m.|
|nanvl\(value, default\)|Returns the value of a property.

The value parameter specifies the name of the property. If the value of the property is null, the function returns the value of the default parameter. |
|newuuid\(\)|Returns a random UUID.|
|payload\(textEncoding\)|Returns the message payload that is sent by a device. The payload is encoded by using the encoding scheme that is specified by the textEncoding parameter.

The default encoding scheme is UTF-8. The default value indicates that `payload()` and `payload(‘utf-8’)` return the same result. |
|power\(n,m\)|Returns the value of number n raised to the power of m.|
|rand\(\)|Returns a random number that is greater than or equal to 0 and less than 1.|
|replace\(source, substring, replacement\)|Replaces the substring in the source column with the replacement string.

Example: `replace(field,’a’,’1’)`. |
|sin\(n\)|Returns the sine of the number n.|
|sinh\(n\)|Returns the hyperbolic sine of the number n.|
|tan\(n\)|Returns the tangent of the number n.|
|tanh\(n\)|Returns the hyperbolic tangent of the number n.|
|thingPropertyFlatMap\(property\)|Returns the values of a property in a Thing Specification Language \(TSL\) model. If a property has multiple values, the values are separated with underscores \(\_\). If a TSL model contains more than 50 properties, the data forwarding feature cannot forward the entire TSL model. You can use this function to extract property data from the TSL model. This way, all properties of the TSL model can be forwarded to other Alibaba Cloud services. You can specify multiple properties as the input parameters of the function. If you do not specify a property, all values of properties are returned.

For example, the function `thingValueFlat("Power", "Position")` adds `"Power": "On", "Position_latitude": 39.9, "Position_longitude": 116.38` to a message body. |
|timestamp\(format\)|Returns the timestamp of the current system time in a specified format.

The format parameter is optional. If you do not specify the format parameter, the UNIX timestamp of the current system time is returned. For example, `timestamp()` returns `1543373798943`. If you specify the format parameter, a timestamp in the specified format is returned. For example, `timestamp('yyyy-MM-dd\'T\'HH:mm:ss\'Z\'')` returns `2018-11-28T10:56:38Z`. |
|timestamp\_utc\(format\)|Returns the UTC timestamp of the current system time in a specified format. The format parameter is optional. If you do not specify the format parameter, the UNIX timestamp of the current system time is returned. For example, `timestamp_utc()` returns `1543373798943`. If you specify the format parameter, a timestamp in the specified format is returned. For example, `timestamp_utc('yyyy-MM-dd\'T\'HH:mm:ss\'Z\'')` returns `2018-11-28T02:56:38Z`. |
|topic\(number\)|Returns the topic information in a specified level.

For example, a topic is named /alDbcLe\*\*\*\*/TestDevice/user/. `topic()` returns the complete topic name /alDbcLe\*\*\*\*/TestDevice/user/set. `topic(1)` returns the first level `alDbcLe****`. `topic(2)` returns the second level `TestDevice`. |
|upper\(string\)|Converts all letters in a specified string into uppercase and returns the uppercase string. For example, `upper(alibaba)` returns `ALIBABA`. |
|to\_base64\(\*\)|Converts the message payload from binary data into a Base64-encoded string.|
|messageId\(\)|Returns the message ID that is generated by IoT Platform .|
|substring\(target, start, end\)|Returns part of a string. Parameters:

-   target: the original string. This parameter is required.
-   start: the index from which the returned characters start. The character at the start index is also returned. This parameter is required.
-   end: the index at which the returned characters end. The character at the end index is not returned. This parameter is optional.

**Note:**

-   Only data of the string and integer types is supported. An integer is converted into a string before being processed.
-   Enclose a constant string in a pair of single quotation marks \('\). The data enclosed in double quotation marks \("\) is parsed as integers.
-   If an input parameter is invalid, for example, the data type is not supported, SQL parsing fails and the rules are not run.

Examples:

-   substring\('012345', 0\) = "012345"
-   substring\('012345', 2\) = "2345"
-   substring\('012345', 2.745\) = "2345"
-   substring\(123, 2\) = "3"
-   substring\('012345', -1\) = "012345"
-   substring\(true, 1.2\) error
-   substring\('012345', 1, 3\) = "12"
-   substring\('012345', -50, 50\) = "012345"
-   substring\('012345', 3, 1\) = "" |
|to\_hex\(\*\)|Converts the message payload from binary data into a hexadecimal string.|

## Example

You can call functions to obtain or process data in the SELECT and WHERE clauses of SQL statements.

For example, a temperature sensor product has the Temperature property. The following script shows the TSL property data that is submitted by a device.

```
{
    "deviceType": "Custom",
    "iotId": "H5KURkKdibnZvSls****000100",
    "productKey": "a1HHrkm****",
    "gmtCreate": 1564569974224,
    "deviceName": "TestDevice1",
    "items": {
        "Temperature": {
            "value": 23.5,
            "time": 1564569974229
        }
    }
}
```

The temperature sensor product has multiple devices. The device names are TestDevice1, TestDevice2, and TestDevice3. The temperature property is forwarded to Function Compute for processing only if the submitted property value is greater than 38. The following figure shows the SQL statement of the rule that is used to filter submitted device data.

![Edit an SQL statement](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7248773061/p176755.png)

SQL statement:

```
SELECT deviceName() as deviceName,items.Temperature.value as Temperature FROM "/sys/a1HHrkm****/+/thing/event/property/post" WHERE items.Temperature.value>38 and deviceName() in ('TestDevice1', 'TestDevice2', 'TestDevice3')
```

In this example, the deviceName\(\) function is used.

-   This function is used in the SELECT clause to filter a device name from the submitted data.
-   This function is used in the WHERE clause to specify a condition. `in ('TestDevice1', 'TestDevice2', 'TestDevice3')` indicates that the property data is forwarded only if the device name is TestDevice1, TestDevice2, or Testdevice3.

For information about how to write the SELECT and WHERE clauses in an SQL statement and about the conditional expressions supported by the rules engine, see [SQL statements](/intl.en-US/Communications/Data Forwarding/SQL statements.md).

