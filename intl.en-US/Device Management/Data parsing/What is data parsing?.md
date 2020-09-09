---
keyword: [Internet of Things, IoT, IoT Platform, data format, parse, script, pass-through data, custom format, Alink]
---

# What is data parsing?

The standard data format defined in IoT Platform is Alink JSON. For low-end devices with limited resources or devices that require high network throughput, using JSON data to communicate with IoT Platform is inappropriate. You can transmit raw data to IoT Platform. IoT Platform provides the data parsing feature that converts data between device-specific formats and the JSON format based on the scripts you have submitted.

The following two types of data can be parsed:

-   Upstream data from custom topics. When devices submit data to the cloud by using custom topics, the payloads are parsed into JSON data.
-   Upstream and downstream TSL data. IoT Platform parses the custom TSL data submitted by devices into Alink JSON data and parses Alink JSON data sent from the cloud into custom format data.

## Parse data from custom topics

After a device submits data by using a custom topic that is attached with the `? _sn=default` tag, IoT Platform calls the data parsing script that you submitted to the console to convert the custom format payloads into JSON data for subsequent processing.

The following figure shows the procedure of data parsing.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8192039951/p76198.png)

The following figure shows the procedure of using custom topics to submit data.

![Parse custom topic data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8987369951/p162999.jpg)

For information about how to write a script for parsing custom topic data, see the following topics:

[Submit scripts for parsing data](/intl.en-US/Device Management/Data parsing/Parse custom topic data/Submit scripts for parsing data.md)

[JavaScript example](/intl.en-US/Device Management/Data parsing/Parse custom topic data/JavaScript example.md)

[Python example](/intl.en-US/Device Management/Data parsing/Parse custom topic data/Python example.md)

[PHP example](/intl.en-US/Device Management/Data parsing/Parse custom topic data/PHP example.md)

## Parse Thing Specification Language \(TSL\) data

When a device of a product whose **Data Format** parameter is set to **Custom** communicates with the cloud, IoT Platform calls the data parsing script that you submitted to parse upstream data into standard Alink JSON data and downstream data into custom format data.

After receiving data from the device, IoT Platform runs the script to convert the pass-through data into Alink JSON data for subsequent processing. Before sending data to the device, IoT Platform runs the script to convert the data into custom format data.

The following figure shows the procedure of data parsing.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9192039951/p7506.png)

The following figure shows the procedure of submitting pass-through property or event data \(upstream data\).

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9192039951/p73933.png)

The following figure shows the procedure of calling device services or configuring device properties \(downstream data\).

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2392039951/p11793.jpeg)

References

For information about sample scripts, see [Example for parsing TSL data](/intl.en-US/Device Management/Data parsing/TSL Data Parsing/Example for parsing TSL data.md), [JavaScript example](/intl.en-US/Device Management/Data parsing/TSL Data Parsing/JavaScript example.md),[Python example](/intl.en-US/Device Management/Data parsing/TSL Data Parsing/Python example.md), and[PHP example](/intl.en-US/Device Management/Data parsing/TSL Data Parsing/PHP example.md).

