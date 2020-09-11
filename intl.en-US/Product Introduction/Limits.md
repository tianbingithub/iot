---
keyword: [Internet of Things, IoT, IoT Platform, IoT application, device, quantity limit, QoS, maximum, most, throttling]
---

# Limits

This article describes the limits of IoT Platform.

## Products and devices

|Item|Description|Limit|
|:---|:----------|:----|
|Tags|The maximum number of tags that you can attach to a product, device, or device group.|100|
|Products|The maximum number of products that an Alibaba Cloud account can create.|1,000|
|Devices|The maximum number of devices that you can add to a product. **Note:**

-   To obtain the number of devices in a product in a timely manner, we recommend that you set an alert threshold for the number of devices. This avoids unexpected errors when you add new devices to the product. For more information, see [Create a threshold-triggered alarm rule](/intl.en-US/Maintenance/Real-time monitoring/Use CloudMonitor to monitor resource usage/Use CloudMonitor to monitor IoT resources.md).
-   If the number of devices exceeds the limit, you must create a product.

|1,000,000|
|The maximum number of devices that you can add to products by using an Alibaba Cloud account. **Note:** If you need to extend the limit based on your business requirements, [submit a ticket](https://selfservice.console.aliyun.com/ticket/createIndex).

|10,000,000|
|Gateways and sub-devices|The maximum number of sub-devices that you can attach to a gateway.|1,500|
|Thing Specification Language \(TSL\) features|The maximum number of features that you can add to a product.|300|
|The maximum number of properties, events, or services that you can add to a product.|256|
|The maximum number of parameters that can be specified for a property of the STRUCT data type.|50|
|The maximum number of enumeration items that can be specified for a feature of the ENUM data type.|100|
|The maximum number of characters that can be specified for a feature of the text data type.|2,048|
|The maximum number of elements that can be specified for a feature of the array data type.|128|
|The maximum number of request or response parameters that can be specified for a service.|20|
|The maximum number of output parameters that can be specified for an event.|50|
|The number of the last versions that can be saved for a single TSL model.|10|
|The maximum size of a file when you import a TSL model.|256 KB|
|Device groups|The maximum number of groups that you can create by using an Alibaba Cloud account. These groups include parent groups and subgroups.|1,000|
|The maximum number of devices that you can add to a group.|20,000|
|The maximum number of groups to which a device can be added.|10|
|Data parsing|The maximum size of a script file that can be uploaded for data parsing.|128 KB|
|Remote configuration|The maximum size of a remote configuration file. The remote configuration file supports only the JSON format.|64 KB|
|Data retention period|The maximum number of days that property, event, and service data can be retained. Data is no longer retained after the specified period ends.|30|
|File management|The maximum size of files that an Alibaba Cloud account can store on IoT Platform servers.|1 GB|
|The maximum number of files that a device can store.|1,000|
|Firmware update|The maximum number of firmware files that an Alibaba Cloud account can contain.|500|
|The maximum size of a firmware file.|1,000 MB|

## Connections and communications

|Item|Description|Limit|
|----|:----------|:----|
|Device access|The maximum number of connections that can be established with IoT Platform at a time if you use the same device certificate information. The device certificate information includes the Productkey and DeviceName parameters.|1|
|Connections|The maximum number of MQTT connection requests that an Alibaba Cloud account can make per second.|500|
|The maximum number of connection requests that a device can make per minute.|5|
|Device subscription|The maximum number of topics to which a device can subscribe. After the limit is exceeded, subscription requests will be rejected. The device can check whether a subscription request succeeds by verifying the received SUBACK message.

|100|
|Requests|The maximum number of requests that the devices of an Alibaba Cloud account can send to IoT Platform per second.|10,000|
|The maximum number of requests that IoT Platform can send to the devices of an Alibaba Cloud account per second.|2,000|
|Service subscription|The maximum number of messages that an Alibaba Cloud Message Queue for AMQP consumer group can receive per second.|1,000|
|Message communication|The maximum number of messages that a device can report per second. **Note:** If a device uses the Pub API to report messages over the MQTT protocol but the messages are blocked due to throttling, no throttling error is returned. However, you can view device logs to find the devices whose messages are blocked due to throttling.

|-   QoS 0: 30 messages per second
-   QoS 1: 10 messages per second |
|The maximum number of messages that a device can receive per second. The limit changes based on the network environment. If the maximum TCP write buffer size is exceeded, an error message is returned. If IoT Platform uses the Pub API operation to send requests to a device and the device cannot process the requests in a timely manner, a throttling error message is returned.

|50 messages per second|
|Bandwidth|The maximum throughput \(bandwidth\) per connection.|1,024 KB|
|Cache requests|The maximum number of unacknowledged message publishing requests from a device. After the limit is reached, IoT Platform rejects new message publishing requests from the device unless a PUBACK message is received.

|100|
|Message retention period|The maximum number of days that a QoS 1 message can be retained. If no PUBACK message is received before the maximum retention period ends, the message publishing request is rejected.

|7|
|MQTT message size|The maximum size of a message that can be sent by using the MQTT protocol. Messages that exceed this limit are discarded.|256 KB|
|CoAP message size|The maximum size of a message that can be sent by using the CoAP protocol. Messages that exceed this limit are discarded.|1 KB|
|MQTT keep-alive mechanism|The heartbeat timeout of an MQTT connection. If the heartbeat timeout exceeds the range, IoT Platform rejects the connection request. We recommend that you set a value that is greater than 300 seconds.

A timer starts when IoT Platform sends a CONNACK message as a respond to a CONNECT message. When IoT Platform receives a PUBLISH, SUBSCRIBE, PING, or PUBACK message, the timer is reset. If no message is received within 1.5 times the specified heartbeat interval, the server terminates the connection.

|30 to 1,200 seconds|
|RRPC timeout period|The timeout period for devices to respond to RRPC requests.|8 seconds|

## Topics

|Item|Description|Limit|
|----|:----------|:----|
|Custom topic categories|The maximum topic categories that can be defined for a product.|50|
|Permissions|A device can publish messages and subscribe only to its own topics.|N/A|
|Topic length|The maximum length of a topic that is encoded in UTF-8.|128 bytes|
|Topic levels|The maximum number of category levels that a topic can include. The number of category levels is indicated by the number of slashes \(/\) in the topic.|7|
|Subscriptions|The maximum number of topics that can be included in a subscription request.|8|
|Time to take effect|The period that a subscribe or unsubscribe operation requires to take effect. A subscription remains effective until you unsubscribe from the topic. We recommend that you subscribe to topics in advance to avoid missing information. For example, a device sends a subscription request to Topic A. After 10 seconds, the subscription takes effect and the device starts to receive messages from Topic A in real time. The device keeps receiving messages from Topic A unless you unsubscribe from the topic.

|10 seconds|
|Topic broadcasting|The maximum size of a message to be broadcasted. To generate a message body, you must convert a raw message into binary data and encode the data by using Base64.

|64 KB|
|The number of messages that can be broadcasted per minute by using the server SDK.|1|

## Device shadows

|Item|Description|Limit|
|----|:----------|:----|
|JSON levels|The maximum number of levels that can be specified in a device shadow JSON file.|5|
|File size|The maximum size of a device shadow JSON file.|16 KB|
|Properties|The maximum number of properties that can be specified in a device shadow JSON file.|128|
|Requests per second|The maximum number of requests that a device can send per second.|20|

## Data forwarding

|Item|Description|Limit|
|----|:----------|:----|
|Rules|The maximum number of rules that an Alibaba Cloud account can create.|1,000|
|Data forwarding destinations|The maximum number of data forwarding operations in a rule.|10|
|Messages processed by the rules engine|The maximum number of data forwarding queries that can be processed per second for an Alibaba Cloud account. RAM users share the quota of the Alibaba Cloud account. After a message is processed, it can be written to multiple Alibaba Cloud services. For more information, see the next row: messages written to Alibaba Cloud services.

If a message is blocked due to throttling, the system attempts to rewrite the message later. If multiple retry attempts fail, the message is discarded.

|1,000|
|Messages written to Alibaba Cloud services|The maximum number of data forwarding queries that can be processed per second for an Alibaba Cloud account. The maximum number can be reached only if the instance of an Alibaba Cloud service provides a high level of performance. RAM users share the quota of the Alibaba Cloud account.

If the limit is exceeded or if the number of concurrent write requests to an Alibaba Cloud service exceeds 40, data forwarding fails due to throttling.

If data forwarding fails due to exceptions of an Alibaba Cloud service, IoT Platform retries data forwarding for three times at an interval of 1, 3, and 10 seconds. If all retry attempts fail, the data is discarded and an error message is sent to the destination Alibaba Cloud service.

|2,000|
|Requirements of data forwarding destinations|Make sure that the destination service instance runs as expected. Data forwarding fails in multiple scenarios. These scenarios include instance failure, overdue payments, improper configurations, and invalid parameter settings, such as invalid values and lack of permissions.|N/A|
|Message deduplication|Data forwarding does not guarantee that a message is received only once. In a distributed environment, rebalance may be temporarily inconsistent and a message may be sent multiple times. If multiple messages have the same ID, deduplication is required when an application receives the messages.|N/A|

## Service subscription

The following table describes the limits on AMQP service subscription.

For more information about the limits of MNS service subscription, see the limits of MNS queues in the [MNS limits]() topic.

## Cloud API operations

The maximum number of queries per second \(QPS\) for the IoT Platform API. For more information, see [API reference](/intl.en-US/Developer Guide (Cloud)/API reference/Overview.md).

If you receive a throttling error when you call an API operation, retry the call. For more information about throttling errors, see [Common errors](https://error-center.alibabacloud.com/status/product/Public). Errors 29 to 31 are throttling errors.

