# Create a product

The first step when you start using IoT Platform is to create products. A product is a collection of devices that typically have the same features. For example, a product can refer to a product model and a device is then a specific device of the product model.

This topic describes how to create products in the IoT Platform console.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  In the left-side navigation pane, click **Devices** \> **Products**, and then click **Create Product**.

3.  Enter all the required information and then click **Create**.

    The parameters are as follows.

    |Parameter|Description|
    |:--------|:----------|
    |Product Name|The name of the product that you want to create. The product name must be unique within the account. For example, you can enter the product model as the product name. A product name is 4 to 30 characters in length, and can contain Chinese characters, English letters, digits, and underscores. A Chinese character counts as two characters.|
    |Node Type|    -   **Directly connected devices**: Indicates that devices of this product connect to IoT Platform directly.
    -   **Gateway sub-devices**: Indicates that devices of this product cannot connect to IoT Platform directly, but connect to gateway devices and use the communication channels of gateway devices to communication with IoT Platform.
    -   **Gateway devices**: Indicates that devices of this product connect to IoT Platform directly and can be mounted with sub-devices . A gateway can manage sub-devices, maintain topological relationships with sub-devices, and synchronize topological relationships to IoT Platform.
For more information about gateway devices and sub-devices, see [Gateways and sub-devices](/intl.en-US/Device Management/Gateways and sub-devices/Gateways and sub-devices.md). |
    |Gateway Connection Protocol|Select a protocol for sub-device and gateway communication. This field is only for **Gateway sub-devices**.     -   **Custom**: Indicates that you want to use another protocol as the connection protocol for sub-device and gateway communication.
    -   **Modbus**: Indicates that the communication protocol between sub-devices and gateways is Modbus.
    -   **OPC UA**: Indicates that the communication protocol between sub-devices and gateways is OPC UA.
    -   **ZigBee**: indicates that the communication protocol between sub-devices and gateways is ZigBee.
    -   **BLE**: indicates that the communication protocol between sub-devices and gateways is BLE. |
    |Network Connection Method|Select a network connection method for the devices:     -   **Wi-Fi**
    -   **Cellular \(2g/3g/4G/5G\)**
    -   **Ethernet**
    -   **Other** |
    |Data Type|Select a format in which devices exchange data with IoT Platform. Options are **ICA Standard Data Format \(Alink JSON\)** and **Custom**.     -   **ICA Standard Data Format \(Alink JSON\)**: The standard data format defined by IoT Platform for device and IoT Platform communication.
    -   **Custom**: If you want to customize the serial data format, select **Custom**. Custom formatted data must be converted to Alink JSON script by [What is data parsing?](/intl.en-US/Device Management/Data parsing/What is data parsing?.md)so that your devices can communicate with the IoT Platform. |
    |Authentication Mode|Currently, only **Device Secret** is supported.|
    |Product Description|Describe the product information. You can enter up to 100 characters.|


1.  To configure features for a product \(such as [Notifications](/intl.en-US/Device Access/Topics/What is a topic.md), [TSL \(Define Feature\)](/intl.en-US/Device Management/TSL/What is a TSL model?.md), and [Server-side Subscription](/intl.en-US/Communications/Server-side Subscription/What is server-side subscription?.md)\), go to the product list, find the target product and then click its corresponding **View** button.
2.  Register devices on IoT Platform.
3.  Develop your physical devices by referring to [Developer Guide \(Devices\)](/intl.en-US/Device Access/Download device SDKs.md).
4.  To publish a product, go to the product details page and click **Publish**.

    Note that before you publish a product, you must make sure that you have configured all the correct information for the product, have completed debugging the features, and have verified that it meets the criteria for being published.

    When the product status is **Published**, you can view the product information but cannot modify or delete the product.

    To cancel the publishing of a product, click **Cancel Publishing**.


