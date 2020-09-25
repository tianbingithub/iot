---
keyword: [Internet of Things, IoT, IoT Platform, create, register, device]
---

# Create a device

A product consists of multiple devices of the same type. After you create a product, you must create devices for the product. You can create a device or multiple devices at a time. This article describes how to create a device at a time.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Devices** \> **Devices**.

3.  On the Devices page, click **Add Device**.

4.  In the Add Device dialog box, set the required parameters, and click **OK**.

    ![Add Device dialog box](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2477929951/p2540.png)

    |Parameter|Description|
    |:--------|:----------|
    |Products|Select a product. The device derives the features and properties of the specified product. **Note:** If the product is registered with another platform, make sure that your account has sufficient activation codes to create the device. |
    |DeviceName|The device name.     -   The device name must be unique within the product.
    -   The device name must be 4 to 32 characters in length, and can contain letters, digits, hyphens \(-\), underscores \(\_\), at signs \(@\), periods \(.\), and colons \(:\).
**Note:** You can leave the DeviceName parameter unspecified. If the parameter is left unspecified, IoT Platform generates a globally unique identifier \(GUID\) as the device name. |
    |Alias|The alias. The alias must be 4 to 64 characters in length, and can contain letters, digits, and underscores \(\_\).|


After a device is created, the View Device Certificate dialog box appears. You can view and copy the device certificate information. A device certificate consists of a product key, device name, and device secret. A device certificate is the credential that the device uses to communicate with IoT Platform. We recommend that you keep your device certificates confidential.

|Parameter|Description|
|:--------|:----------|
|ProductKey|The key of the product to which the device belongs. It is the GUID that is issued by IoT Platform to the product.|
|DeviceName|The device name. The identifier of the device. The identifier is unique within the product. DeviceName and ProductKey uniquely identify the device, and are used for authentication with IoT Platform.|
|DeviceSecret|The device key that is issued by IoT Platform for device authentication and encryption. It must be used in combination with the device name.|

View device information. For more information, see [Manage devices](/intl.en-US/Device Access/Create devices/Manage devices.md).

The status of the created devices is **Not Activated**. Use a Link SDK to implement a device, connect the device to IoT Platform, and then activate the device. For more information, see the [Link SDK documentation](https://www.alibabacloud.com/help/product/93051.htm).

Connect the device to IoT Platform. For more information about best practices, see the following articles:

-   [Access IoT Platform by using MQTT.fx](/intl.en-US/Best Practices/Device access/Access IoT Platform by using MQTT.fx.md)
-   [Connect Android Things to IoT Platform](/intl.en-US/Best Practices/Device access/Connect Android Things to IoT Platform.md)
-   [t127995.md\#]()

