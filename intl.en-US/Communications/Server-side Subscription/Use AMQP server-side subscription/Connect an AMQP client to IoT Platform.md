---
keyword: [IoT, IoT Platform, AMQP, server-side subscription to device messages]
---

# Connect an AMQP client to IoT Platform

This article describes how to connect an Advanced Message Queuing Protocol \(AMQP\) client to IoT Platform. You can perform this operation after you configure AMQP server-side subscription in the IoT Platform console. This allows you to receive device messages by using the AMQP client.

## Protocol version

For more information, see [AMQP](https://www.amqp.org/).

Server-side subscription of Alibaba Cloud IoT Platform complies only with AMQP 1.0.

## Procedure

1.  An AMQP client and IoT Platform establish a TCP connection through a three-way handshake. Then, a TLS handshake is performed to implement authentication.

    **Note:** To ensure security, IoT Platform requires that AMQP clients use TLS encryption. Transmission over unencrypted channels is not supported.

2.  The client requests to establish a connection.

    PLAIN SASL is used to authenticate the connection. The authentication is based on a username and password. After the username and password authentication succeeds on IoT Platform, the connection is established.

    AMQP requires that the open frame includes the idle-time-out field when a connection is established. This field indicates the heartbeat timeout period. The heartbeat timeout period ranges from 30,000 milliseconds to 300,000 milliseconds. IoT Platform ends the connection if no frame is transmitted over the connection after the heartbeat timeout period expires. The method that is used to set the idle-time-out field varies with the programming language of the SDK. For more information, see demos of SDKs for different programming languages.

3.  The client sends a request to IoT Platform to establish a receiver link. The receiver link is a one-way channel that you can use to push data from IoT Platform to the client.

    The client must establish the receiver link within 15 seconds after the AMQP connection is established. Otherwise, IoT Platform ends the AMQP connection.

    After the receiver link is established, the client is connected to IoT Platform.

    **Note:**

    -   Only one receiver link can be created for each connection. Sender links are not supported. IoT Platform can push messages to the client, but the client cannot send messages to IoT Platform.
    -   The class name of the receiver link varies based on the programming language of the SDK. For example, the receiver link class is named MessageConsumer in some SDKs.

## Configurations

This section describes the endpoint and authentication parameters that the AMQP client can use to access IoT Platform.

-   Endpoint:

    -   To view the endpoints of the instance that you purchased, perform the following steps: Log on to the IoT Platform console. In the left-side navigation pane, click **Instances**. On the page that appears, click **View** in the Actions column of the instance. On the Instance Details page, you can view the endpoints.

        **Note:** The Instance Details page shows an AMQP endpoint that start with `amqps://`. If you need to connect an AMQP client to IoT Platform, the `amqps://` prefix must be removed from the AMQP endpoint.

    -   The endpoint for public instances is in the `${uid}.iot-amqp.${regionId}.aliyuncs.com` format.
    |Variable|Description|
    |--------|-----------|
    |uid|The ID of your Alibaba Cloud account. To view the account ID, perform the following steps: Log on to the Alibaba Cloud Management Console by using your Alibaba Cloud account. In the upper-right corner of the page, click the profile picture to go to the Security Settings page. |
    |regionId|The ID of the region where IoT Platform resides. You can view the region in the upper-right corner of the IoT Platform console. For more information about region IDs, see [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm). |

-   Port number:
    -   If you use a Java, .NET, Python 2.7, Node.js, or Go client, the port number is 5671.
    -   If you use a Python3 or PHP client, the port number is 61614.
-   Client-side authentication parameters:

    ```
    userName = clientId|iotInstanceId=${iotInstanceId},authMode=aksign,signMethod=hmacsha1,consumerGroupId=${consumerGroupId},authId=${accessKey},timestamp=1573489088171|
    password = signMethod(stringToSign, accessSecret)
    ```

    |Parameter|Required|Description|
    |---------|--------|-----------|
    |clientId|Required|The client ID. You must use a unique identifier, such as the UUID, MAC address, or IP address of the client. The client ID must be 1 to 64 characters in length. This parameter will be displayed on the Consumer Group Status page of Service Subscription in the console. |
    |iotInstanceId|No|The instance ID. This parameter is required only for purchased instances.|
    |authMode|Yes|The authentication mode. Valid value: aksign.|
    |signMethod|Yes|The signature algorithm. Valid values: hmacmd5, hmacsha1, and hmacsha256.|
    |consumerGroupId|Yes|The consumer group ID. You can view the IDs of your consumer groups in the IoT Platform console.|
    |authId|Yes|The authentication information. In the aksign authentication mode, set the authId parameter to your AccessKey ID. To obtain an AccessKey pair, perform the following steps: Log on to the Alibaba Cloud Management Console, move the pointer over the profile picture, and then click **AccessKey Management**. On the AccessKey Management page, you can view the AccessKey pair.

**Note:** If you use the AccessKey ID of a RAM user, the RAM user must have the AliyunIOTFullAccess permission on IoT Platform. Otherwise, the connection fails. For more information about authorization methods, see [t7494.md\#section\_vb3\_y4m\_51r](/intl.en-US/User Accounts/RAM authorization/Resource Access Management (RAM)/Use RAM users.md). |
    |timestamp|Yes|The timestamp. The timestamp is a LONG integer in milliseconds.|
    |cleanSession|No|Indicates whether to delete accumulated offline messages. Valid values:     -   true: deletes accumulated offline messages.
    -   false: retains accumulated offline messages.
Default value: false. |

    |Parameter|Required|Description|
    |---------|--------|-----------|
    |signMethod|Yes|The signature algorithm. Use the signature algorithm that is specified in the userName parameter to calculate the signature value. Then, you can convert the value to a Base64-encoded string.|
    |stringToSign|Yes|The string to sign. Sort the parameters that must be signed in alphabetical order. Splice the key and value of each parameter by using an equal sign \(=\), and splice the parameters by using an ampersand \(&\).

The parameters that must be signed include authId and timestamp.

Set the stringToSign parameter to a value in the `authId=${accessKey}&timestamp=1573489088171` format. |
    |accessSecret|Yes|The AccessKey secret. To obtain an AccessKey pair, perform the following steps: Log on to the Alibaba Cloud Management Console, move the pointer over the profile picture, and then click **AccessKey Management**. On the AccessKey Management page, you can view the AccessKey pair. |


## Messaging

After the receiver link between the AMQP client and IoT Platform is established, IoT Platform pushes messages to the client over the link.

Each message that is pushed from IoT Platform to the client consists of the following parts:

-   Message body. It is based on the binary format and contains the payload of the message
-   Message attributes such as topic and message ID. You can obtain the attributes from the application-properties that are defined in AMQP. The attributes are in the `key:value` format.

    |Key|Description|
    |---|-----------|
    |topic|The message topic.|
    |messageId|The message ID.|
    |generateTime|The time when the message was generated.|


Message receipt:

AMQP requires that the client sends a receipt \(referred to as a settle in AMQP\) to IoT Platform. The receipt is used to notify IoT Platform that a message is received. The receipt can be manually or automatically sent. We recommend that you use the automatic mode. For more information, see the user guide of the client.

Messaging policies:

1.  Messages are pushed in real time.
2.  Messages are accumulated if the bandwidth is throttled or the previous attempt to send the messages fails.
3.  The accumulated messages are re-sent. If IoT Platform receives an ACK packet from the AMQP client, IoT Platform starts receiving the next message.
4.  IoT Platform determines whether the messages are received based on the ACK packet. If IoT Platform does not receive an ACK packet, the message is accumulated and then re-sent.

**Note:**

-   The order of messages is not preserved. You can use timestamps to preserve the order of services.
-   Messages can be repeatedly sent. To ensure that a message is received, a message may be repeatedly sent until IoT Platform receives an ACK packet from the client or the message expires. You can deduplicate messages based on message IDs.
-   To delete accumulated messages Each time the client starts a new session, you can implement a cleanSession mechanism on the client. For more information, see the demo of AMQP server-side subscription.

## References

-   [Limits](/intl.en-US/Communications/Server-side Subscription/Limits.md)
-   [Java SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Java SDK access example.md)
-   [.NET SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/.NET SDK access example.md)
-   [Node.js SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Node.js SDK access example.md)
-   [Python 2.7 SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Python 2.7 SDK access example.md)
-   [Go SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Go SDK access example.md)

