---
keyword: [Internet of Things, IoT Platform, IoT, device, message, communication, topic, topic category, wildcard, custom topic, publish, subscribe]
---

# Customize a topic category

This article describes how to customize the topic category of a product. Each topic category of a product is used by all devices of the product.

## Procedure

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com).

2.  In the left-side navigation pane, choose **Devices** \> **Products**.

3.  On the Products page, find the product for which you want to customize a topic category, and click **View**.

4.  On the Product Details page, choose **Topic Categories** \> **Topic Category** \> **Edit Topic Category**.

5.  Set the required parameters, and click **OK**.

    ![Edit Topic Category](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5613490061/p170005.png)

    |Parameter|Description|
    |---------|-----------|
    |Device Operation Authorizations|The device access to the topic category. Valid values: **Publish**, **Subscribe**, and **Publish and Subscribe**.|
    |Topic Category|The name of the topic category. The name can contain letters, digits, and underscores \(\_\). Each field of the topic category cannot be empty. **Note:** Only if you set the Device Operation Authorizations parameter of a topic category to **Subscribe**, you can specify the + and \# wildcards in the topic category. These wildcards allow devices to subscribe to multiple topics at a time. For more information about how to use wildcards, see [A topic that includes one or more wildcards](#section_4x5_31w_af5). |
    |Description|The description.|


## A topic that includes one or more wildcards

If you set the Device Operation Authorizations parameter to **Subscribe**, IoT Platform allows you to specify two wildcards in a topic. This feature allows a device to subscribe to multiple topics at a time.

|Wildcard|Description|
|:-------|:----------|
|\#|This wildcard must be specified for the last field in a topic and can match all filed values at the current level and sub-levels. For example, you create the `/a1aycMA****/${deviceName}/user/#` topic category. If Device 1 subscribes to the `/a1aycMA****/device1/user/#` topic, the device subscribes to all topics that start with `/a1aycMA****/device1/user/`, for example, `/a1aycMA****/device1/user/update` and `/a1aycMA****/device1/user/update/error`. |
|+|This wildcard can match all field values at the current level. For example, you create the `/a1aycMA****/${deviceName}/user/+/error` topic category. If Device 1 subscribes to the `/a1aycMA****/device1/user/+/error` topic, the device subscribes to multiple topics, such as `/a1aycMA****/device1/user/get/error` and `/a1aycMA****/device1/user/update/error`. |

A topic that includes one or more wildcards represents a set of topics. A device can subscribe to the topic. However, on the **Topic List** page of the device, the **Post Message** action is unavailable for the topic. You cannot publish device messages to the topic.

## Topic-based communication

IoT Platform calls the [Pub](/intl.en-US/Developer Guide (Cloud)/API reference/Communications/Pub.md) API operation to publish messages to a specified topic. Devices receive messages from IoT Platform by subscribing to the topic.

For more information about topic-based communication, see [Use custom topics for communication](/intl.en-US/Best Practices/Communications/Use custom topics for communication.md).

