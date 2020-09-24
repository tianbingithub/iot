---
keyword: [Internet of Things, IoT, IoT Platform, device, device certificate, authenticate connection, unique-certificate-per-device, security authentication, ProductKey, DeviceName, DeviceSecret, device activation]
---

# Unique-certificate-per-device authentication

If the unique-certificate-per-device authentication method is used, you must install a unique device certificate on each device in advance. The device certificate includes a product key, device name, and device secret. When you connect a device to IoT Platform, IoT Platform authenticates the device certificate. After the device passes authentication, IoT Platform activates the device to enable data communication between the device and IoT Platform.

The unique-certificate-per-device authentication method is recommended because of its high level of security.

Procedure:

![Device authentication](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4477929951/p32767.png)

1.  Create a product.

    Create a product in the [IoT Platform console](http://iot.console.aliyun.com/). For more information, see [Create a product](/intl.en-US/Device Access/Create a product.md).

2.  Add a device.

    Add a device to an existing product and obtain the device certificate information. For more information, see [Create a device](/intl.en-US/Device Access/Create devices/Create a device.md) and [Create multiple devices at a time](/intl.en-US/Device Access/Create devices/Create multiple devices at a time.md).

3.  Burn the device certificate information onto the device.

    1.  Download a [Link SDK](https://www.alibabacloud.com/help/doc-detail/96627.htm).

    2.  Initialize the Link SDK. Specify the device certificate information in the Link SDK.

        Initialize the Link SDK in which the unique-certificate-per-device authentication method is specified. For more information, see the device authentication, and authentication and connection articles of language-specific Link SDKs in the [Link SDK](https://help.aliyun.com/document_detail/96596.html) documentation.

    3.  Develop the device SDK based on your business needs. For example, you can develop the following features: over-the-air \(OTA\) update, sub-device connection, Thing Specification Language \(TSL\), and device shadows.

    4.  Burn the developed device SDK to the device on the production line.

4.  Connect the device to IoT Platform.

    After you power on the device and connect the device to IoT Platform, the device submits an authentication request that includes the device certificate information to IoT Platform. For more information, see [Establish MQTT connections over TCP](/intl.en-US/Device Access/Protocols for connecting devices/Use MQTT protocol/Establish MQTT connections over TCP.md), [Establish connections over CoAP](/intl.en-US/Device Access/Protocols for connecting devices/Use CoAP protocol/Establish connections over CoAP.md), and [Establish connections over HTTP](/intl.en-US/Device Access/Protocols for connecting devices/Use HTTP protocol/Establish connections over HTTP.md).

5.  Activate the device in the IoT Platform console.

    IoT Platform authenticates the device certificate. After the device passes authentication and connects with IoT Platform, the device can publish messages to topics and subscribe to topic messages. This enables messaging between the device and IoT Platform.


