---
keyword: [物联网, 物联网平台, IoT, 规则引擎, 流转数据, 发送数据, DataHub]
---

# 数据转发到DataHub

您可以使用规则引擎将数据转到DataHub上，再由DataHub将数据流转至实时计算、MaxCompute等服务中，以实现更多计算场景。

-   已创建DataHub Project和用于接收数据的Topic。DataHub使用方法，请参见[DataHub文档](https://help.aliyun.com/document_detail/158789.html)。
-   已创建数据转发规则和编写处理数据的SQL，请参见[设置数据流转规则](/cn.zh-CN/消息通信/云产品流转/设置数据流转规则.md)。

1.  登录[物联网平台控制台](http://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在左侧导航栏，选择**规则引擎** \> **云产品流转**。

4.  单击规则对应的**查看**，进入数据流转规则页。

5.  单击**转发数据**一栏对应的**添加操作**。

6.  在添加操作对话框中，选择操作为**发送数据到DataHub中**。按照界面提示，设置其他信息，单击**确认**。

    ![数据流转到DataHub](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7438320061/p166667.png)

    |参数|说明|
    |:-|:-|
    |选择操作|选择**发送数据到DataHub中**。|
    |地域|选择DataHub所在地域。|
    |Project|选择DataHub Project。您可以单击**创建Project**，跳转到DataHub控制台，创建DataHub Project，请参见[DataHub文档](https://help.aliyun.com/document_detail/158789.html)。 |
    |Topic|选择接收数据的DataHub Topic。选择Topic后，规则引擎会自动获取Topic中的Schema，规则引擎筛选出来的数据将会映射到对应的Schema中。

**说明：**

    -   将数据映射到Schema时，需使用`${}`，否则存入表中的将会是一个常量。
    -   Schema与规则引擎的的数据类型必须保持一致，否则无法存储。
您可以单击**创建Topic**，跳转到DataHub控制台，创建DataHub Topic。 |
    |角色|授权物联网平台将数据写入DataHub。如您还未创建相关角色，单击**创建RAM角色**，跳转到RAM控制台，创建角色和授权策略，请参见[创建RAM角色](/cn.zh-CN/角色管理/创建RAM角色/创建可信实体为阿里云账号的RAM角色.md)。 |

7.  回到云产品流转页，单击规则对应的**启动**按钮启动规则。


[通过大数据平台搭建设备监控大屏](/cn.zh-CN/最佳实践/场景应用/通过大数据平台搭建设备监控大屏.md)

