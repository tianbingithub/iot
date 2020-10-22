---
keyword: [物联网, IoT, 物联网平台, MQTT, MQTT.fx]
---

# 使用MQTT.fx接入物联网平台

MQTT.fx是一款基于Eclipse Paho，使用Java语言编写的MQTT客户端工具，支持通过Topic订阅和发布消息。下面以MQTT.fx为例，介绍使用第三方软件以MQTT协议接入物联网平台。

已在[物联网平台控制台](https://iot.console.aliyun.com)创建产品和设备，并获取设备证书信息（ProductKey、DeviceName和DeviceSerect）。创建产品和设备具体操作细节，请参考[创建产品](/cn.zh-CN/设备接入/创建产品.md)、[单个创建设备](/cn.zh-CN/设备接入/创建设备/单个创建设备.md)或[批量创建设备](/cn.zh-CN/设备接入/创建设备/批量创建设备.md)。

## 使用MQTT.fx接入

1.  下载并安装MQTT.fx软件。请访问[MQTT.fx官网](https://mqttfx.jensd.de/index.php/download)。

2.  打开MQTT.fx软件，单击设置图标。

    ![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p7694.png)

3.  设置连接参数。物联网平台目前支持两种连接模式，不同模式设置参数不同：

    -   TCP直连：Client ID中`securemode=3`，无需设置SSL/TLS信息。
    -   TLS直连：Client ID中`securemode=2`，需要设置SSL/TLS信息。
    **说明：** 设置参数时，请确保参数值中或参数值的前后均没有空格。

    1.  设置基本信息。

        ![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p7698.png)

        |参数|说明|
        |:-|:-|
        |Profile Name|输入您的自定义名称。|
        |Profile Type|选择为**MQTT Broker**。|
        |MQTT Broker Profile Settings|
        |Broker Address|接入域名。         -   对于您购买的实例，接入域名请在[物联网平台控制台](https://iot.console.aliyun.com)，找到对应的实例，单击实例进入实例详情查看。
        -   公共实例的接入域名：`${YourProductKey}.iot-as-mqtt.${YourRegionId}.aliyuncs.com`。其中：
            -   $\{YourProductKey\}：请替换为设备所属产品的ProductKey。可登录[物联网平台控制台](https://iot.console.aliyun.com)，在对应实例的设备详情页获取。
            -   $\{YourRegionId\}：请参见[地域和可用区](/cn.zh-CN/产品简介/地域和可用区.md)，替换为您的Region ID。 |
        |Broker Port|设置为1883。|
        |Client ID|填写mqttClientId，用于MQTT的底层协议报文。 格式固定：`${clientId}|securemode=3,signmethod=hmacsha1|`。

完整示例：`12345|securemode=3,signmethod=hmacsha1|`。

其中：

        -   $\{clientId\}为设备的ID信息。可取任意值，长度在64字符以内。建议使用设备的MAC地址或SN码。
        -   securemode为安全模式，TCP直连模式设置为`securemode=3`，TLS直连为`securemode=2`。
        -   signmethod为算法类型，支持hmacmd5和hmacsha1。
**说明：** 输入Client ID信息后，请勿单击**Generate**。 |
        |General General栏目下的设置项可保持系统默认，也可以根据您的具体需求设置。 |

    2.  单击**User Credentials**，设置User Name和Password。

        ![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p7699.png)

        |参数|说明|
        |:-|:-|
        |User Name|由设备名DeviceName、符号（&）和产品ProductKey组成。 固定格式：`${YourDeviceName}&${YourProductKey}`。

完整示例如：`device&alxxxxxxxxx`。 |
        |Password|密码由参数值拼接加密而成。 **说明：** 如果您使用的MQTT.fx版本，在粘贴Password后不显示具体的字符串，只要光标已从输入框的前部移至了后部，则表示粘贴成功，请勿重复粘贴。

您可以使用物联网平台提供的生成工具自动生成Password，也可以手动生成Password。

        -   单击下载[Password生成小工具](https://files.alicdn.com/tpsservice/88413c66e471bec826257781969d1bc7.zip)。解压缩下载包后，双击sign文件，即可使用。

使用Password生成小工具的输入参数：

            -   productKey：设备所属产品Key。可在控制台设备详情页查看。
            -   deviceName：设备名称。可在控制台设备详情页查看。
            -   deviceSecret：设备密钥。可在控制台设备详情页查看。
            -   timestamp：（可选）时间戳。
            -   clientId：设备的ID信息，与**Client ID**中$\{clientId\}一致。
            -   method：选择签名算法类型，与**Client ID**中signmethod确定的加密方法一致。
        -   手动生成方法如下：
            1.  拼接参数。

提交给服务器的clientId、deviceName、productKey和timestamp（timestamp为非必选参数）参数及参数值依次拼接。

本例中，clientId值为12345，deviceName值为device，productKey值为alxxxxxxxxx，拼接结果为：`clientId12345deviceNamedeviceproductKeyalxxxxxxxxx`。

            2.  加密。

通过**Client ID**中确定的加密方法，使用设备deviceSecret，将拼接结果加密。

假设设备的deviceSecret值为abc123，加密计算格式为`hmacsha1(abc123,clientId12345deviceNamedeviceproductKeyalxxxxxxxxx)`。 |

    3.  （可选）TLS直连模式（即`securemode=2`）下，需要选择SSL/TLS，勾选**Enable SSL/TLS**，设置Protocol。建议Protocol选择为**TLSv1.2**。

        **说明：** TCP直连模式（即`securemode=3`）下，无需设置SSL/TLS信息，直接进入下一步。

        ![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p7734.png)

4.  设置完成后，单击右下角的**OK**。

5.  单击**Connect**进行连接。

    ![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p7735.png)


## 下行通信测试

从物联网平台发送消息，在MQTT.fx上接收消息，测试MQTT.fx与物联网平台连接是否成功 。

1.  在MQTT.fx上，单击**Subscribe**。

2.  输入一个设备具有订阅权限的自定义Topic，单击**Subscribe**，订阅这个自定义Topic。

    ![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p7736.png)

    订阅成功后，该Topic将显示在列表中。

    ![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p7737.png)

3.  登录[物联网平台控制台](https://iot.console.aliyun.com)，在对应实例下，找到该设备的设备详情页，在**Topic列表**页签下，单击已订阅的Topic对应的**发布消息**。

4.  输入消息内容，单击**确认**。

    ![发布消息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p127634.png)

5.  回到MQTT.fx上，查看是否接收到消息。

    ![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p7739.png)


## 上行通信测试

在MQTT.fx上发送消息，通过查看设备日志，测试MQTT.fx与物联网平台连接是否成功 。

1.  在MQTT.fx上，单击**Publish**。

2.  输入一个设备具有发布权限的Topic，和要发送的消息内容，单击**Publish**，向这个Topic推送一条消息。

    ![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p37281.png)

3.  登录[物联网平台控制台](https://iot.console.aliyun.com)，在对应实例下，选择**监控运维** \> **日志服务** \> **云端运行日志**，查看该设备的设备到云消息。


## 查看日志

在MQTT.fx上，单击**Log**查看操作日志和错误提示日志。

![MQTT.fx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0694559951/p7740.png)

