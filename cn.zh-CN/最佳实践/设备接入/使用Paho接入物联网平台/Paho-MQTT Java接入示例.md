---
keyword: [IoT, 物联网, Paho.MQTT, Java]
---

# Paho-MQTT Java接入示例

本文档介绍如何使用Java语言的Paho MQTT库接入阿里云物联网平台，并进行物模型消息通信。

已在物联网平台中，创建了产品和设备，并在产品的功能定义页签下，定义一个LightSwitch属性。

请参见[创建产品](/cn.zh-CN/设备接入/创建产品.md)、[单个创建设备](/cn.zh-CN/设备接入/创建设备/单个创建设备.md)和[单个添加物模型](/cn.zh-CN/设备管理/物模型/单个添加物模型.md)。

## 准备开发环境

本示例使用的开发环境如下：

-   操作系统：macOS
-   JDK版本：[JDK8](https://www.oracle.com/technetwork/pt/java/javase/downloads/jdk8-downloads-2133151.html)
-   集成开发环境：[IntelliJ IDEA社区版](https://www.jetbrains.com/idea/)

## 下载Java语言的Paho MQTT库

在Maven工程中添加如下依赖：

```
<dependencies>
  <dependency>
      <groupId>org.eclipse.paho</groupId>
      <artifactId>org.eclipse.paho.client.mqttv3</artifactId>
      <version>1.2.0</version>
    </dependency>
</dependencies>
```

## 接入物联网平台

1.  单击打开[MqttSign.java](http://code.aliyun.com/edward.yangx/public-docs/wikis/user-guide/linkkit/Paho_MQTT_Guide/MqttSign.java)，下载阿里云提供的计算MQTT连接参数所需的源码。

    MqttSign.java文件定义了MqttSign类，类说明如下：

    -   原型：

        ```
        class MqttSign
        ```

    -   功能：

        用于计算设备接入物联网平台的MQTT连接参数username、password和clientid。

    -   成员：

        |类型定义|方法描述|
        |----|----|
        |public void|`calculate(String productKey, String deviceName, String deviceSecret)`根据设备的productKey、deviceName和deviceSecret计算出MQTT连接参数username、password和clientid。 |
        |public String|`getUsername()`用于获取MQTT建连参数username。 |
        |public String|`getPassword()`用于获取MQTT建连参数password。 |
        |public String|`getClientid()`用于获取MQTT建连参数clientid。 |

2.  打开IntelliJ IDEA，创建项目。

3.  将MqttSign.java导入项目中。

4.  在项目中，添加实现设备接入物联网平台的程序文件。

    您需编写程序调用MqttSign.java中的MqttSign类计算MQTT连接参数，实现接入物联网平台和通信。

    开发说明和示例代码如下：

    -   调用MqttSign计算MQTT连接参数。

        ```
        String productKey = "a1X2bEn****";
        String deviceName = "example1";
        String deviceSecret = "ga7XA6KdlEeiPXQPpRbAjOZXwG8y****";
        
        // 计算MQTT连接参数。
        MqttSign sign = new MqttSign();
        sign.calculate(productKey, deviceName, deviceSecret);
        
        System.out.println("username: " + sign.getUsername());
        System.out.println("password: " + sign.getPassword());
        System.out.println("clientid: " + sign.getClientid());
        ```

    -   调用Paho MQTT客户端连接物联网平台。

        ```
        //接入物联网平台的域名。
        String port = "443";
        String broker = "ssl://" + productKey + ".iot-as-mqtt.cn-shanghai.aliyuncs.com" + ":" + port;
        
        // Paho MQTT客户端。
        MqttClient sampleClient = new MqttClient(broker, sign.getClientid(), persistence);
        
        // Paho MQTT连接参数。
        MqttConnectOptions connOpts = new MqttConnectOptions();
        connOpts.setCleanSession(true);
        connOpts.setKeepAliveInterval(180);
        connOpts.setUserName(sign.getUsername());
        connOpts.setPassword(sign.getPassword().toCharArray());
        sampleClient.connect(connOpts);
        System.out.println("Broker: " + broker + " Connected");
        ```

        **说明：** 代码中的地域代码（cn-shanghai），需设置为您的物联网平台设备所在地域代码。地域代码表达方法，请参见[地域和可用区](/cn.zh-CN/产品简介/地域和可用区.md)。

    -   发布消息。

        以下示例代码上报物模型属性LightSwitch。

        ```
        // Paho MQTT发布消息。
        String topic = "/sys/" + productKey + "/" + deviceName + "/thing/event/property/post";
        String content = "{\"id\":\"1\",\"version\":\"1.0\",\"params\":{\"LightSwitch\":1}}";
        MqttMessage message = new MqttMessage(content.getBytes());
        message.setQos(0);
        sampleClient.publish(topic, message);
        ```

        物模型通信数据格式，请参见[设备属性、事件、服务](/cn.zh-CN/设备管理/Alink协议/设备属性、事件、服务.md)。

        如果您要使用自定义Topic通信，请参见[什么是Topic](/cn.zh-CN/设备接入/消息通信Topic/什么是Topic.md)。

    -   订阅Topic，获取云端下发消息。

        以下示例中，订阅的是上报属性值后，物联网平台返回应答消息的Topic。

        ```
        class MqttPostPropertyMessageListener implements IMqttMessageListener {
            @Override
            public void messageArrived(String var1, MqttMessage var2) throws Exception {
                System.out.println("reply topic  : " + var1);
                System.out.println("reply payload: " + var2.toString());
            }
        }
        ...
        // Paho MQTT消息订阅。
        String topicReply = "/sys/" + productKey + "/" + deviceName + "/thing/event/property/post_reply";
        sampleClient.subscribe(topicReply, new MqttPostPropertyMessageListener());
        ```

    关于设备、服务器和物联网平台的通信方式介绍，请参见[通信方式概述](/cn.zh-CN/消息通信/通信方式概述.md)。

5.  编译项目。


## 示例Demo

使用Demo代码程序接入物联网平台。

1.  [下载Demo代码包](http://code.aliyun.com/edward.yangx/public-docs/wikis/user-guide/linkkit/Paho_MQTT_Guide/aiot-java-demo.zip)，并解压缩。

2.  打开IntelliJ IDEA，导入Demo包中的示例工程aiot-java-demo。

3.  在src/main/java/com.aliyun.iot/App中，修改设备信息为您的设备信息。

    -   将productKey、deviceName和deviceSecret替换为您的设备证书信息。
    -   将`String broker = "ssl://" + productKey + ".iot-as-mqtt.cn-shanghai.aliyuncs.com" + ":" + port;`中的地域代码（cn-shanghai）替换为您的物联网平台设备所在地域代码。地域代码表达方法，请参见[地域和可用区](/cn.zh-CN/产品简介/地域和可用区.md)。
4.  运行App程序。

    运行成功日志如下图所示。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6131649951/p72437.png)

    登录[物联网平台控制台](http://iot.console.aliyun.com/)，在对应的实例下，可查看设备状态和日志。

    -   选择**设备管理** \> **设备**，可看到该设备的状态显示为**在线**。
    -   选择**监控运维** \> **日志服务**，可查看**云端运行日志**和**设备本地日志**日志。详情请参见[云端运行日志](/cn.zh-CN/监控运维/日志服务/云端运行日志.md)、[设备本地日志](/cn.zh-CN/监控运维/日志服务/设备本地日志.md)。

## 错误码

如果设备通过MQTT协议接入物联网平台失败，请根据错误码排查问题。服务端错误码说明，请参见[错误排查](/cn.zh-CN/最佳实践/设备接入/使用Paho接入物联网平台/错误排查.md)。

