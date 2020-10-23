---
keyword: [Internet of Things, IoT, IoT Platform, TSL model, TSL, feature definition, property, event, service, add a TSL feature]
---

# Add a TSL feature

You can add a Thing Specification Language \(TSL\) feature for a product. A feature can be a property, event, or service. This article describes how to add a TSL feature in the IoT Platform console.

-   If a product is published, you cannot edit the TSL model of the product. To edit a TSL model, you must unpublish the product for which the TSL model is defined.
-   You can edit a historical version of a TSL model to generate a new version.
-   IoT Platform can save the latest 10 versions of a TSL model. Earlier versions are overwritten.
-   After you edit a TSL model, you must release the TSL model to apply the update.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Devices** \> **Products**.

3.  On the Products page, find the product for which you want to add features, and click **View** in the Actions column.

4.  On the Product Details page, click the **Define Feature** tab, and then click **Edit Draft**.

5.  Optional. Select a historical version from the Version History drop-down list, and click **Restore to This Version**. Then, you can edit the TSL model based on the historical version.

6.  Add a self-defined feature.

    Click **Add Self-defined Feature**. Then, you can add a property, service, or event to the product.

    -   Add a property: In the Add Self-defined Feature dialog box, select **Properties** in the Feature Type field. Set the required parameters, and click **OK**.

        ![Properties](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6192039951/p107808.png)

        The following table describes the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Feature Name|The name of the property, for example, power consumption. The name must be unique for a product.

The name must be 1 to 30 characters in length, and can contain letters, digits, hyphens \(-\), underscores \(\_\), forward slashes \(/\), and periods \(.\). It must start with a letter or a digit. |
        |Identifier|The ID of the property. The ID must be unique for a product. The ID is indicated by the value of the identifier parameter in the Alink JSON format. A device uses the ID as the key to report property data, and IoT Platform verifies the ID to determine whether to receive the data. The ID must be 1 to 50 characters in length, and can contain letters, digits, and underscores \(\_\). For example, you can set the ID to PowerComsumption. **Note:** You cannot set this parameter to one of the following reserved words: set, get, post, time, and value. |
        |Data Type|        -   int32: 32-bit integer. You must specify a value range, step size, and unit.
        -   float: single-precision floating-point number. You must specify a value range, step size, and unit.
        -   double: double-precision floating-point number. You must specify a value range, step size, and unit.
        -   enum: enumeration. You must specify a value and description for an ENUM item. Examples: 1-heating mode and 2-cooling mode.
        -   bool: Boolean. You must use the numbers 0 and 1 to define Boolean values. Examples: 0-off and 1-on.
        -   text: string. You must specify the length of the string. The string must be 1 to 10,240 bytes in length.
        -   date: timestamp. A UTC timestamp. Data type: string. Unit: ms.
        -   struct: JSON object. You must define a JSON struct and add JSON parameters to the struct. For example, you can define the color of a lamp as a struct that is composed of the following three parameters: Red, Green, and Blue. Nested structs are not supported.
        -   array: array. You must specify the data type and number of the elements in an array. Valid values: int32, float, double, text, and struct. Make sure that the elements in an array are of the same data type. The number of elements ranges from 1 to 512.
**Note:** If the gateway connection protocol of the product is set to Modbus, you do not need to specify this parameter. |
        |Value Range|If the Data Type parameter is set to int32, float, or double, you must specify a value range for the property.|
        |Step|The minimum granularity by which property values change. If the Data Type parameter is set to int32, float, or double, you must specify a step size based on your business requirements. For example, assume that you add the temperature property to a thermometer product. You can set the Data Type parameter to int32, the Step parameter to 2, the Unit parameter to °C, and the Value Range parameter to 0 to 100. In this case, each time the temperature changes by 2°C, a device reports a temperature value, for example, 0°C, 2°C, 4°C, 6°C, or 8°C. |
        |Unit|You can select None or other values based on the actual conditions.|
        |Read/Write Type|        -   Read/Write: Both GET and SET requests are supported.
        -   Read-only: Only GET requests are supported.
**Note:** If the gateway connection protocol of the product is set to Modbus, you do not need to specify this parameter. |
        |Description|The description of the feature. The description must be 1 to 100 characters in length.|
        |Extended Information|The mapping between the device connection protocol and the standard TSL model. After you configure the extended settings, you can view these settings on the TSL Extension Information tab of the TSL model in the IoT Platform console.

If the gateway connection protocol is set to Custom, OPC UA, or Modbus, you must specify this parameter.

        -   If the gateway connection protocol is set to Custom, enter a JSON-formatted description for custom configurations. The description must be 1 to 1,024 characters in length.
        -   If the gateway connection protocol is set to OPC UA, enter a node name. The node name must be unique in the properties of a product.
        -   If the gateway connection protocol is set to Modbus, set the following parameters:
            -   Operation Type:
                -   Coil Status \(read-only, 01\)
                -   Coil Status \(read and write, 01-read, 05-write\)
                -   Coil Status \(read and write, 01-read, 0F-write\)
                -   Discrete Input \(read-only, 02\)
                -   Holding Registers \(read-only, 03\)
                -   Holding Registers \(read and write, 03-read, 06-write\)
                -   Holding Registers \(read and write, 03-read, 10-write\)
                -   Input Registers \(read-only, 04\)
            -   Register Address: You must specify a hexadecimal value that starts with `0x`. Valid values: `0x0 to 0xFFFF`, for example, `0xFE`.
            -   Original Data Type: Multiple data types are supported. Valid values: int16, uint16, int32, uint32, int64, uint64, float, double, string, and bool.
            -   Value Range: The value range is retrieved after the original data is processed by using a specified scale factor. Data that exceeds the value range is discarded. IoT Platform sets the default value ranges for the following operation types:
                -   Coil Status: 0 to 1
                -   Discrete Input: 0 to 1
                -   Holding Registers: -2147483648 to 2147483647
                -   Input Registers: -2147483648 to 2147483647
            -   Switch High Byte and Low Byte in Register: specifies whether to swap the first and last 8 bits of the 16-bit integer in the register. Valid values:
                -   true: The first and last 8 bits are swapped.
                -   false: The first and last 8 bits are not swapped.
            -   Switch Register Bits Sequence: specifies whether to swap the first and last 16 bits of the original 32-bit integer. Valid values:
                -   true: The first and last 16 bits are swapped.
                -   false: The first and last 16 bits are not swapped.
            -   Zoom Factor: You can set this parameter to a negative number. You cannot set this parameter to 0. Default value: 1.
            -   Data Report: You can select At Specific Time or Report Changes. |

    -   Add a service: In the Add Self-defined Feature dialog box, select **Services** in the Feature Type field. Set the required parameters and click **OK**.

        **Note:** If the gateway connection protocol is set to Modbus, you cannot add services.

        ![Services](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6192039951/p107812.png)

        The following table describes the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Feature Name|The service name. The name must be 1 to 30 characters in length, and can contain letters, digits, hyphens \(-\), underscores \(\_\), forward slashes \(/\), and periods \(.\). It must start with a letter or a digit. |
        |Identifier|The ID of the service. The ID must be unique for a product. The ID is indicated by the value of the identifier parameter that is specified for the service in the Alink JSON format. The ID must be 1 to 50 characters in length, and can contain letters, digits, and underscores \(\_\). **Note:** You cannot set this parameter to one of the following reserved words: set, get, post, time, and value. |
        |Invoke Method|        -   Asynchronous: IoT Platform returns the result after the call is executed. IoT Platform does not wait for a response from the device.
        -   Synchronous: IoT Platform waits for a response from the device. If no response is returned, the call times out. |
        |Input Parameters|Optional. The input parameters of the service. Click **+Add Parameter**, and add an input parameter in the dialog box that appears.

If the gateway connection protocol is set to OPC UA, you must set a parameter index to specify the order of the parameters.

**Note:**

        -   You cannot set this parameter to one of the following reserved words: set, get, post, time, and value.
        -   You can use a property as an input parameter or customize an input parameter. For example, when you specify the Automatic Sprinkling service, you can use the Sprinkling Interval and Sprinkling Amount properties as the input parameters. When IoT Platform calls the Automatic Sprinkling service, the sprinkler automatically starts sprinkling based on the specified sprinkling interval and amount.
        -   You can add a maximum of 20 input parameters to a service. |
        |Output Parameters|Optional. The output parameters of the service. Click **+Add Parameter**, and add an output parameter in the dialog box that appears.

If the gateway connection protocol is set to OPC UA, you must set a parameter index to specify the order of the parameters.

**Note:**

        -   You cannot set this parameter to one of the following reserved words: set, get, post, time, and value.
        -   You can use a property as an output parameter or customize an output parameter. For example, when you specify the Automatic Sprinkling service, you can use the Soil Humidity property as an output parameter. When IoT Platform calls the Automatic Sprinkling service, the data about soil humidity is returned.
        -   You can add a maximum of 20 output parameters to a service. |
        |Extended Information|The mapping between the device connection protocol and the standard TSL model. After you configure the extended settings, you can view these settings on the TSL Extension Information tab of the TSL model in the IoT Platform console.

If the gateway connection protocol is set to Custom or OPC UA, you must specify the extended information. If the gateway connection protocol is set to Custom, enter a JSON-formatted description for custom configurations. The description must be 1 to 1,024 characters in length.

If the gateway connection protocol is set to OPC UA, enter a node name. The node name must be unique in the properties of a product. |
        |Description|The description of the feature. The description must be 1 to 100 characters in length.|

    -   Add an event: In the Add Self-defined Feature dialog box, select **Events** in the Feature Type field. Set the required parameters and click **OK**.

        **Note:** If the gateway connection protocol is set to Modbus, you cannot add events.

        ![Events](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6192039951/p107816.png)

        The following table describes the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Feature Name|The name of the event. The name must be 1 to 30 characters in length, and can contain letters, digits, hyphens \(-\), underscores \(\_\), forward slashes \(/\), and periods \(.\). It must start with a letter or a digit. |
        |Identifier|The ID of the event. The ID must be unique for a product. The ID is indicated by the value of the identifier parameter in the Alink JSON format. A device uses the ID as the key to report property data, for example, ErrorCode. The ID must be 1 to 50 characters in length, and can contain letters, digits, and underscores \(\_\).**Note:** You cannot set this parameter to one of the following reserved words: set, get, post, time, and value. |
        |Event Type|You can perform logic processing or analytics for events of different types.         -   Info: Devices report common notifications, such as the completion of a specific task.
        -   Alert: Devices report emergencies or exceptions that occur during device running. Events of this type have a high priority.
        -   Error: Devices report emergencies or exceptions that occur during device running. Events of this type have a high priority. |
        |Output Parameters|The output parameters of an event. Click **+Add Parameter**, and add an output parameter in the dialog box that appears. You can use a property as an output parameter or customize an output parameter. For example, you can use the Voltage property as an output parameter. When a device reports an error, the voltage of the device is also reported for troubleshooting. If the gateway connection protocol is set to OPC UA, you must set a parameter index to specify the order of the parameters.

**Note:**

        -   You cannot set this parameter to one of the following reserved words: set, get, post, time, and value.
        -   You can add a maximum of 50 output parameters to a service. |
        |Extended Information|The mapping between the device connection protocol and the standard TSL model. After you configure the extended settings, you can view these settings on the TSL Extension Information tab of the TSL model in the IoT Platform console.

If the gateway connection protocol is set to Custom or OPC UA, you must specify the extended information. If the gateway connection protocol is set to Custom, enter a JSON-formatted description for custom configurations. The description must be 1 to 1,024 characters in length.

If the gateway connection protocol is set to OPC UA, enter a node name. The node name must be unique in the properties of a product. |
        |Description|The description of the feature. The description must be 1 to 100 characters in length.|

7.  Release the TSL model.

    1.  On the Edit Draft page, click **Release Online**. The Release model online? dialog box appears.

    2.  Optional. Click **+Add post notes**, and enter a version number and note.

        |Parameter|Description|
        |---------|-----------|
        |Version Number|The version number of the TSL model. You can manage the TSL model based on the version number. The version number must be 1 to 16 characters in length, and can contain letters, digits, and periods \(.\). |
        |Note|The description of the TSL model. The description must be 1 to 100 characters in length, and can contain letters, digits, and special characters.|

    3.  f an online version is available, you must check the differences between the current version and the online version.

        Click **View differences**. In the View Differences dialog box, you can view the differences. If the current version is configured as normal, click **Confirm**. In the Release model online? dialog box, the checkbox is automatically selected.

    4.  Click **OK** to release the TSL model.

        **Note:**

        -   A TSL model is applied to the product only after it is released.
        -   IoT Platform can save the latest 10 versions of a TSL model. Earlier versions are overwritten.

After you edit a TSL model, you must release the TSL model to apply the update. On the Define Feature tab of the Product Details page, you can perform the following operations:

-   Click **TSL Model** to view the TSL model in the JSON format.

