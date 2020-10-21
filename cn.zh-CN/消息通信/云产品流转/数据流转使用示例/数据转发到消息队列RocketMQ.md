---
keyword: [物联网, 物联网平台, IoT, 规则引擎, 流转数据, 发送数据, 消息队列（RocketMQ）]
---

# 数据转发到消息队列RocketMQ

您可以使用规则引擎，将物联网平台数据转发到消息队列（RocketMQ）中存储。从而实现消息从设备、物联网平台、RocketMQ到应用服务器之间的全链路高可靠传输能力。

-   已创建消息队列（RocketMQ）实例和用于接收数据的Topic。RocketMQ使用方法，请参见[RocketMQ快速入门]()。
-   已创建数据转发规则和编写处理数据的SQL，请参见[设置数据流转规则](/cn.zh-CN/消息通信/云产品流转/设置数据流转规则.md)。

1.  登录[物联网平台控制台](http://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在左侧导航栏，选择**规则引擎** \> **云产品流转**。

4.  单击规则对应的**查看**，进入数据流转规则页。

5.  单击**转发数据**一栏对应的**添加操作**。

6.  在添加操作对话框中，选择操作为**发送数据到消息队列（RocketMQ）中**。按照界面提示，设置其他信息，单击**确认**。

    ![数据流转到MQ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1176700061/p166330.png)

    |参数|说明|
    |:-|:-|
    |选择操作|选择**发送数据到消息队列（RocketMQ）中**。|
    |地域|选择RocketMQ所在地域。|
    |实例|选择RocketMQ实例。 您可以单击**创建实例**，跳转到消息队列控制台，创建RocketMQ实例，请参见[消息队列文档]()。 |
    |Topic|选择用于接收物联网平台数据的RocketMQ Topic。 您可以单击**创建Topic**，跳转到消息队列控制台，创建RocketMQ Topic。 |
    |Tag|（可选）设置标签。 设置标签后，所有通过该操作流转到RocketMQ对应Topic里的消息都会携带该标签。您可以在RocketMQ消息消费端，通过标签进行消息过滤。

标签长度限制为128字节，可以输入常量或变量。变量格式为$\{key\}，代表SQL处理后的JSON数据中key对应的value值。 |
    |授权|授权物联网平台将数据写入RocketMQ。 如您还未创建相关角色，单击**创建RAM角色**，跳转到RAM控制台，创建角色和授权策略，请参见[创建RAM角色](/cn.zh-CN/角色管理/创建RAM角色/创建可信实体为阿里云账号的RAM角色.md)。 |

7.  回到云产品流转页，单击规则对应的**启动**按钮启动规则。

8.  测试。

    向规则SQL中定义的Topic发布一条消息，然后到RocketMQ控制台消息查询页，查看是否成功接收到消息。


[设备消息通过RocketMQ流转到服务器](/cn.zh-CN/最佳实践/消息通信/设备消息通过RocketMQ流转到服务器.md)

