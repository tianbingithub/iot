# Linux设备接入物联网平台

阿里云提供的设备端C语言SDK可以直接运行于Linux系统，并通过MQTT协议接入物联网平台。本文以在Ubuntu x86\_64系统上编译设备端C语言SDK为例，介绍设备上云的配置和开发过程。

有关设备端C语言SDK详细信息，请参见[C SDK文档](https://help.aliyun.com/document_detail/96623.html)。

## 创建产品和设备

1.  登录[物联网平台控制台](https://iot.console.aliyun.com)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在左侧导航栏，选择**设备管理** \> **产品**，再单击**创建产品**，创建一个产品。

    |参数|说明|
    |--|--|
    |产品名称|自定义产品名称。|
    |所属品类|选择**自定义品类**。|
    |节点类型|选择**直连设备**。|
    |连网方式|选择**Wi-Fi**。|
    |数据格式|选择**ICA标准数据格式（Alink JSON）**。|
    |认证方式|选择**设备密钥**。|

4.  在左侧导航栏，选择**设备**，再单击**添加设备**，在刚创建的产品下添加设备。

    设备创建成功后，获取设备证书信息（ProductKey、DeviceName和DeviceSecret）。


## 定义产品物模型

物联网平台提供的设备端C SDK Demo包中，包含一个完整的物模型JSON文件。本示例中，导入该物模型文件，生成产品的物模型。

1.  编辑物模型文件。

    1.  下载[C SDK Demo](https://code.aliyun.com/linkkit/c-sdk/repository/archive.zip?ref=v3.0.1)。

    2.  解压Demo包后，打开src/dev\_model/examples目录下的model\_for\_examples.json文件。

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7431649951/p71412.png)

    3.  将物模型JSON文件中的productKey的值替换为您在物联网平台上创建的产品ProductKey，然后保存文件。

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7431649951/p71413.png)

2.  在物联网平台控制台对应实例的产品页，找到之前创建的产品，单击对应的**查看**。

3.  在产品详情页功能定义页签下，单击**编辑草稿** \> **快速导入**。

4.  在弹出的对话框中，选择**导入物模型**，上传上一步编辑好的物模型JSON文件，单击**确定**。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7431649951/p71414.png)

    导入成功后，该文件定义的所有功能将显示在自定义功能列表中。

5.  单击**发布上线**，将物模型发布为正式版。


## 配置SDK

在开发工具中，导入C SDK Demo，并修改配置文件中的信息为您的设备信息。

1.  在SDK Demo中wrappers/os/ubuntu目录下HAL\_OS\_linux.c文件中，修改设备证书信息为您的设备证书信息。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7431649951/p71419.png)

2.  编译SDK。在SDK根目录中，执行make reconfig，并选择3，然后make。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7431649951/p71422.png)

3.  测试运行SDK。

    在SDK根目录中，执行./src/dev\_model/examples/linkkit\_example\_solo。执行结果如下图。

    ![运行SDK](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7431649951/p140146.png)

    SDK运行成功后，可在物联网平台控制台对应实例下，设备对应的设备详情页，查看设备状态和设备上报的物模型数据。


