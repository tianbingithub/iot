---
keyword: [Internet of Things, IoT Platform, IoT, device, message, communication, topic, topic category, system topic, custom topic]
---

# What is a topic?

IoT Platform communicates with devices based on topics. Topics are associated with devices, and topic categories are associated with products. A topic category of a product is used by all devices within the product to generate device-specific topics for messaging between devices and IoT Platform.

## Topic categories

Topic categories are used to simplify authorization and improve topic-based communication between devices and IoT Platform A topic category consists of a set of topics. For example, a custom topic category named `/${YourProductKey}/$ {YourDeviceName}/user/update` consists of a set of topics, such as `/${YourProductKey}/device1/user/update` and `/${YourProductKey}/device2/user/update`.

After devices are created for a product, all topic categories of the product are accessible to the devices. You do not need to create topics for each device.

![Topic](../images/p35287.png "Procedure for generating topics")

In the [IoT Platform console](https://iot.console.aliyun.com), choose **Devices** \> **Products**, find a product, and then click **View**. On the Product Details page, click the **Topic Categories** tab, and view the required topic category on the **Topics for Basic Communications**, **Topics for TSL Communications**, or **Topic Category** tab. For more information about the three types of topic category, see [Topic types](#section_0e7_4fw_ppz).

When you use a topic category, note the following items:

-   A topic category consists of several fields. Separate these fields with forward slashes \(/\). A topic category contains the following pre-defined fields: $\{YourProductKey\} and $\{YourDeviceName\}. Replace the $\{YourProductKey\} variable with a product key. Replace the $\{YourDeviceName\} variable with a device name.
-   Permissions:
    -   **Publish**: specifies that you can publish messages to the topic.
    -   **Subscribe**: specifies that you can subscribe to the topic to receive messages.

        The SUB and UNSUB requests must be submitted by devices. After a device sends a SUB request to subscribe to a topic, the subscription is permanently valid. The subscription is canceled only when the device sends an UNSUB request to the topic.


## Topics

A topic category is used for topic definition rather than messaging. Only topics can be used for messaging.

After a device is created, all topics for the device are generated based on the topic categories of the product to which the device belongs. Topics are in the same format as topic categories. The difference between topics and topic categories is that the $\{YourDeviceName\} variable in topic categories is replaced with a real device name in topics.

Topics are used by devices only for messaging. For example, the `/${YourProductKey}/device1/user/update` topic belongs to a device named Device 1. Only Device 1 can publish messages and subscribe to this topic. Other devices cannot use this topic.

After a device sends a SUB request to subscribe to a topic, perform the following steps to view the subscribed topic. Log on to the [IoT Platform console](https://iot.console.aliyun.com), choose **Devices** \> **Devices**, find the device, and then click **View**. On the Device Details page, click the **Topic List** tab. All subscribed topics appear in the **Subscribed Topics** section of the tab. IoT Platform can send upstream messages to these topics.

You can click **Post Message** next to a topic of a device and send a message to the device by using the topic. The post message action is unavailable for topics that include wildcards. For more information, see [A topic that includes one or more wildcards](/intl.en-US/Device Access/Topics/Customize a topic category.md).

A device can send a UNSUB request to unsubscribe from a topic. After the topic is unsubscribed, it is removed from the **Subscribed Topics** section.

## Topic types

Topics are categorized into the following three types.

|Type|Description|
|:---|:----------|
|Topics for basic communications|Topics for basic communications are predefined in IoT Platform. Topics of this type include the following topics: -   Firmware update-related topics. For more information about the purpose and data format of each topic, see [Firmware update](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Firmware update.md).
-   Device tag-related topics. For more information about the purpose and data format of each topic, see [Device tags](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device tags.md).
-   Clock synchronization-related topics. Clock synchronization is implemented by using the Network Time Protocol \(NTP\) service. For more information, see [Configure the NTP service](/intl.en-US/Device Management/Configure the NTP service.md).
-   Device shadow-related topics. For more information about the purpose and data format of each topic, see [Device shadow data stream](/intl.en-US/Device Management/Device shadows/Device shadow data stream.md).
-   Configuration update-related topics. For more information about the purpose and data format of each topic, see [Remote configuration](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Remote configuration.md).
-   Broadcast topics. You can call the [PubBroadcast](/intl.en-US/Developer Guide (Cloud)/API reference/Communications/PubBroadcast.md) API operation of IoT Platform to broadcast messages to devices that subscribe to a broadcast topic. This allows you to batch control the devices. |
|Topics for TSL communications|Topics for Thing Specification Language \(TSL\) communications are predefined in IoT Platform. Topics of this type include the following topics: For more information about the data format of each TSL-based topic, see [Device properties, events, and services](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device properties, events, and services.md). **Note:** You cannot call the Pub API operation of IoT Platform to send messages to a TSL-based topic.

IoT Platform allows you to perform the following operations to control remote devices by using TSL-based topics: 1. Call the [SetDeviceProperty](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/SetDeviceProperty.md) or [SetDevicesProperty](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/SetDevicesProperty.md) API operation to set the values of device properties. 2. Call the [InvokeThingService](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/InvokeThingService.md) or [InvokeThingsService](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/InvokeThingsService.md) API operation to invoke device services. |
|Custom topics|You can customize topics on the **Topic Categories** tab based on your business requirements. For more information, see [Customize a topic category](/intl.en-US/Device Access/Topics/Customize a topic category.md).A topic category is a configuration template that is used to generate topics. Each topic category of a product is used by all devices within the product. If you edit and update a topic category, the change applies to all topics that are generated by the topic category. This may affect messaging between devices that subscribe to these topics and IoT Platform. We recommend that you complete the configuration of topic categories when you implement devices. We also recommend that you do not modify or adjust the topic categories after these devices are connected to IoT Platform. |

