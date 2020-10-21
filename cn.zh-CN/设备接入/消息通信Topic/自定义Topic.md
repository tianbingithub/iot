---
keyword: [物联网, 物联网平台, IoT, 设备, 消息, 通信, Topic, Topic类, 通配符, 自定义Topic, 发布, 订阅]
---

# 自定义Topic

本文介绍如何为产品自定义Topic类。自定义Topic类将自动映射到该产品下的所有设备中。

## 操作步骤

1.  登录[物联网平台控制台](https://iot.console.aliyun.com)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在左侧导航栏，选择**设备管理** \> **产品**。

4.  在产品页面，找到需要自定义Topic类的产品，并单击对应操作栏中的**查看**按钮。

5.  在产品详情页面，单击**Topic类列表** \> **自定义Topic** \> **定义Topic类**。

6.  配置参数，单击**确认**。

    ![自定义Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0645559951/p134081.png)

    |参数|描述|
    |--|--|
    |设备操作权限|设备对该Topic的操作权限，可设置为**发布**、**订阅**、**发布和订阅**。|
    |Topic类|将Topic类填充完整。类目命名只能包含字母、数字和下划线（\_），每级类目不能为空。 **说明：** 只有设备操作权限为**订阅**时，才可以使用通配符+和\#自定义Topic类，以便设备实现批量订阅Topic。通配符使用方法请参见[带通配符的自定义Topic](#section_4x5_31w_af5) 。 |
    |描述|可输入文字，描述该Topic类。|


## 带通配符的自定义Topic

物联网平台支持在设备操作权限为**订阅**的自定义Topic中使用两种通配符，以便设备实现批量订阅Topic。

|通配符|描述|
|:--|:-|
|\#|\#只能出现在Topic的最后一个类目，代表本级及下级所有类目。 例如：自定义Topic`/a1aycMA****/${deviceName}/user/#`。设备device1订阅`/a1aycMA****/device1/user/#`，表示订阅以`/a1aycMA****/device1/user/`为开头的全部Topic，包含`/a1aycMA****/device1/user/update`、`/a1aycMA****/device1/user/update/error`等Topic。 |
|+|代表本级所有类目。 例如：自定义Topic`/a1aycMA****/${deviceName}/user/+/error`。设备device1订阅`/a1aycMA****/device1/user/+/error`，表示订阅`/a1aycMA****/device1/user/get/error`、`/a1aycMA****/device1/user/update/error`等Topic。 |

由于带通配符的Topic实质为一组Topic的集合，因此带通配符的Topic不支持在设备的**Topic列表**页面执行**发布消息**操作，将消息发布到已订阅该Topic的设备。

## 自定义Topic通信

服务端调用[Pub](/cn.zh-CN/云端开发指南/云端API参考/消息通信/Pub.md)，可向指定的自定义Topic发布消息。设备通过订阅该Topic，接收来自服务端的消息。

使用自定义Topic通信的示例，请参见[使用自定义Topic进行通信](/cn.zh-CN/最佳实践/消息通信/使用自定义Topic进行通信.md)。

