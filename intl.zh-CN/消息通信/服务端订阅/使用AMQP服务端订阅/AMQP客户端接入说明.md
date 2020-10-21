---
keyword: [IoT, 物联网平台, AMQP, 服务端订阅设备消息]
---

# AMQP客户端接入说明

控制台配置完AMQP服务端订阅后，您需要参考本文将AMQP客户端接入物联网平台。成功接入后，在您的服务端运行AMQP客户端，即可接收设备消息。

## 协议版本说明

AMQP协议标准的详细介绍，请参见[AMQP协议标准](https://www.amqp.org/)。

阿里云物联网平台服务端订阅仅支持AMQP 1.0版的协议标准。

## 连接认证过程

1.  首先，AMQP客户端与物联网平台经过三次握手建立TCP连接，然后进行TLS握手校验。

    **说明：** 为了保障安全，接收方必须使用TLS加密，不支持非加密的TCP传输。

2.  客户端请求建立Connection。

    连接认证方式为PLAIN-SASL，可以理解为用户名（userName）和密码（password）认证。云端认证userName和password通过后，建立Connection。

    此外，根据AMQP协议，客户端建连时，还需在Open帧中携带心跳时间，即AMQP协议的idle-time-out参数。心跳时间单位为毫秒，取值范围为30,000~300,000。如果超过心跳时间，Connection上没有任何帧通信，物联网平台将关闭连接。SDK不同，idle-time-out参数设置方法不同。具体设置方法，请参见各语言SDK示例文档。

3.  客户端向云端发起请求，建立Receiver Link（即云端向客户端推送数据的单向通道）。

    客户端建立Connection成功后，需在15秒内完成Receiver Link的建立，否则物联网平台会关闭连接。

    建立Receiver Link后，客户端成功接入物联网平台。

    **说明：**

    -   一个Connection上只能创建一个Receiver Link，不支持创建Sender Link，即只能由云端向客户端推送消息，客户端不能向云端发送消息。
    -   Receiver Link在有的SDK中名称不同，例如在有的SDK上称为MessageConsumer，请根据具体SDK设置。

## 连接配置说明

AMQP客户端接入物联网平台的连接地址和连接认证参数说明如下：

-   接入域名：
    -   您购买的实例的接入域名，请在物联网平台控制台**实例管理**页面，单击实例对应的**查看**，进入实例详情页查看。
    -   公共实例的接入域名：`${uid}.iot-amqp.${regionId}.aliyuncs.com`。

        |字段|说明|
        |--|--|
        |uid|您的阿里云账号ID。 可登录[物联网平台控制台](https://iot.console.aliyun.com/)，单击账号头像，跳转至安全设置页面查看。 |
        |regionId|您的物联网平台服务所在地域ID。 在物联网平台控制台左上方可查看地域。RegionId的表达方法，请参见[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)。 |

-   端口：
    -   对于Java、.NET、Python 2.7、Node.js、Go客户端：端口号为5671。
    -   对于Python3、PHP客户端：端口号为61614。
-   客户端身份认证参数：

    ```
    userName = clientId|iotInstanceId=${iotInstanceId},authMode=aksign,signMethod=hmacsha1,consumerGroupId=${consumerGroupId},authId=${accessKey},timestamp=1573489088171|
    password = signMethod(stringToSign, accessSecret)
    ```

    |参数|是否必需|说明|
    |--|----|--|
    |clientId|是|表示客户端ID，建议使用您的AMQP客户端所在服务器UUID、MAC地址、IP等唯一标识。长度不可超过64个字符。 登录物联网平台控制台，在**规则引擎** \> **服务端订阅** \> **消费组列表**，单击消费组对应的**查看**，消费组详情页将显示该参数，方便您识别区分不同的客户端。 |
    |iotInstanceId|否|实例ID。仅您购买的实例需要传入。|
    |authMode|是|鉴权模式。目前仅支持aksign模式。|
    |signMethod|是|签名算法。可选：hmacmd5、hmacsha1和hmacsha256。|
    |consumerGroupId|是|消费组ID。登录物联网平台控制台，在**规则引擎** \> **服务端订阅** \> **消费组列表**查看您的消费组ID。 |
    |authId|是|认证信息。在aksign鉴权模式下，authId取值为您的阿里云账号AccessKey ID。 登录物联网平台控制台，将鼠标移至账号头像上，然后单击**AccessKey管理**，获取AccessKey。

**说明：** 如果使用子账号AccessKey ID，您需要给该子账号授予管理物联网平台的权限（AliyunIOTFullAccess），否则将会连接失败。授权方法请参见[授权子账号访问物联网平台](/intl.zh-CN/账号与登录/账号授权/RAM授权管理/子账号访问.md)。 |
    |timestamp|是|当前时间。Long类型的毫秒值时间戳。|

    |参数|是否必需|说明|
    |--|----|--|
    |signMethod|是|签名算法。请使用userName中指定的签名算法计算签名值，并转为base64字符串。|
    |stringToSign|是|待签名的字符串。 将需要签名的参数的键值对按照首字母字典排序，并在键值间添加等号（=）；参数间添加与号（&），拼接成待签名的字符串。

需要签名的参数为：authId和timestamp。

待签名的字符串为`stringToSign = authId=${accessKey}&timestamp=1573489088171`。 |
    |accessSecret|是|您的阿里云账号的AccessKey Secret。 登录物联网平台控制台，将鼠标移至账号头像上，然后单击**AccessKey管理**，获取AccessKey。 |


## 接入示例

您可以使用以下语言的AMQP客户端接入示例。示例中的参数配置，请参见[连接配置说明](#section_3k8_hw3_oid)。

-   [Java SDK接入示例](/intl.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/Java SDK接入示例.md)
-   [.NET SDK接入示例](/intl.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/.NET SDK接入示例.md)
-   [Node.js SDK接入示例](/intl.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/Node.js SDK接入示例.md)
-   [Python 2.7 SDK接入示例](/intl.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/Python 2.7 SDK接入示例.md)
-   [Python3 SDK接入示例]()
-   [PHP SDK接入示例]()
-   [Go SDK接入示例](/intl.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/Go SDK接入示例.md)

## 接收云端推送的消息

客户端和云端之间的Receiver Link建连成功后，云端就可以在这条Link上向AMQP客户端推送消息。

云端推送的消息：

-   消息体：消息的payload为二进制格式。
-   消息的业务属性，如消息Topic和Message ID等，需要从AMQP协议的Application Properties中获取。格式为`key:value`。

    |Key|含义|
    |---|--|
    |topic|消息Topic。|
    |messageId|消息ID。|
    |generateTime|消息生成时间。|


消息回执：

按照AMQP协议的定义，客户端需要给云端回执（AMQP协议上一般称为settle），通知云端消息已经被成功接收。AMQP客户端通常会提供自动回执模式（推荐）和手动回执模式。具体请参考相应的客户端的使用说明。

消息策略：

1.  实时消息直接推送。
2.  推送限流或失败进入堆积，堆积消息会尝试重新发送。
3.  堆积消息会尝试重新发送。如果AMQP客户端回复ACK，云端马上触发下一个拉取消息窗口。
4.  云端根据客户端返回的ACK判断消息是否送达。云端未收到ACK，则消息将进入堆积，并重发。

**说明：**

-   消息不保序。您可以使用时间戳来实现业务的相对有序。
-   消息可能重复发送。为了确保消息送达，在某些情况下，同一条消息可能重复发送，直到客户端返回ACK或消息过期。您可以根据消息ID做唯一性处理。
-   同一个消费组的多个消费端之间，在某些情况下（如网络抖动、消费端重连），可能在个别消费端上存在短暂的流量不均衡，属于正常现象，云端会自动检测并执行Rebalance，一般能在10分钟内恢复均衡。如果您的消息QPS较高或者消息处理比较耗费资源，建议适当增加消费端的数目，保持一定的消费能力冗余。
-   您可以在物联网平台控制台清除堆积消息，参见[查看和监控消费组](/intl.zh-CN/消息通信/服务端订阅/使用AMQP服务端订阅/管理消费组.md)。

