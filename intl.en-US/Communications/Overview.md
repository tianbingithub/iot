---
keyword: [Internet of Things, IoT, communication]
---

# Overview

After your devices and server are connected to IoT Platform, the devices and server can communicate by using IoT Platform.

## Devices send data to IoT Platform

After you connect devices to IoT Platform, the devices can communicate with IoT Platform. The devices can use either of the following ways to send data to IoT Platform:

-   Use custom topics to send custom-format data.
    1.  On the IoT Platform console, set **Allowed Operations** of a custom topic to **Publish**. Product topics are automatically mapped to the devices under the product.

        You can create custom topics by using either of the following methods:

        -   Use the IoT Platform console. For more information, see [Customize a topic category](/intl.en-US/Device Access/Topics/Customize a topic category.md).
        -   Use the [IoT Platform SDK](/intl.en-US/Developer Guide (Cloud)/SDK reference/Download SDKs.md). You can call the [CreateProductTopic](/intl.en-US/Developer Guide (Cloud)/API reference/Manage Topics/CreateProductTopic.md) API operation to create a custom topic.
    2.  Configure devices to send messages to custom topics when you develop the devices.

        You must configure custom topics and message formats for sending messages on devices. For information about how to use the Link SDK that is provided by IoT Platform, see [The server receives messages from the device](/intl.en-US/Best Practices/Communications/Use custom topics for communication.md).

-   Use Thing Specification Language \(TSL\)-specific topics to send data.

    For more information about TSL features, see [What is a TSL model?](/intl.en-US/Device Management/TSL/What is a TSL model?.md).

    Devices can submit properties and events.

    Procedure

    1.  In the IoT Platform console, define TSL features based on your business requirements. For more information, see [Add a TSL feature](/intl.en-US/Device Management/TSL/Add a TSL feature.md).
    2.  When you develop your devices, configure the devices to submit properties and events based on the defined TSL features.

        For more information about the standard data formats for properties and events that are submitted by devices, see [Report device properties](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device properties, events, and services.md) and [Submit device events](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device properties, events, and services.md).

        **Note:** IoT Platform provides predefined TSL-specific topics. You can directly use these topics.

        For information about how to use the Link SDK that is provided by IoT Platform, see [Report properties and events by using the device SDK](/intl.en-US/Best Practices/Communications/TSL model communication.mdsection_l5d_7u3_9hp).


## IoT Platform forwards data to enterprise servers

You can configure IoT Platform to use either of the following methods to forward data to your server. The data that can be forwarded includes messages submitted by devices, changes of device status, changes of device lifecycles, historical TSL data, firmware update status, information of new sub-devices that are discovered by gateways, and changes of device topologies. Messages are forwarded based on topics. For more information about the data formats of topics, see [Data formats](/intl.en-US/Communications/Data formats.md).

![Forwards data to the server](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8085929951/p127775.jpg)

-   Server-side subscription: You can use the server-side subscription feature provided by IoT Platform to subscribe to messages of one or more types. IoT Platform can forward messages of specified types from all devices under the product to your server based on your subscription settings. You can use either of the following methods to configure a server-side subscription:
    -   Use the AMQP SDK to receive device data that is forwarded by IoT Platform. For more information, see [AMQP server-side subscription](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Configure AMQP server-side subscriptions.md).
    -   Use the Message Service \(MNS\) SDK to receive device data that is forwarded by IoT Platform to MNS queues. For more information, see [MNS server-side subscription](/intl.en-US/Communications/Server-side Subscription/Use MNS server-side subscription.md).
-   Data forwarding: You can use the data forwarding feature of the rules engine to forward specified device data to MNS topics or RocketMQ queues based on data forwarding rules. You can receive messages on your server by using the MNS SDK or RocketMQ SDK.
    1.  Create a rule to forward device data to an MNS topic or RocketMQ queue. For more information, see [Configure data forwarding rules](/intl.en-US/Communications/Data Forwarding/Configure data forwarding rules.md).
    2.  Use the MNS SDK to receive messages. For more information, see [Manage MNS topics](https://www.alibabacloud.com/help/doc-detail/32450.htm).

For information about the differences between data forwarding and server-side subscription, see [Compare data forwarding solutions](/intl.en-US/Communications/Data Forwarding/Compare data forwarding solutions.md).

## Perform remote control on devices

You can use the IoT Platform SDK on your server to achieve remote control on devices. To perform remote control on devices, you must call API operations to send commands from IoT Platform to the devices. You can use either of the following methods to send commands:

![Control devices](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8085929951/p127779.jpg)

-   Use custom topics.
    -   Asynchronous control: Call the [Pub](/intl.en-US/Developer Guide (Cloud)/API reference/Communications/Pub.md) operation to send custom-format data to a custom topic whose Allowed Operations parameter is set to **Subscribe**. Devices subscribe to this topic to receive messages.

        **Note:** You cannot call the Pub operation to send TSL-related commands.

        For more information about how to use custom topics to control remote devices, see [The server sends messages to the device](/intl.en-US/Best Practices/Communications/Use custom topics for communication.md).

    -   Synchronous control: Call the [RRpc](/intl.en-US/Developer Guide (Cloud)/API reference/Communications/RRpc.md) operation to send messages to specified devices and synchronously retrieves the responses.

        For more information about synchronous MQTT communication, see [What is RRPC?](/intl.en-US/Communications/RRPC/What is RRPC?.md).

        For more information about how to call the RRpc operation for synchronous communication, see [Control Raspberry Pi servers from IoT Platform](/intl.en-US/Best Practices/Communications/Control Raspberry Pi servers from IoT Platform.md).

    -   Batch control: Call the [PubBroadcast](/intl.en-US/Developer Guide (Cloud)/API reference/Communications/PubBroadcast.md) operation to broadcast messages to all online devices.

        For more information about how to achieve batch control, see [Broadcast messages](/intl.en-US/Communications/Broadcast messages.md).

-   Use TSL-specific topics.

    You can use TSL-specific API operations to send property setting or service calling commands from IoT Platform to devices.

    -   Control a single device.
        -   Call the [SetDeviceProperty](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/SetDeviceProperty.md) operation to send a property setting command to a single device.

            After IoT Platform sends the command, the device receives and runs the command in an asynchronous manner. To check whether the device properties are updated, you must view the properties that are later submitted by the device.

        -   Call the [InvokeThingService](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/InvokeThingService.md) operation to send a service calling command to a single device.

            The Invoke Method parameter determines whether a service is called in a synchronous or asynchronous manner. This parameter is specified when you [customize the service](/intl.en-US/Device Management/TSL/Add a TSL feature.mdtable_lhs_fq3_y2b).

            If the Invoke Method parameter is set to **Synchronous**, the response is synchronously returned after the InvokeThingService operation is called.

            If the Invoke Method parameter is set to **Asynchronous**, the response is asynchronously returned after the InvokeThingService operation is called. You can use the rules engine to retrieve the response. You must set the data source of the SQL query for the rule to **TSL Data Reporting** and set the specific topic to `thing/downlink/reply/message`. For more information about how to configure data forwarding rules, see [Configure data forwarding rules](/intl.en-US/Communications/Data Forwarding/Configure data forwarding rules.md).

    -   Control multiple devices.
        -   Call the [SetDevicesProperty](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/SetDevicesProperty.md) operation to send a property setting command to multiple devices.
        -   Call the [InvokeThingsService](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/InvokeThingsService.md) operation to send a service calling command to multiple devices.
    For more information about how to use TSL-specific topics to control remote devices, see [TSL model communication](/intl.en-US/Best Practices/Communications/TSL model communication.md).


## Achieve communication between devices

You can connect devices at two ends to IoT Platform and use IoT Platform to process connection and communication requests between the devices. The following topics describe the two methods that can be used to achieve communication between devices:

-   [Use the rules engine to establish M2M communication](/intl.en-US/Best Practices/Communications/Communication between devices/Use the rules engine to establish M2M communication.md)
-   [Use topic-based message routing to establish M2M communication](/intl.en-US/Best Practices/Communications/Communication between devices/Use topic-based message routing to establish M2M communication.md)

## Sample scenarios of using device data

[Integrate with a weather service](/intl.en-US/Best Practices/Data forwarding/Integrate with a weather service.md)

[Upload temperature and humidity data to DingTalk chatbots](/intl.en-US/Best Practices/Data forwarding/Upload temperature and humidity data to DingTalk chatbots.md)

