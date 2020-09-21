# Manage consumer groups

This topic describes how to manage consumer groups. Consumer groups function as the IDs of message consumers. IoT Platform can forward messages to a specified consumer group. A message consumer can access IoT Platform by joining the consumer group to receive messages.

-   Applications of consumer groups:

    -   You can use the AMQP server-side subscription to subscribe to a specified type of messages from all devices under a product. Then, you can forward the messages to a specified consumer group. Your applications receive messages by listening to the consumer group.

        For information about how to AMQP server-side subscriptions, see [Configure AMQP server-side subscription in the console](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Configure AMQP server-side subscription in the console.md).

    -   You can use the data forwarding function to forward messages from a specified topic to an AMQP consumer group. Your applications receive messages by listening to the consumer group.

        For information about how to configure data forwarding rules, see [Configure data forwarding rules](/intl.en-US/Communications/Data Forwarding/Configure data forwarding rules.md).

    For information about the differences between server-side subscription and data forwarding, see [Compare data forwarding solutions](/intl.en-US/Communications/Data Forwarding/Compare data forwarding solutions.md).

-   Usage of consumer groups: Configure the consumer group ID on the AMQP client. The AMQP client accesses IoT Platform as a consumer group and receives messages.

    Multiple AMQP clients can share a consumer group ID to form a consumer group. When a device message arrives, IoT Platform sends it to a random client in the consumer group. Each consumer group can have up to 64 clients.

    For more information, see [Connect an AMQP client to IoT Platform](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Connect an AMQP client to IoT Platform.md).


## Create a consumer group

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  Select **Rules** \> **Server-side Subscription** \> **Consumer Groups**.

3.  Click **Create Consumer Group**.

4.  In the Create Consumer Group dialog box that appears, enter a group name and click **OK**.

    The consumer group name can contain Chinese, English letters, Japanese, digits, and underscores \(\_\). It must be 4 to 30 characters in length. Each Chinese or Japanese occupies 2 characters.


## View and monitor consumer groups

You can view the message consumption rate and accumulation amount in a consumer group. You can also set Cloud Monitor alert rules to monitor the consumer group.

1.  On the Server-side Subscription page, click the **Consumer Groups** tab.

2.  On the Consumer Groups tab, find the consumer group you want to view, and click **View**.

3.  On the Consumer Group Details page, click the **Consumer Group Status** tab.

4.  On the Consumer Group Status tab, you can view the message consumption rate, number of accumulated messages, time of the last consumption, and online client list.

    If the number of accumulated messages is greater than 0, a **Clear** button is displayed to the right of **Accumulated Messages**.

5.  Configure a CloudMonitor alert rule to monitor the accumulation of message queues in consumer groups and the consumption rate of message consumption in consumer groups. You can also receive alert messages.

    1.  On the Consumer Group Status tab, click **Alert Settings**.

    2.  On the Create Alert Rule page, you can set an alert rule based on a specific threshold, and click **Confirm**.

        |Section|Parameter|Description|
        |-------|:--------|:----------|
        |Related Resource|Product|Select **IoT Platform**.|
        |Resource Range|Valid values:        -   **All resource**: includes all consumer groups under all instances.
        -   **Instance** |
        |Region|This parameter is available only when you set Resource Range to **Instance**. This is the region of the IoT Platform instance to which the consumer group to be monitored belongs.|
        |Instance|Select the IoT Platform instance and consumer group to be monitored. You can select multiple consumer groups. An alert notification is sent only when the number of accumulated messages or the consumption rate of a consumer group triggers the alert rule. |
        |Set Alert Rules|Alert Rule|Set the name of the alert rule.|
        |Rule Description|Describes the condition to trigger the alert rule. You must configure the following items:         -   Select a metric for the rule.
        -   Select a scanning period for the rule. For example, if the scan period is set to 60 minutes, scans are performed every 60 minutes.
        -   Set the triggering condition. For example, an alert is triggered only if the number of devices exceeds 5,000 for three consecutive scan periods. |
        |Mute for|Set the interval for the next alert if the triggering condition remains after the alert is triggered.|
        |Effective Period|Set the period for which the alert rule is effective. Alert notifications are sent only within the effective period.|
        |Notification Method|Notification Contact|Set the contact group that receives alerts. For more information, see [Configure alert contacts](/intl.en-US/Best Practices/Maintenance and Monitoring/Use CloudMonitor to monitor IoT resources/Configure alert contacts.md).|
        |Notification Methods|Valid value:

        -   Info: Email + DingTalk robot notification. |
        |Auto Scaling|If you select this option, the corresponding scaling rule is triggered when the alert occurs.|
        |Log Service|If you select this option, the alert information is written to Log Service.|
        |Email Subject|This parameter is available only when you set Resource Range to **Instance**. Enter the subject of the email that is sent to the alert contact if the alert is triggered. The default subject is Product Name + Metric Name + Instance ID.|
        |Email Remark|Enter the remarks of the email that is sent to the alert contact if the alert is triggered.|
        |HTTP CallBack|Enter a URL that can be accessed from the Internet. The Cloud Monitor pushes alert information to this address by using the POST request.|


## Delete a consumer group

The consumer group that you have created can be deleted, but the default consumer group cannot be deleted. You can delete a consumer group so that all consumers in the group stop receiving messages.

1.  On the Server-side Subscription page, select the **Consumer Groups** tab.

2.  Cancel the subscription. If the consumer group is associated with a subscription, you must cancel the subscription before you can delete the consumer group. If the consumer group has no subscriptions, you can skip this step.

    1.  Find the corresponding consumer group and click **View**.

    2.  On the Subscribed Products tab of the Consumer Group Details page, find the product name, and click **Unsubscribe**. Read the dialog box that appears and make sure that you understand the consequences. Then click **Confirm**.

        **Note:** If the server-side subscription has only one consumer group, you cannot unsubscribe on the Consumer Group Details page. You must go back to the Server-side Subscription page, edit the subscription, change the consumer group or delete the subscription.

3.  On the Consumer Groups tab of the Server-side Subscription page, find the target consumer group and click **Delete**. Read the dialog box that appears and make sure that you understand the consequences. Then click **Confirm**.


## Reference

Configure the consumer group ID to the AMQP client to receive messages. For more information, see:

-   [Connect an AMQP client to IoT Platform](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Connect an AMQP client to IoT Platform.md)
-   [Java SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Java SDK access example.md)
-   [Node.js SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Node.js SDK access example.md)
-   [.NET SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/.NET SDK access example.md)
-   [Python 2.7 SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Python 2.7 SDK access example.md)
-   [t1916992.md\#]()
-   [t1912557.md\#]()
-   [Go SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Go SDK access example.md)

