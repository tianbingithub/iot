---
keyword: [物联网, 物联网平台, IoT, 规则引擎, 流转数据, 发送数据, 另一Topic]
---

# 数据转发到另一Topic

您可以设置将规则SQL处理完的数据，转发到另一个Topic中，实现M2M通信或者更多其他通信场景。

已创建数据转发规则和编写处理数据的SQL，请参见[设置数据流转规则](/cn.zh-CN/消息通信/云产品流转/设置数据流转规则.md)。

规则引擎数据转发功能将Topic 1中的数据转发到Topic 2内。

数据流转示意图如下。

![数据流转到topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5086549951/p97231.png)

1.  登录[物联网平台控制台](http://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在左侧导航栏，选择**规则引擎** \> **云产品流转**。

4.  单击规则对应的**查看**，进入数据流转规则页。

5.  单击**转发数据**一栏对应的**添加操作**。

6.  在添加操作对话框中，选择操作为**发布到另一个Topic**。按照界面提示，设置其他信息，单击**确认**。

    ![发布到另一个Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8364700061/p166307.png)

    |参数|说明|
    |:-|:-|
    |选择操作|选择**发布到另一个Topic**。|
    |Topic|选择数据转发目的地Topic。 可选的Topic类型：

    -   自定义：目的地Topic为一个自定义Topic。该自定义Topic的设备操作权限需为**订阅**，即所属设备可订阅这个Topic，获取转发的消息。
    -   物模型数据下发：目的地Topic为设备接收设置属性值指令的Topic：`thing/service/property/set`。设备从该Topic接收转发数据，并根据数据内容，设置属性值。用于目的地Topic所属设备根据转发的数据更改属性值的场景。
选择Topic类型后，您还需选择产品、设备和Topic名称，即指定具体Topic。 |

7.  回到云产品流转页，单击规则对应的**启动**按钮启动规则。


[基于规则引擎的M2M设备间通信](/cn.zh-CN/最佳实践/消息通信/M2M设备间通信/基于规则引擎的M2M设备间通信.md)

