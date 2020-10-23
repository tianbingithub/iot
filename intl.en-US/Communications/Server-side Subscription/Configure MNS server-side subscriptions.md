---
keyword: [Internet of Things, IoT Platform, IoT, server, listening, device message, server-side subscription, submitted device data, device status change, detection of sub-devices by a gateway, lifecycle change, device topology change, MNS]
---

# Configure MNS server-side subscriptions

IoT Platform allows you to send device messages to Message Service \(MNS\). Cloud applications can obtain the device messages by listening to MNS queues. This article describes how to configure an MNS server-side subscription.

If you use a RAM user, the RAM user must have the `AliyunIOTAccessingMNSRole` permission.

1.  In the IoT Platform console, configure an MNS server-side subscription for a product. IoT Platform automatically forwards messages to MNS queues.

    1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

    2.  In the left-side navigation pane, choose **Rules** \> **Server-side Subscription**.

    3.  On the Subscriptions tab of the Server-side Subscription page, click **Create Subscription**.

    4.  In the Create Subscription dialog box, set the parameters and click **OK**.

        |Parameter|Description|
        |---------|-----------|
        |Product|Select the product to which the devices belong. The messages submitted by these devices are pushed to consumers.|
        |Subscription Type|Select **MNS**.|
        |Push Message Type|Select the types of messages. You can subscribe to the following types of device messages:         -   **Device Upstream Notification**: the messages in the topics whose **Allowed Operations** parameter is set to **Publish**.

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
        -   **OTA Update Status Notification**: the notifications that devices send during firmware verification and batch update. When a device update succeeds or fails, a notification is pushed. |

    5.  In the dialog box that appears, click **OK**.

        IoT Platform automatically creates an MNS message queue in the format of `aliyun-iot-${yourproductkey}`. When you configure a queue listener, you must specify this message queue.

        You are charged for using MNS resources. For more information about billing methods, see [MNS billing](https://www.alibabacloud.com/help/zh/doc-detail/71896.htm).

        **Note:** If you delete the MNS server-side subscription, the corresponding MNS queue is automatically deleted.

2.  Configure an MNS client and listen to the MNS queue.

    In this example, the MNS SDK for Java is used to listen to the MNS queue.

    For information about how to download the MNS SDK, see[MNS documentation](https://www.alibabacloud.com/help/zh/doc-detail/27508.htm).

    1.  To install the MNS SDK for Java, add the following dependencies to the pom.xml file:

        ```
        <dependency>
            <groupId>com.aliyun.mns</groupId>
            <artifactId>aliyun-sdk-mns</artifactId>
            <version>1.1.8</version>
            <classifier>jar-with-dependencies</classifier>
        </dependency>
        ```

    2.  Specify the following parameters when you configure the MNS SDK:

        ```
        CloudAccount account = new CloudAccount( $AccessKeyId, $AccessKeySecret, $AccountEndpoint);
        ```

        -   Replace $AccessKeyId and $AccessKeySecret with the AccessKey ID and AccessKey secret of your Alibaba Cloud account. These parameters are required when you call API operations. Log on to the Alibaba Cloud Console, move the pointer over the profile picture of your Alibaba Cloud account, and then click **AccessKey Management** to create or view AccessKey pairs.
        -   Replace $AccountEndpoint with the MNS endpoint. In the MNS console, click **Get Endpoint**.
    3.  Specify the logic to receive device messages.

        ```
        MNSClient client = account.getMNSClient(); 
        CloudQueue queue = client.getQueueRef("aliyun-iot-a1xxxxxx8o9"); //Specify the automatically created queue.
        
            while (true) { 
            //Retrieve messages. 
            Message popMsg = queue.popMessage(10);  //The timeout period of long polling requests is 10 seconds.      
            if (popMsg ! = null) { 
                System.out.println("PopMessage Body: "+ popMsg.getMessageBodyAsRawString()); //Obtain raw messages. 
                queue.deleteMessage(popMsg.getReceiptHandle()); //Delete the messages from the queue. 
            } else { 
                System.out.println("Continuing"); } }
                                    
        ```

    4.  Run the program to listen to the MNS queue.
3.  Start a device and send a message from the device to IoT Platform.

    For more information about how to develop a device-side SDK, see[Link SDK documentation](https://www.alibabacloud.com/help/doc-detail/96624.htm).

4.  Check whether your cloud application retrieves the message. The following code shows the format of a retrieved message:

    ```
    {
    "messageid":" ",
    "messagetype":"upload",
    "topic":"/al12345****/device123/user/update",
    "payload":" ", 
    "timestamp": " ",
    }
    ```

    |Parameter|Description|
    |:--------|:----------|
    |messageid|The ID of the message. The message ID is generated by IoT Platform.|
    |messagetype|The type of the message. Valid values:    -   upload: submitted device data
    -   status: device status changes
    -   topo\_listfound: the detection of sub-devices by a gateway
    -   topo\_lifecycle: device topology changes
    -   device\_lifecycle: device lifecycle changes
    -   thing\_history: historical TSLdata
    -   ota\_event: firmware update status |
    |topic|The IoT Platform topic from which the message is forwarded.|
    |payload|The base64-encoded message payload. For more information about data formats, see [Data formats](/intl.en-US/Communications/Data formats.md). |
    |timestamp|The timestamp. It is the number of seconds that have elapsed since the epoch time January 1, 1970, 00:00:00 UTC.|


**Related topics**  


[Data formats](/intl.en-US/Communications/Data formats.md)

