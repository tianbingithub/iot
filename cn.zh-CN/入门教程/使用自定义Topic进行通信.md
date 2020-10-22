---
keyword: [物联网, IoT, 物联网平台, 自定义 Topic 通信, Pub, 发布消息, 服务器]
---

# 使用自定义Topic进行通信

您可以在物联网平台上自定义Topic类，设备将消息发送到自定义Topic中，服务端通过AMQP SDK获取设备上报消息；服务端通过调用云端API Pub向设备发布指令。自定义Topic通信不使用物模型，消息的数据结构由您自定义。

本示例中，电子温度计定期与服务器进行数据的交互，传递温度和指令等信息。温度计向服务器上行发送当前的温度；服务器向温度计下行发送精度设置指令。

![自定义Topic通信](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7447749951/p48433.png)

## 准备开发环境

本示例中，设备端和云端均使用Java语言的SDK，需先准备Java开发环境。可从[Java 官方网站](http://developers.sun.com/downloads/)下载、安装Java开发环境。

新建项目，添加以下Maven依赖，导入阿里云设备端SDK和云端SDK。

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

## 创建产品和设备

1.  登录[物联网平台控制台](https://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在左侧导航栏，单击**设备管理** \> **产品**。

4.  单击**创建产品**，创建温度计产品。

    详细操作指导，请参见[创建产品](/cn.zh-CN/设备接入/创建产品.md)。

5.  产品创建成功后，单击该产品对应的**查看**。

6.  在产品详情页的Topic类列表页签下，增加自定义Topic类。

    详细操作指导，请参见[自定义Topic](/cn.zh-CN/设备接入/消息通信Topic/自定义Topic.md)。

    本示例中，定义了以下两个Topic类：

    -   设备发布消息Topic：/$\{productKey\}/$\{deviceName\}/user/devmsg，权限为发布。
    -   设备订阅消息Topic：/$\{productKey\}/$\{deviceName\}/user/cloudmsg，权限为订阅。
7.  在服务端订阅页签下，设置AMQP服务端订阅，订阅**设备上报消息。**

    详细操作指导，请参见[配置AMQP服务端订阅](/cn.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/配置AMQP服务端订阅.md)。

8.  在左侧导航栏，单击**设备**，然后在刚创建的温度计产品下，添加设备。

    详细操作指导，请参见[单个创建设备](/cn.zh-CN/设备接入/创建设备/单个创建设备.md)。


## 设备发送消息给服务器

流程图：

![自定义Topic通信](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7447749951/p48448.png)

在整个流程中：

-   服务器通过AMQP客户端接收消息，需配置AMQP客户端接入物联网平台，监听设备消息。

    AMQP客户端配置说明，请参见[AMQP客户端接入说明](/cn.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/AMQP客户端接入说明.md)。

    以下是使用[Qpid JMS 0.47.0](https://qpid.apache.org/releases/qpid-jms-0.47.0/index.html)作为AMQP客户端接入物联网平台的示例。

    -   添加Maven依赖。

        ```
        <!-- amqp 1.0 qpid client -->
         <dependency>
           <groupId>org.apache.qpid</groupId>
           <artifactId>qpid-jms-client</artifactId>
           <version>0.47.0</version>
         </dependency>
         <!-- util for base64-->
         <dependency>
           <groupId>commons-codec</groupId>
          <artifactId>commons-codec</artifactId>
          <version>1.10</version>
        </dependency>
        ```

    -   接入物联网平台，监听消息。示例代码如下：

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
        
            //业务处理异步线程池，线程池参数可以根据您的业务特点调整，或者您也可以用其他异步方式处理接收到的消息。
            private final static ExecutorService executorService = new ThreadPoolExecutor(
                Runtime.getRuntime().availableProcessors(),
                Runtime.getRuntime().availableProcessors() * 2, 60, TimeUnit.SECONDS,
                new LinkedBlockingQueue<>(50000));
        
            public static void main(String[] args) throws Exception {
                //参数说明，请参见AMQP客户端接入说明文档。
                String accessKey = "${YourAccessKey}";
                String accessSecret = "${YourAccessSecret}";
                String consumerGroupId = "${YourConsumerGroupId}";
                //iotInstanceId：购买的实例请填写实例ID，公共实例请填空字符串""。
                String iotInstanceId = "${YourIotInstanceId}"; 
                long timeStamp = System.currentTimeMillis();
                //签名方法：支持hmacmd5、hmacsha1和hmacsha256。
                String signMethod = "hmacsha1";
                //控制台服务端订阅中消费组状态页客户端ID一栏将显示clientId参数。
                //建议使用机器UUID、MAC地址、IP等唯一标识等作为clientId。便于您区分识别不同的客户端。
                String clientId = "${YourClientId}";
        
                //userName组装方法，请参见AMQP客户端接入说明文档。
                String userName = clientId + "|authMode=aksign"
                    + ",signMethod=" + signMethod
                    + ",timestamp=" + timeStamp
                    + ",authId=" + accessKey
                    + ",iotInstanceId=" + iotInstanceId
                    + ",consumerGroupId=" + consumerGroupId
                    + "|";
                //计算签名，password组装方法，请参见AMQP客户端接入说明文档。
                String signContent = "authId=" + accessKey + "&timestamp=" + timeStamp;
                String password = doSign(signContent,accessSecret, signMethod);
                //接入域名，请参见AMQP客户端接入说明文档。
                String connectionUrl = "failover:(amqps://${YourHost}:5671?amqp.idleTimeout=80000)"
                    + "?failover.reconnectDelay=30";
        
                Hashtable<String, String> hashtable = new Hashtable<>();
                hashtable.put("connectionfactory.SBCF",connectionUrl);
                hashtable.put("queue.QUEUE", "default");
                hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
                Context context = new InitialContext(hashtable);
                ConnectionFactory cf = (ConnectionFactory)context.lookup("SBCF");
                Destination queue = (Destination)context.lookup("QUEUE");
                // 创建连接。
                Connection connection = cf.createConnection(userName, password);
                ((JmsConnection) connection).addConnectionListener(myJmsConnectionListener);
                // 创建会话。
                // Session.CLIENT_ACKNOWLEDGE: 收到消息后，需要手动调用message.acknowledge()。
                // Session.AUTO_ACKNOWLEDGE: SDK自动ACK（推荐）。
                Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
                connection.start();
                // 创建Receiver连接。
                MessageConsumer consumer = session.createConsumer(queue);
                consumer.setMessageListener(messageListener);
            }
        
            private static MessageListener messageListener = new MessageListener() {
                @Override
                public void onMessage(Message message) {
                    try {
                        //1.收到消息之后一定要ACK。
                        // 推荐做法：创建Session选择Session.AUTO_ACKNOWLEDGE，这里会自动ACK。
                        // 其他做法：创建Session选择Session.CLIENT_ACKNOWLEDGE，这里一定要调message.acknowledge()来ACK。
                        // message.acknowledge();
                        //2.建议异步处理收到的消息，确保onMessage函数里没有耗时逻辑。
                        // 如果业务处理耗时过程过长阻塞住线程，可能会影响SDK收到消息后的正常回调。
                        executorService.submit(() -> processMessage(message));
                    } catch (Exception e) {
                        logger.error("submit task occurs exception ", e);
                    }
                }
            };
        
            /**
             * 在这里处理您收到消息后的具体业务逻辑。
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
                 * 连接成功建立。
                 */
                @Override
                public void onConnectionEstablished(URI remoteURI) {
                    logger.info("onConnectionEstablished, remoteUri:{}", remoteURI);
                }
        
                /**
                 * 尝试过最大重试次数之后，最终连接失败。
                 */
                @Override
                public void onConnectionFailure(Throwable error) {
                    logger.error("onConnectionFailure, {}", error.getMessage());
                }
        
                /**
                 * 连接中断。
                 */
                @Override
                public void onConnectionInterrupted(URI remoteURI) {
                    logger.info("onConnectionInterrupted, remoteUri:{}", remoteURI);
                }
        
                /**
                 * 连接中断后又自动重连上。
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
             * 计算签名，password组装方法，请参见AMQP客户端接入说明文档。
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

-   配置设备端SDK接入物联网平台，并发送消息。
    -   配置设备认证信息。

        ```
        final String productKey = "XXXXXX";
        final String deviceName = "XXXXXX";
        final String deviceSecret = "XXXXXXXXX";
        final String region = "XXXXXX";
        ```

    -   设置初始化连接参数，包括MQTT连接配置、设备信息和初始物模型属性。

        ```
        LinkKitInitParams params = new LinkKitInitParams();
        //LinkKit底层是MQTT协议，设置MQTT的配置。
        IoTMqttClientConfig config = new IoTMqttClientConfig();
        config.productKey = productKey;
        config.deviceName = deviceName;
        config.deviceSecret = deviceSecret;
        config.channelHost = productKey + ".iot-as-mqtt." + region + ".aliyuncs.com:1883";
        //设备的信息。
        DeviceInfo deviceInfo = new DeviceInfo();
        deviceInfo.productKey = productKey;
        deviceInfo.deviceName = deviceName;
        deviceInfo.deviceSecret = deviceSecret;
        //报备的设备初始状态。
        Map<String, ValueWrapper> propertyValues = new HashMap<String, ValueWrapper>();
        
        params.mqttClientConfig = config;
        params.deviceInfo = deviceInfo;
        params.propertyValues = propertyValues;
        ```

    -   初始化连接。

        ```
        //连接并设置连接成功以后的回调函数。
        LinkKit.getInstance().init(params, new ILinkKitConnectListener() {
             @Override
             public void onError(AError aError) {
                 System.out.println("Init error:" + aError);
             }
        
             //初始化成功以后的回调。
             @Override
             public void onInitDone(InitResult initResult) {
                 System.out.println("Init done:" + initResult);
             }
         });
        ```

    -   设备发送消息。

        设备端连接物联网平台后，向以上定义的Topic发送消息。需将onInitDone函数内容替换为以下内容：

        ```
        @Override
         public void onInitDone(InitResult initResult) {
             //设置pub消息的topic和内容。
             MqttPublishRequest request = new MqttPublishRequest();
             request.topic = "/" + productKey + "/" + deviceName + "/user/devmsg";
             request.qos = 0;
             request.payloadObj = "{\"temperature\":35.0, \"time\":\"sometime\"}";
             //发送消息并设置成功以后的回调。
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

        服务器收到消息如下：

        ```
        Message
        {payload={"temperature":35.0, "time":"sometime"},
        topic='/a1uzcH0****/device1/user/devmsg',
        messageId='1131755639450642944',
        qos=0,
        generateTime=1558666546105}
        ```


## 服务器发送消息给设备

流程图：

![自定义Topic通信](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7447749951/p48453.png)

-   配置设备端SDK订阅Topic。

    配置设备认证信息、设置初始化连接参数、初始化连接，请参见[设备发送消息给服务器](#section_bfb_ykj_zva)中的相应示例代码。

    设备要接收服务器发送的消息，还需订阅消息Topic。

    配置设备端订阅Topic示例如下：

    ```
    //初始化成功以后的回调。
    @Override
    public void onInitDone(InitResult initResult) {
        //设置订阅的topic。
        MqttSubscribeRequest request = new MqttSubscribeRequest();
        request.topic = "/" + productKey + "/" + deviceName + "/user/cloudmsg";
        request.isSubscribe = true;
        //发出订阅请求并设置订阅成功或者失败的回调函数。
        LinkKit.getInstance().subscribe(request, new IConnectSubscribeListener() {
            @Override
            public void onSuccess() {
                System.out.println("");
            }
    
            @Override
            public void onFailure(AError aError) {
    
            }
        });
    
        //设置订阅的下行消息到来时的回调函数。
        IConnectNotifyListener notifyListener = new IConnectNotifyListener() {
            //此处定义收到下行消息以后的回调函数。
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

-   配置云端SDK调用云端API [Pub](/cn.zh-CN/云端开发指南/云端API参考/消息通信/Pub.md)发布消息。
    -   设置身份认证信息。

        ```
         String regionId = "XXXXXX";
         String accessKey = "XXXXXX";
         String accessSecret = "XXXXXXXXX";
         final String productKey = "XXXXXX";
        ```

    -   设置连接参数。

        ```
        //设置client的参数。
        DefaultProfile profile = DefaultProfile.getProfile(regionId, accessKey, accessSecret);
        IAcsClient client = new DefaultAcsClient(profile);
        ```

    -   设置消息发布参数。

        ```
        PubRequest request = new PubRequest();
        request.setQos(0);
        //设置发布消息的topic。
        request.setTopicFullName("/" + productKey + "/" + deviceName + "/user/cloudmsg");
        request.setProductKey(productKey);
        //设置消息的内容，一定要用base64编码，否则乱码。
        request.setMessageContent(Base64.encode("{\"accuracy\":0.001,\"time\":now}"));
        ```

    -   发送消息。

        ```
        try {
             PubResponse response = client.getAcsResponse(request);
             System.out.println("pub success?:" + response.getSuccess());
         } catch (Exception e) {
             System.out.println(e);
         }
        ```

        设备端接收到的消息如下：

        ```
        msg = [{"accuracy":0.001,"time":now}]
        ```


## 附录：Demo

[下载Pub/Sub demo](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/44229/intl_en/1577169169729/PubSubDemo.zip)，包含本示例的云端SDK和设备端SDK配置代码Demo。

AMQP客户端接入物联网平台示例，请参见：

-   [Java SDK接入示例](/cn.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/Java SDK接入示例.md)
-   [.NET SDK接入示例](/cn.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/.NET SDK接入示例.md)
-   [Node.js SDK接入示例](/cn.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/Node.js SDK接入示例.md)
-   [Python 2.7 SDK接入示例](/cn.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/Python 2.7 SDK接入示例.md)
-   [PHP SDK接入示例]()
-   [Go SDK接入示例](/cn.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/Go SDK接入示例.md)

