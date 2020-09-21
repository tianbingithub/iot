---
keyword: [物联网, IoT, 物联网平台, MQTT, 一型一密, 动态注册, ClientID, DeviceToken, DeviceSecret]
---

# 基于MQTT通道的设备动态注册

直连设备可通过MQTT通道进行动态注册，即使用一型一密连接认证方式连接物联网平台。设备先基于TLS建立与物联网平台的连接，获取TCP连接所需信息，再断开连接，并重新建立TCP连接进行通信。下面介绍动态注册流程。

已完成[一型一密文档](/intl.zh-CN/设备接入/设备安全认证/一型一密.md)中的以下步骤：

1.  创建产品。
2.  开启动态注册。
3.  添加设备。
4.  产线烧录。

## 动态注册流程

![流程](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2645559951/p146802.png)

1.  设备发送CONNECT报文，报文中包含动态注册参数，请求建立连接。

    **说明：** 目前，动态注册只支持使用TLS建立连接，不支持TCP直连；动态注册时，云端不会校验MQTT连接的Keep Alive（保活时间），因此可以不用设置Keep Alive时间。

    -   MQTT连接域名：
        -   您购买的实例的连接域名请在物联网平台控制台实例管理页面，单击实例对应的**查看**，进入实例详情页查看。
        -   公共实例的连接域名为`${YourProductKey}.iot-as-mqtt.${YourRegionId}.aliyuncs.com:1883`。 其中：
            -   $\{YourProductKey\}：请替换为设备所属产品的的ProductKey。可从物联网平台控制台设备详情页获取。
            -   $\{YourRegionId\}：请参见[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)替换为您的Region ID。
    -   CONNECT报文的动态注册参数：

        -   当设备属于您购买的实例，且使用[一型一密](/intl.zh-CN/设备接入/设备安全认证/一型一密.md)免预注册认证方式时，动态注册参数如下：

            ```
            mqttClientId: clientId+"|securemode=2,authType=xxxx,random=xxxx,signmethod=xxxx,instanceId=xxxx|"
            mqttUserName: deviceName+"&"+productKey
            mqttPassword: sign_hmac(productSecret,content) 
            ```

        -   当设备属于公共实例，或使用[一型一密](/intl.zh-CN/设备接入/设备安全认证/一型一密.md)预注册认证方式时，动态注册参数如下：

            ```
            mqttClientId: clientId+"|securemode=2,authType=xxxx,random=xxxx,signmethod=xxxx|"
            mqttUserName: deviceName+"&"+productKey
            mqttPassword: sign_hmac(productSecret,content) 
            ```

        参数说明：

        -   mqttClientId

            参数取值中包含的详细参数如下表所示。

            |参数|说明|
            |--|--|
            |clientId|客户端ID。建议使用设备的MAC地址或SN码，长度在64个字符内。|
            |securemode|安全模式。动态注册功能仅支持使用TLS通道连接，不支持TCP直连，因此必须设置为2 。|
            |authType|一型一密认证方式，不同类型将返回不同的认证参数：            -   register：[一型一密](/intl.zh-CN/设备接入/设备安全认证/一型一密.md)预注册认证方式，返回DeviceSecret。
            -   regnwl：[一型一密](/intl.zh-CN/设备接入/设备安全认证/一型一密.md)免预注册认证方式，返回DeviceToken、ClientID。 |
            |random|随机数。您自定义随机数。|
            |signMethod|签名算法。目前支持hmacmd5、hmacsha1、hmacsha256。|
            |instanceId|实例ID。请在物联网平台控制台实例管理页面查看。|

        -   mqttUserName

            组成结构：`deviceName+"&"+productKey`

            示例：`device1&al123456789`

        -   mqttPassword

            计算方法：`sign_hmac(productSecret,content)`

            其中，content的值是提交给服务器的必需参数和值（deviceName、productKey、random）按照字母顺序排序、拼接（无拼接符号）的字符串。然后，将content的值通过mqttClientId中的signMethod指定的算法，使用产品的ProductSecret进行签名计算。

            示例：`hmac_sha1(h1nQFYPZS0mW****, deviceNamedevice1productKeyal123456789random123)`

2.  物联网平台返回CONNECT ACK。

    返回0，则表示建连成功，即动态注册成功。

    建连失败，则需根据返回的错误码，确定错误原因。

    设备发送连接请求后，物联网平台返回的结果状态码和说明如下表。

    |结果码|消息|说明|
    |---|--|--|
    |0|CONNECTION\_ACCEPTED|动态注册成功。|
    |2|IDENTIFIER\_REJECTED|参数错误。原因可能是：    -   必填参数缺失或格式错误。
    -   您使用了TCP直连注册。动态注册只能使用TLS通道。 |
    |3|SERVER\_UNAVAILABLE|云端错误。请稍后再试。|
    |4|BAD\_USERNAME\_OR\_PASSWORD|动态注册失败，鉴权未通过。请检查传入的mqttUserName和mqttPassword取值是否正确。 |

3.  建立连接后，物联网平台使用Topic：`/ext/register`，根据CONNECT报文中的authType，返回不同的认证参数：

    **说明：** 设备无需订阅推送证书的Topic。

    -   authType取值为register：一型一密预注册认证方式，返回DeviceSecret。

        物联网平台推送的消息Payload格式如下：

        ```
        {
          "productKey" : "xxx",
          "deviceName" : "xxx",
          "deviceSecret" : "xxx"
        }
        ```

    -   authType取值为regnwl：一型一密免预注册认证方式，返回ClientID、DeviceToken。

        物联网平台推送的消息Payload格式如下：

        ```
        {
          "productKey" : "xxx",
          "deviceName" : "xxx",
          "clientId" : "xxx",
          "deviceToken" : "xxx"
        }
        ```

4.  设备收到并保存DeviceSecret，或ClientID和DeviceToken的组合，断开当前MQTT连接。

    设备可以通过发送DISCONNECT报文或直接断开TCP连接，断开当前连接。

    如果设备未断开此连接，15秒之后，物联网平台会主动断开连接。

    如果您使用Eclipse Paho MQTT客户端，设置`MqttConnectOptions.setAutomaticReconnect(false)`关闭自动重连。否则，注册成功并TCP断连后，重连逻辑会发起新的动态注册请求。

5.  设备使用DeviceSecret，或使用ClientID和DeviceToken的组合，再次发起MQTT连接请求，建立设备与物联网平台的连接，进行消息通信。详情请参见[MQTT-TCP连接通信](/intl.zh-CN/设备接入/使用开放协议自主接入/MQTT协议接入/MQTT-TCP连接通信.md)。


