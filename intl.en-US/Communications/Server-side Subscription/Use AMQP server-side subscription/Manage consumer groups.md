# Manage consumer groups

This topic describes how to manage consumer groups. Consumer groups function as the IDs of message consumers. IoT Platform can forward messages to a specified consumer group. A message consumer can access IoT Platform by joining the consumer group to receive messages.

-   Applications of consumer groups:

    -   You can use the AMQP server-side subscription to subscribe to a specified type of messages from all devices under a product and forward the messages to a specified consumer group. Your applications receive messages by listening to the consumer group.

        For more information about configuring AMQP server-side subscriptions, see [Configure AMQP server-side subscription in the console](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Configure AMQP server-side subscription in the console.md).

    -   You can use the data forwarding function to forward messages from a specified topic to an AMQP consumer group. Your applications receive messages by listening to the consumer group.

        For more information about how to configure data forwarding rules, see [Configure data forwarding rules](/intl.en-US/Communications/Data Forwarding/Configure data forwarding rules.md).

    For more information about the differences between server-side subscription and data forwarding, see [Compare data forwarding solutions](/intl.en-US/Communications/Data Forwarding/Compare data forwarding solutions.md).

-   Usage of consumer groups: configure the consumer group ID on the AMQP client. The AMQP client accesses IoT Platform as a consumer group and receives messages.

    Multiple AMQP clients can share a consumer group ID to form a consumer group. When a device message arrives, IoT Platform sends it to a random client in the consumer group. A consumer group can have up to 64 clients.

    For more information, see [Connect an AMQP client to IoT Platform](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Connect an AMQP client to IoT Platform.md).


## Create a consumer group

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  On the Server-side Subscription page, choose **Consumer Groups** \> **Create Consumer Group**.

3.  In the Create Consumer Group dialog box that appears, enter a group name and click **OK**.

    The consumer group name can contain letters, digits, and underscores \(\_\). It can be 4 to 30 characters in length.

    After the consumer group is created, the page automatically jumps to the Consumer group details page. On this page, you can create subscriptions or view the status of consumer groups.


## View and monitor consumer groups

You can view the message consumption and accumulation status in a consumer group. You can also set CloudMonitor alert rules to monitor the message accumulation and consumption rate.

1.  On the Server-side Subscription page, select the **Consumer Groups** tab.

2.  On the Consumer Groups tab, find the consumer group you want to view, and click **View**.

3.  On the Consumer Group Details page, select the **Consumer Group Status** tab.

4.  On the Consumer Group Status tab, you can view the message consumption rate, number of accumulated messages, time of the last consumption, and online client list.

    If the number of accumulated messages is greater than 0, a **Clear** button is displayed to the right of **Accumulated Messages**.

5.  Configure a CloudMonitor alert rule to monitor the accumulation of message queues in consumer groups and the consumption rate of message consumption in consumer groups. You can also receive alert messages.

    1.  On the Consumer Group Status tab, click **Alert Settings**.

    2.  On the Create Alert Rule page, you can set an alert rule based on a specific threshold.

        |Parameter|Description|
        |:--------|:----------|
        |Product|Select **IoT Platform**.|
        |Resource Range|Only the **Instance** option is supported.|
        |Region|This parameter is available only when you set Resource Range to **Instance**. This is the region of IoT Platform instance to which the consumer group to be monitored belongs.|
        |Instance|Select the IoT Platform instance and consumer group to be monitored. You can select multiple consumer groups. An alert notification is sent only when the number of accumulated messages or the consumption rate of a consumer group triggers the alert rule. |
        |Alert Rule|Set the name of the alert rule.|
        |Rule Description|Describes the condition to trigger the alert rule. You must configure the following items:         -   Select a metric for the rule.
        -   Select a scanning period for the rule. For example, if the scan period is set to 60 minutes, scans are performed every 60 minutes.
        -   Set the triggering condition. For example, an alert is triggered only if the number of devices exceeds 5,000 for three consecutive scan periods. |
        |Silent Duration|Set the interval for the next alert if the triggering condition remains after the alert is triggered.|
        |Effective Period|Set the period for which the alert rule is effective. The alert notifications are only sent within the effective period.|
        |Notification Method|Set the notification parameters, such as the notification contacts and notification methods. For more information about notification contacts, see [Configure alert contacts](https://www.alibabacloud.com/help/doc-detail/131982.htm). |


## Delete a consumer group

The consumer group that you have created can be deleted, but the default consumer group cannot be deleted. You can delete a consumer group so that all consumers in the group stop receiving messages.

1.  On the Server-side Subscription page, select the **Consumer Groups** tab.

2.  Cancel the subscription. If the consumer group is associated with a subscription, you must cancel the subscription before you can delete the consumer group. If the consumer group has no subscriptions, you can skip this step.

    1.  Find the corresponding consumer group and click **View**.

    2.  On the Subscribed Products tab of the Consumer Group Details page, find the product name, and click **Unsubscribe**. Read the dialog box that appears and make sure that you understand the consequences. Then click **Confirm**.

        **Note:** If the server-side subscription has only one consumer group, you cannot unsubscribe on the Consumer Group Details page. You must go back to the Server-side Subscription page, edit the subscription, change the consumer group or delete the subscription.

3.  On the Consumer Groups tab of the Server-side Subscription page, find the target consumer group and click **Delete**. Read the dialog box that appears and make sure that you understand the consequences. Then click **Confirm**.


## Related topics

For more information about how to configure the consumer group ID on the AMQP client to receive messages, see:

[Connect an AMQP client to IoT Platform](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Connect an AMQP client to IoT Platform.md)

[Java SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Java SDK access example.md)

[Node.js SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Node.js SDK access example.md)

[.NET SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/.NET SDK access example.md)

[Python 2.7 SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Python 2.7 SDK access example.md)

