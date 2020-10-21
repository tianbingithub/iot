---
keyword: [MQTT, 物联网, IoT, 物联网平台, TCP, 设备连接, 通信]
---

# MQTT-TCP连接通信

本文档主要介绍基于TCP的MQTT连接，连接方式为MQTT客户端直连。

在进行MQTT CONNECT协议设置时，注意：

-   如果同一个设备证书（ProductKey、DeviceName和DeviceSecret）或同一组ProductKey、DeviceName、ClientID、DeviceToken同时用于多个物理设备连接，可能会导致客户端频繁上下线。因为新设备连接认证时，原设备会被迫下线，而设备被下线后，又会自动尝试重新连接。
-   MQTT连接模式中，设备端[Link SDK](https://www.alibabacloud.com/help/doc-detail/96627.htm)断开后会自动重连。您可以通过日志服务查看设备行为。

## MQTT客户端直连

1.  （可选）推荐使用TLS加密。

    -   设备端Link SDK已配置TLS加密，您无需自行配置。
    -   若您自行开发设备端SDK，需要[下载根证书](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt)。根证书使用方法，请参见[mbed TLS](https://tls.mbed.org/kb/how-to/mbedtls-tutorial)。
2.  使用MQTT客户端连接服务器。连接方法，请参见[开源MQTT客户端](https://github.com/mqtt/mqtt.github.io/wiki/libraries)。

    如果需了解MQTT协议，请参见 [MQTT官方文档](http://mqtt.org/) 。

    **说明：** 若使用第三方代码，阿里云不提供技术支持。

3.  MQTT连接。

    建议您使用设备端SDK接入物联网平台。如果您自行开发接入，连接参数如下。

    |接入域名|    -   您购买的实例的接入域名，请在物联网平台控制台**实例管理**页面，单击实例对应的**查看**，进入实例详情页查看。
    -   公共实例的接入域名：`${YourProductKey}.iot-as-mqtt.${YourRegionId}.aliyuncs.com:1883`。 其中：
        -   $\{YourProductKey\}：请替换为设备所属产品的ProductKey。可从物联网平台控制台设备详情页获取。
        -   $\{YourRegionId\}：请参见[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm) |
    |可变报头（variable header）：Keep Alive|CONNECT指令中需包含Keep Alive（保活时间）。保活心跳时间取值范围为30秒~1200秒。如果心跳时间不在此区间内，物联网平台会拒绝连接。建议取值300秒以上。如果网络不稳定，将心跳时间设置高一些。|
    |MQTT的CONNECT报文参数|    -   一机一密、一型一密预注册认证方式：使用设备证书（ProductKey、DeviceName和DeviceSecret）连接。

        ```
mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
mqttUsername: deviceName+"&"+productKey
mqttPassword: sign_hmac(deviceSecret,content)
        ```

        -   mqttClientId：格式中`| |`内为扩展参数。
        -   clientId：表示客户端ID，建议使用设备的MAC地址或SN码，64个字符内。
        -   securemode：表示目前安全模式，可选值有2（TLS直连模式）和3（TCP直连模式）。
        -   signmethod：表示签名算法类型。支持hmacmd5，hmacsha1和hmacsha256，默认为hmacmd5。
        -   timestamp：表示当前时间毫秒值，可以不传递。
        -   mqttPassword：sign签名需把提交给服务器的参数按字典排序后，根据signmethod加签。签名计算示例，请参见[MQTT连接签名示例](/intl.zh-CN/设备接入/使用开放协议自主接入/MQTT协议接入/MQTT连接签名示例.md)。
        -   content的值为提交给服务器的参数（ProductKey、DeviceName、timestamp和clientId），按照字母顺序排序， 然后将参数值依次拼接。
示例：

假设`clientId = 12345，deviceName = device， productKey = pk， timestamp = 789，signmethod=hmacsha1，deviceSecret=secret`，那么使用TCP方式提交给MQTT的参数如下：

        ```
mqttclientId=12345|securemode=3,signmethod=hmacsha1,timestamp=789|
mqttUsername=device&pk
mqttPassword=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString(); 
        ```

加密后的Password为二进制转16制字符串，示例结果为：

        ```
FAFD82A3D602B37FB0FA8B7892F24A477F85****
        ```

    -   一型一密免预注册认证方式：使用ProductKey、DeviceName、ClientID、DeviceToken连接。

        ```
mqttClientId: clientId+"|securemode=-2,authType=connwl|"
mqttUsername: deviceName+"&"+productKey
mqttPassword: deviceToken
        ```

        -   mqttClientId：格式中`| |`内为扩展参数。
        -   clientId、deviceToken：设备动态注册时获得的ClientID、DeviceToken，请参见[基于MQTT通道的设备动态注册](/intl.zh-CN/设备接入/使用开放协议自主接入/MQTT协议接入/基于MQTT通道的设备动态注册.md)。
        -   securemode：表示目前安全模式，采用一型一密免预注册时，固定取值为-2。
        -   authType：表示认证方式，采用一型一密免预注册时，固定取值为regnwl。 |


## 示例

使用开源MQTT客户端接入物联网平台的示例，请参见：

-   [Paho-MQTT Go接入示例](/intl.zh-CN/最佳实践/设备接入/使用Paho接入物联网平台/Paho-MQTT Go接入示例.md)
-   [Paho-MQTT C\#接入示例](/intl.zh-CN/最佳实践/设备接入/使用Paho接入物联网平台/Paho-MQTT C#接入示例.md)
-   [Paho-MQTT C接入示例](/intl.zh-CN/最佳实践/设备接入/使用Paho接入物联网平台/Paho-MQTT C接入示例.md)
-   [Paho-MQTT Java接入示例](/intl.zh-CN/最佳实践/设备接入/使用Paho接入物联网平台/Paho-MQTT Java接入示例.md)
-   [Paho-MQTT Android接入示例](/intl.zh-CN/最佳实践/设备接入/使用Paho接入物联网平台/Paho-MQTT Android接入示例.md)

## MQTT保活

设备端在保活时间间隔内，至少需要发送一次报文，包括ping请求。

如果物联网平台在保活时间内无法收到任何报文，物联网平台会断开连接，设备端需要进行重连。

连接保活时间的取值范围为30秒~1200秒。建议取值300秒以上。

