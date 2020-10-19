---
keyword: [物联网, IoT, 物联网平台, 创建, 注册, 设备, LoRa, LoRaWAN]
---

# 创建LoRa设备

物联网平台支持创建LoRa产品和设备。创建LoRa产品后，可以根据本文操作，创建LoRa设备。您可以单个创建LoRa设备，也可以批量操作。

-   已创建连网方式为LoRaWAN的产品，参见[创建产品](/cn.zh-CN/设备接入/创建产品.md)。
-   已确定设备的证书信息。证书信息有两种组合：

    -   DevEUI、PIN Code：阿里云颁发的证书信息，一般印刷于设备外显标签上。
    -   DevEUI、JoinEui、AppKey：对于购买时开启了Link WAN的实例，您可使用该组证书信息单个创建设备，不支持批量创建。当设备无阿里云颁发的证书信息时，用户自定义该组证书信息：DevEUI、JoinEui定义为16位HEX，且JoinEui不能以d896e0开头，AppKey定义为32位HEX。
    以上两种证书信息中，DevEUI都是LoRa设备的唯一标识符，采用LoRaWAN协议标准规范。请确保DevEUI在产品下唯一，且已烧录到设备中。


## 单个创建设备

1.  登录[物联网平台控制台](http://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  左侧导航栏选择**设备管理** \> **设备**，单击**添加设备**。

4.  在对话框中选择已创建的连网方式为LoRaWAN的产品。新创建的设备将继承该产品定义好的功能和特性。

5.  填写设备的证书信息。

    -   对于公共实例和购买时未开启Link WAN的实例，填写DevEUI和PIN Code：
    -   对于购买时开启了Link WAN的实例，可以选择：
        -   **阿里云颁发**：填写DevEUI和PIN Code。
        -   **用户自定义**：填写DevEUI、JoinEui和AppKey。
6.  单击**确认**，完成设备创建。

    设备创建完成后，将自动弹出**查看设备证书**弹框。您可以查看、复制LoRa设备的证书信息。


## 批量创建设备

1.  登录[物联网平台控制台](http://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  左侧导航栏选择**设备管理** \> **设备**，单击**批量添加**。

4.  选择已创建的连网方式为LoRaWAN的产品。新创建的设备将继承该产品定义好的功能和特性。

5.  单击**下载.csv模板**下载表格模板，在模板中填写DevEUI和PIN Code，然后将填好的表格上传至控制台。

6.  单击**确认**，完成设备创建。


您可以参见[物联网络管理平台文档](https://help.aliyun.com/document_detail/96549.html)搭建物联网所需的网络服务和开发设备端（即网关开发和节点开发）。

**说明：** 在物联网平台创建LoRa设备后，您无需再在物联网络管理平台录入设备信息和配置数据流转。

