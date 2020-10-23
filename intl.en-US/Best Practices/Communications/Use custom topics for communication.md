---
keyword: [Internet of Things, IoT, IoT Platform, communication based on custom topics, Pub, publish messages, server]
---

# Use custom topics for communication

You can create custom topic categories in the IoT Platform console. Then, a device can send messages to a custom topic that belongs to a topic category. Your server can receive the messages by using an AMQP SDK. Your server can also call the Pub API operation to send commands to the device. Communication based on custom topics does not use the TSL model. In this case, you can define the data structure of the message.

In this example, an electronic thermometer exchanges data with a server at a regular interval. The thermometer sends the real-time temperature data to the server, and the server sends the precision setting command to the thermometer.

![Communication based on custom topics](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6447749951/p48433.png)

## Prepare the development environment

In this example, both devices and IoT Platform use SDKs for Java. You need to prepare the Java development environment first. You can visit the [Java official website](http://developers.sun.com/downloads/) to download and install the Java IDE.

Add the following Maven dependencies to import the device SDK \(Link SDK for Java\) and IoT Platform SDK:

```
<dependencies>
  <dependency>
     <groupId>com.aliyun.alink.linksdk</groupId>
     <artifactId>iot-linkkit-java</artifactId>
     <version>1.2.0.1</version>
     <scope>compile</scope>
  </dependency>
  <dependency>
      <groupId>com.aliyun</groupId>
      <artifactId>aliyun-java-sdk-core</artifactId>
      <version>3.7.1</version>
  </dependency>
  <dependency>
      <groupId>com.aliyun</groupId>
      <artifactId>aliyun-java-sdk-iot</artifactId>
      <version>7.6.0</version>
  </dependency>
  <dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>iot-client-message</artifactId>
    <version>1.1.2</version>
  </dependency>
</dependencies>
```

## Create a product and a device

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Devices** \> **Products**.

3.  Click **Create Product** to create a thermometer product.

    For more information, see [Create a product](/intl.en-US/Device Access/Create a product.md).

4.  After the thermometer product is created, find it on the Products page and click **View** in the Actions column.

5.  On the Products Details page, click the Topic Categories tab. Then, you can create topic categories for the product.

    For more information, see [Customize a topic category](/intl.en-US/Device Access/Topics/Customize a topic category.md).

    In this example, you must define the following two topic categories:

    -   /$\{productKey\}/$\{deviceName\}/user/devmsg: Devices send messages to this topic. Set Device Operation Authorizations to Publish for this topic category.
    -   /$\{productKey\}/$\{deviceName\}/user/cloudmsg: Devices subscribe to this topic. Set Device Operation Authorizations to Subscribe for this topic category.
6.  Click the Server-side Subscription tab. Then, you can create an AMQP server-side subscription to subscribe to **Device Upstream Notification**.

    For more information, see [Configure AMQP server-side subscriptions](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Configure AMQP server-side subscriptions.md).

7.  In the left-side navigation pane, choose **Devices \> Devices**. Then, you can add a thermometer device to the thermometer product.

    For more information, see [Create a device](/intl.en-US/Device Access/Create devices/Create a device.md).


## The server receives messages from the device

The following figure shows how the server receives messages from the device.

![Communication based on custom topics](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6447749951/p48448.png)

This section describes how to configure the server and the device to implement the process.

-   The server receives messages from an AMQP client. Therefore, you must configure the AMQP client to connect the client with IoT Platform and enable the client to send messages.

    For more information, see [Connect an AMQP client to IoT Platform](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Connect an AMQP client to IoT Platform.md).

    The following example shows how to connect a [Qpid JMS 0.47.0](https://qpid.apache.org/releases/qpid-jms-0.47.0/index.html) client as an AMQP client to IoT Platform.

    -   Add the following Maven dependency:

        ```
        <! -- amqp 1.0 qpid client -->
         <dependency>
           <groupId>org.apache.qpid</groupId>
           <artifactId>qpid-jms-client</artifactId>
           <version>0.47.0</version>
         </dependency>
         <! -- util for base64-->
         <dependency>
           <groupId>commons-codec</groupId>
          <artifactId>commons-codec</artifactId>
          <version>1.10</version>
        </dependency>
        ```

    -   Connect the client to IoT Platform and enable the client to listen for device messages. Sample code:

        ```
        import java.net.URI;
        import java.util.Hashtable;
        import java.util.concurrent.ExecutorService;
        import java.util.concurrent.LinkedBlockingQueue;
        import java.util.concurrent.ThreadPoolExecutor;
        import java.util.concurrent.TimeUnit;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import javax.jms.Connection;
        import javax.jms.ConnectionFactory;
        import javax.jms.Destination;
        import javax.jms.Message;
        import javax.jms.MessageConsumer;
        import javax.jms.MessageListener;
        import javax.jms.MessageProducer;
        import javax.jms.Session;
        import javax.naming.Context;
        import javax.naming.InitialContext;
        import org.apache.commons.codec.binary.Base64;
        import org.apache.qpid.jms.JmsConnection;
        import org.apache.qpid.jms.JmsConnectionListener;
        import org.apache.qpid.jms.message.JmsInboundMessageDispatch;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        
        public class AmqpJavaClientDemo {
        
            private final static Logger logger = LoggerFactory.getLogger(AmqpJavaClientDemo.class);
        
            // Asynchronous thread pool for business processing. You can modify the thread pool parameters based on your business requirements. You can also use other asynchronous methods to process the received messages.
            private final static ExecutorService executorService = new ThreadPoolExecutor(
                Runtime.getRuntime().availableProcessors(),
                Runtime.getRuntime().availableProcessors() * 2, 60, TimeUnit.SECONDS,
                new LinkedBlockingQueue<>(50000));
        
            public static void main(String[] args) throws Exception {
                // For more information about the parameters, see the "Connect an AMQP client to IoT Platform" topic.
                String accessKey = "${YourAccessKey}";
                String accessSecret = "${YourAccessSecret}";
                String consumerGroupId = "${YourConsumerGroupId}";
                // iotInstanceId: If you are using a purchased instance, you must specify the instance ID. If you are using a public instance, you can enter an empty string "".
                String iotInstanceId = "${YourIotInstanceId}"; 
                long timeStamp = System.currentTimeMillis();
                // Signature method: hmacmd5, hmacsha1, or hmacsha256.
                String signMethod = "hmacsha1";
                // The value of the clientId parameter is displayed in the Client ID column on the Consumer Group Status tab of an AMQP consumer group in the console.
                // We recommend that you set clientId to a unique identifier, such as the UUID, MAC address, or IP address. 
                String clientId = "${YourClientId}";
        
                // For more information about how to configure the userName, see the "Connect an AMQP client to IoT Platform" topic.
                String userName = clientId + "|authMode=aksign"
                    + ",signMethod=" + signMethod
                    + ",timestamp=" + timeStamp
                    + ",authId=" + accessKey
                    + ",iotInstanceId=" + iotInstanceId
                    + ",consumerGroupId=" + consumerGroupId
                    + "|";
                // For more information about how to configure the password, see the "Connect an AMQP client to IoT Platform" topic.
                String signContent = "authId=" + accessKey + "&timestamp=" + timeStamp;
                String password = doSign(signContent,accessSecret, signMethod);
                // For more information about how to configure the host(endpoint), see the "Connect an AMQP client to IoT Platform" topic.
                String connectionUrl = "failover:(amqps://${YourHost}:5671? amqp.idleTimeout=80000)"
                    + "? failover.reconnectDelay=30";
        
                Hashtable<String, String> hashtable = new Hashtable<>();
                hashtable.put("connectionfactory.SBCF",connectionUrl);
                hashtable.put("queue.QUEUE", "default");
                hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
                Context context = new InitialContext(hashtable);
                ConnectionFactory cf = (ConnectionFactory)context.lookup("SBCF");
                Destination queue = (Destination)context.lookup("QUEUE");
                // Create Connection
                Connection connection = cf.createConnection(userName, password);
                ((JmsConnection) connection).addConnectionListener(myJmsConnectionListener);
                // Create Session
                // Session.CLIENT_ACKNOWLEDGE: After a message is received, manually call the message.acknowledge() method.
                // Session.AUTO_ACKNOWLEDGE: The SDK automatically sends an ACK packet (recommended).
                Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
                connection.start();
                // Create Receiver Link
                MessageConsumer consumer = session.createConsumer(queue);
                consumer.setMessageListener(messageListener);
            }
        
            private static MessageListener messageListener = new MessageListener() {
                @Override
                public void onMessage(Message message) {
                    try {
                        //1. Ensure that an ACK packet is sent after a message is received.
                        // We recommend that you select Session.AUTO_ACKNOWLEDGE when you create a session. Then an ACK packet is automatically sent.
                        // You can also select Session.CLIENT_ACKNOWLEDGE when you create a session. Then, you must call the message.acknowledge() method to send an ACK packet.
                        // message.acknowledge();
                        //2. We recommend that you process received messages asynchronously. Do not implement a time-consuming logic in the onMessage() method.
                        // If a time-consuming logic is implemented in this method, the thread may be blocked. This may affect the callback of the SDK after a message is received.
                        executorService.submit(() -> processMessage(message));
                    } catch (Exception e) {
                        logger.error("submit task occurs exception ", e);
                    }
                }
            };
        
            /**
             * Implement the business logic after messages are received.
             */
            private static void processMessage(Message message) {
                try {
                    byte[] body = message.getBody(byte[].class);
                    String content = new String(body);
                    String topic = message.getStringProperty("topic");
                    String messageId = message.getStringProperty("messageId");
                    logger.info("receive message"
                        + ", topic = " + topic
                        + ", messageId = " + messageId
                        + ", content = " + content);
                } catch (Exception e) {
                    logger.error("processMessage occurs error ", e);
                }
            }
        
            private static JmsConnectionListener myJmsConnectionListener = new JmsConnectionListener() {
                /**
                 * A connection is established.
                 */
                @Override
                public void onConnectionEstablished(URI remoteURI) {
                    logger.info("onConnectionEstablished, remoteUri:{}", remoteURI);
                }
        
                /**
                 * The connection fails after the retry attempts reach the maximum limit.
                 */
                @Override
                public void onConnectionFailure(Throwable error) {
                    logger.error("onConnectionFailure, {}", error.getMessage());
                }
        
                /**
                 * The connection is interrupted.
                 */
                @Override
                public void onConnectionInterrupted(URI remoteURI) {
                    logger.info("onConnectionInterrupted, remoteUri:{}", remoteURI);
                }
        
                /**
                 * The connection is interrupted and then automatically restored.
                 */
                @Override
                public void onConnectionRestored(URI remoteURI) {
                    logger.info("onConnectionRestored, remoteUri:{}", remoteURI);
                }
        
                @Override
                public void onInboundMessage(JmsInboundMessageDispatch envelope) {}
        
                @Override
                public void onSessionClosed(Session session, Throwable cause) {}
        
                @Override
                public void onConsumerClosed(MessageConsumer consumer, Throwable cause) {}
        
                @Override
                public void onProducerClosed(MessageProducer producer, Throwable cause) {}
            };
        
            /**
             * For more information about the signature algorithm of a password, see the "Connect an AMQP client to IoT Platform" topic.
             */
            private static String doSign(String toSignString, String secret, String signMethod) throws Exception {
                SecretKeySpec signingKey = new SecretKeySpec(secret.getBytes(), signMethod);
                Mac mac = Mac.getInstance(signMethod);
                mac.init(signingKey);
                byte[] rawHmac = mac.doFinal(toSignString.getBytes());
                return Base64.encodeBase64String(rawHmac);
            }
        }
        ```

-   Configure the device SDK to connect the device with IoT Platform and enable the device to send messages.
    -   Specify the device authentication parameters.

        ```
        final String productKey = "XXXXXX";
        final String deviceName = "XXXXXX";
        final String deviceSecret = "XXXXXXXXX";
        final String region = "XXXXXX";
        ```

    -   Set the connection initialization parameters. These include the MQTT connection parameters, device parameters, TSL model parameters.

        ```
        LinkKitInitParams params = new LinkKitInitParams();
        // Set the MQTT connection parameters. Link Kit uses MQTT as the underlying protocol.
        IoTMqttClientConfig config = new IoTMqttClientConfig();
        config.productKey = productKey;
        config.deviceName = deviceName;
        config.deviceSecret = deviceSecret;
        config.channelHost = productKey + ".iot-as-mqtt." + region + ".aliyuncs.com:1883";
        // Set the device parameters.
        DeviceInfo deviceInfo = new DeviceInfo();
        deviceInfo.productKey = productKey;
        deviceInfo.deviceName = deviceName;
        deviceInfo.deviceSecret = deviceSecret;
        // Register the initial status of the device.
        Map<String, ValueWrapper> propertyValues = new HashMap<String, ValueWrapper>();
        
        params.mqttClientConfig = config;
        params.deviceInfo = deviceInfo;
        params.propertyValues = propertyValues;
        ```

    -   Initialize the connection.

        ```
        // Initialize the connection and configure the callback function that is used after the initialization succeeds.
        LinkKit.getInstance().init(params, new ILinkKitConnectListener() {
             @Override
             public void onError(AError aError) {
                 System.out.println("Init error:" + aError);
             }
        
             // Configure the callback function that is used after the initialization succeeds.
             @Override
             public void onInitDone(InitResult initResult) {
                 System.out.println("Init done:" + initResult);
             }
         });
        ```

    -   Send a message from the device.

        After a device connects to IoT Platform, the device sends a message to the specified topic. Replace the content of the onInitDone function, as shown in the following example:

        ```
        @Override
         public void onInitDone(InitResult initResult) {
             // Set the topic to which the message is published and the message content.
             MqttPublishRequest request = new MqttPublishRequest();
             request.topic = "/" + productKey + "/" + deviceName + "/user/devmsg";
             request.qos = 0;
             request.payloadObj = "{\"temperature\":35.0, \"time\":\"sometime\"}";
             // Publish the message and configure the callback functions that are used after the message is published.
             LinkKit.getInstance().publish(request, new IConnectSendListener() {
                 @Override
                 public void onResponse(ARequest aRequest, AResponse aResponse) {
                     System.out.println("onResponse:" + aResponse.getData());
                 }
        
                 @Override
                 public void onFailure(ARequest aRequest, AError aError) {
                     System.out.println("onFailure:" + aError.getCode() + aError.getMsg());
                 }
             });
         }
        ```

        The server receives the following message:

        ```
        Message
        {payload={"temperature":35.0, "time":"sometime"},
        topic='/a1uzcH0****/device1/user/devmsg',
        messageId='1131755639450642944',
        qos=0,
        generateTime=1558666546105}
        ```


## The server sends messages to the device

The following figure shows how the server sends a message to the device.

![Communication based on custom topics](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6447749951/p48453.png)

-   Configure the device SDK to subscribe to a topic.

    For more information about how to specify device authentication parameters, set connection initialization parameters, and initialize the connection, see the sample code in the [The server receives messages from the device](#section_bfb_ykj_zva) section.

    The device must subscribe to a specific topic before the device can receive messages sent by the server.

    The following example shows how to configure the device SDK to subscribe to a topic:

    ```
    // Configure the callback function that is used after the initialization succeeds.
    @Override
    public void onInitDone(InitResult initResult) {
        // Set the topic to which the device subscribes.
        MqttSubscribeRequest request = new MqttSubscribeRequest();
        request.topic = "/" + productKey + "/" + deviceName + "/user/cloudmsg";
        request.isSubscribe = true;
        // Send a subscription request and configure the callback functions that are used after the subscription succeeds or fails.
        LinkKit.getInstance().subscribe(request, new IConnectSubscribeListener() {
            @Override
            public void onSuccess() {
                System.out.println("");
            }
    
            @Override
            public void onFailure(AError aError) {
    
            }
        });
    
        // Set the listener that listens for subscribed messages.
        IConnectNotifyListener notifyListener = new IConnectNotifyListener() {
            // Configure the callback functions that are used after a subscribed message is received.
            @Override
            public void onNotify(String connectId, String topic, AMessage aMessage) {
                System.out.println(
                    "received message from " + topic + ":" + new String((byte[])aMessage.getData()));
            }
    
            @Override
            public boolean shouldHandle(String s, String s1) {
                return false;
            }
    
            @Override
            public void onConnectStateChange(String s, ConnectState connectState) {
    
            }
        };
        LinkKit.getInstance().registerOnNotifyListener(notifyListener);
    }
    ```

-   Configure the IoT Platform SDK to enable IoT Platform to publish a message by calling the [Pub](/intl.en-US/Developer Guide (Cloud)/API reference/Communications/Pub.md) API operation.
    -   Specify identity verification information.

        ```
         String regionId = "XXXXXX";
         String accessKey = "XXXXXX";
         String accessSecret = "XXXXXXXXX";
         final String productKey = "XXXXXX";
        ```

    -   Set the connection parameters.

        ```
        // Set the parameters of the client.
        DefaultProfile profile = DefaultProfile.getProfile(regionId, accessKey, accessSecret);
        IAcsClient client = new DefaultAcsClient(profile);
        ```

    -   Set the parameters that are used to publish a message.

        ```
        PubRequest request = new PubRequest();
        request.setQos(0);
        // Set the topic to which the message is published.
        request.setTopicFullName("/" + productKey + "/" + deviceName + "/user/cloudmsg");
        request.setProductKey(productKey);
        // Set the MessageContent parameter. The message content must be encoded in Base64. Otherwise, the message content will appear as garbled characters.
        request.setMessageContent(Base64.encode("{\"accuracy\":0.001,\"time\":now}"));
        ```

    -   Publish the message.

        ```
        try {
             PubResponse response = client.getAcsResponse(request);
             System.out.println("pub success?:" + response.getSuccess());
         } catch (Exception e) {
             System.out.println(e);
         }
        ```

        The device receives the following message:

        ```
        msg = [{"accuracy":0.001,"time":now}]
        ```


## Appendix: demo

You can [Download the Pub/Sub demo](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/44229/intl_en/1577169169729/PubSubDemo.zip) to view the demo for this example.

For more information about how to connect an AMQP client to IoT Platform, see the following articles:

-   [Java SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Java SDK access example.md)
-   [.NET SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/.NET SDK access example.md)
-   [Node.js SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Node.js SDK access example.md)
-   [Python 2.7 SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Python 2.7 SDK access example.md)
-   [PHP SDK access example]()
-   [Go SDK access example](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Go SDK access example.md)

