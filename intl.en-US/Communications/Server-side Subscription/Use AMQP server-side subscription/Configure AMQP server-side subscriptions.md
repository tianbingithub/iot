---
keyword: [IoT, IoT Platform, AMQP, server-side subscription to device messages]
---

# Configure AMQP server-side subscriptions

This article describes how to configure and manage an AMQP server-side subscription in the IoT Platform console.

## Configure a subscription

To set the type of messages to which you want to subscribe, perform the following steps:

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Rules** \> **Server-side Subscription**.

3.  On the Server-side Subscription page, click **Create Subscription**.

4.  In the Create Subscription dialog box, set the parameters and click **OK**.

    |Parameter|Description|
    |---------|-----------|
    |Product|Select the product to which the devices belong. The messages submitted by these devices are pushed to consumers.|
    |Subscription Type|Select **AMQP**.|
    |Consumer Group|Select consumer groups. You can select one or more consumer groups as needed. IoT Platform generates a default consumer group. To create a consumer group, click Create Consumer Group in the lower-right corner of the **Select Consumer Groups** dialog box. For more information, see [Manage consumer groups](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Manage consumer groups.md). |
    |Push Message Type|Select the types of messages. You can subscribe to the following types of device messages:     -   **Device Upstream Notification**: the messages in the topics whose **Allowed Operations** parameter is set to **Publish**.

This type of messages includes custom data and Thing Specification Language \(TSL\) data that is submitted by devices. The upstream TSL data includes property data, event data, responses to property setting requests, and responses to service calling requests. The TSL data pushed to user servers is processed by IoT Platform. For information about data formats, see [Data formats](/intl.en-US/Communications/Data formats.md).

For example, the following three topics are defined for a product:

        -   `/${YourProductKey}/${YourDeviceName}/user/get`. The Allowed Operations parameter of this topic is set to Subscribe.
        -   `/${YourProductKey}/${YourDeviceName}/user/update`. The Allowed Operations parameter of this topic is set to Publish.
        -   `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post`. The Allowed Operations parameter of this topic is set to Subscribe.
IoT Platform pushes messages in the following topics: `/${YourProductKey}/${YourDeviceName}/user/update` and `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post`.

    -   **Device Status Change Notification**: the notifications that devices send when the online or offline status changes.
    -   **Gateway's sub-devices discovery report**: the sub-device data that gateways submit when they detect new sub-devices. The gateways must have the applications that can be used to detect sub-devices. This message type is specific to gateways.
    -   **Device Topological Relation Changes**: the notifications that gateways send when topological relationships between sub-devices and the gateways are created or deleted. This message type is specific to gateways.
    -   **Device Changes Throughout Lifecycle**: the notifications that devices send when the devices are created, deleted, enabled, or disabled.
    -   **TSL Historical Data Reporting**: The historical properties and events that are submitted by devices.
    -   **OTA Update Status Notification**: the notifications that devices send during firmware verification and batch update. When a device update succeeds or fails, a notification is pushed.
    -   **Device tag change**: the messages that devices send when device tags change.
    -   **Submit a module version number**: the message that devices send when OTA module versions change.
    -   **Batch status notification**: the notifications that devices send when the status of OTA update batches change. |


[Configure AMQP clients](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Connect an AMQP client to IoT Platform.md)

After the configurations are completed and submitted device data is received by an AMQP client, you can view the operations logs in the IoT Platform console. Choose **Maintenance** \> **Device Log**. On the page that appears, you can view the log entries on the Cloud run log tab. The log entries are generated when a device submits data, IoT Platform forwards the data to the AMQP client, and the AMQP client returns an ACK message.

