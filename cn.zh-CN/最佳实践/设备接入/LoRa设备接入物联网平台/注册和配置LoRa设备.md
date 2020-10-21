---
keyword: [IoT, 物联网平台, LoRa]
---

# 注册和配置LoRa设备

配置LoRa网关后，您需要创建LoRa产品和设备，定义物模型，编写和提交LoRa设备的数据解析脚本。

## 创建产品和设备

在物联网平台注册LoRa产品和设备。

1.  登录[物联网平台控制台](https://iot.console.aliyun.com)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在左侧导航栏，选择**设备管理** \> **产品**。

4.  在产品页，单击**创建产品**，创建一个连网方式为**LoRaWAN**的产品。

    |参数|说明|
    |--|--|
    |产品名称|自定义产品名称。|
    |所属分类|选择**自定义品类**。|
    |节点类型|选择**直连设备**。|
    |连网方式|选择**LoRaWAN**。 **说明：** 首次创建连网方式为LoRaWAN的产品，需单击**立即授权**，授权物联网平台访问物联网络管理平台。 |
    |入网凭证|选择您在物联网络平台中创建并已授权的入网凭证。|
    |数据格式|选择为**透传/自定义**。|

5.  在左侧导航栏，选择**设备管理** \> **设备**。

6.  在设备页，单击**添加设备**，在刚创建的产品下添加设备。

    **说明：** 设备的DevEUI和PIN Code，请在您的设备标签上查看。


## 定义物模型

本示例中，需定义以下属性：温度、湿度、二氧化碳浓度、挥发有机物、甲醛、光照强度和PM2.5。

1.  在物联网平台控制台对应实例下的左侧导航栏，选择**设备管理** \> **产品**。

2.  在产品页，找到之前创建的LoRa产品，单击对应的**查看**。

3.  在产品详情页的功能定义页签下，选择**编辑草稿** \> **添加自定义功能**。

4.  逐个添加下图中的属性。添加完成后，单击**发布上线**，发布物模型。

    ![物模型属性](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1431649951/p93006.png)


## 编写数据解析脚本

本示例中LoRa设备上报的数据是二进制格式。阿里云物联网平台的标准数据格式为Alink JSON格式，不能直接使用二进制数据进行业务处理。对此，物联网平台提供数据解析功能，您可以依据设备数据格式和物模型，编写数据解析脚本，提交至物联网平台，用于解析数据。

1.  在物联网平台控制台对应实例上，LoRa产品的产品详情页，选择**数据解析**页签。

2.  在数据解析页签下，选择脚本语言，然后在编辑脚本下的输入框中，输入数据解析脚本。

    数据解析脚本编写指导，请访问[LoRaWAN设备数据解析](/cn.zh-CN/设备管理/数据解析/LoRaWAN设备数据解析.md)。

    本示例的数据解析脚本如下：

    ```
    var PROPERTY_REPORT_METHOD = 'thing.event.property.post';
    /*
    示例数据：
    传入参数：
        AA1FC800003710FF000503690B0018013500FFFFFFFFFFFFFFFFFFFFFFFFFF6C
    输出结果：
    {
      "method": "thing.event.property.post",
      "id": 1526870753,
      "params": {
        "temperature": 10,
        "humidity": 88
      },
      "version": "1.0"
    }
    */
    //上行数据，自定义二进制转Alink JSON格式。
    function rawDataToProtocol(bytes) {
    
        var uint8Array = new Uint8Array(bytes.length);
        for (var i = 0; i < bytes.length; i++) {
            uint8Array[i] = bytes[i] & 0xff;
        }
    
        var dataView = new DataView(uint8Array.buffer, 0);
    
        var jsonMap = new Object();
    
            //属性上报method。
            jsonMap['method'] = PROPERTY_REPORT_METHOD;
            //协议版本号固定字段。
            jsonMap['version'] = '1.0';
            //标示该次请求ID值。
            jsonMap['id'] = new Date().getTime();
            var params = {};
            //12、13对应产品属性中PM2.5。
            params['pm25'] = (dataView.getUint8(13)*256+dataView.getUint8(12));
            //14、15对应产品属性中temperature。
            params['temperature'] = (dataView.getUint8(15)*256+dataView.getUint8(14))/10;
            //16、17对应产品属性中humidity。
            params['humidity'] = (dataView.getUint8(17)*256+dataView.getUint8(16));
            //18、19对应产品属性中co2。
            params['co2'] = (dataView.getUint8(19)*256+dataView.getUint8(18));
            //22、23对应产品属性中甲醛hcho。
            params['hcho'] = (dataView.getUint8(23)*256+dataView.getUint8(22))/100;
            //26、27对应产品属性中挥发性有机物voc。
            //params['voc'] = (dataView.getUint8(27)*256+dataView.getUint8(26))/100;
            //28、29对应产品属性中光照lightLux。
            params['lightLux'] = (dataView.getUint8(29)*256+dataView.getUint8(28));
    
            jsonMap['params'] = params;
    
        return jsonMap;
    }
    //下行指令，Alink JSON格式物模型数据转二进制格式。
    function protocolToRawData(json) {
        var payloadArray = [];
        // 追加LoRa下行帧头部。
        payloadArray = payloadArray.concat(0x5d);
        payloadArray = payloadArray.concat(0x0a);//厂商提供数据端口。
        payloadArray = payloadArray.concat(0x00);
        // 追加LoRa业务内容。
        return payloadArray;
    }
    ```

3.  测试脚本。

    1.  选择模拟类型为**设备上报数据**。

    2.  在模拟输入下的输入框中，输入一个模拟数据，例如：`AA1FC800003710FF000503690B0018013500FFFFFFFFFFFFFFFFFFFFFFFFFF6C`。

    3.  单击**执行**。

        运行成功后，可在**运行结果**看到以下解析结果。

        ```
        {
          "method": "thing.event.property.post",
          "id": 1577359668030,
          "params": {
            "hcho": 655.35,
            "pm25": 11,
            "lightLux": 65535,
            "co2": 65535,
            "temperature": 28,
            "humidity": 53
          },
          "version": "1.0"
        }
        ```

4.  确认脚本能正确解析数据后，单击**提交**，将脚本提交到物联网平台系统。

    **说明：** 物联网平台不能调用草稿状态的脚本，只有已提交的脚本才会被调用来解析数据。


[设备接入物联网平台](/cn.zh-CN/最佳实践/设备接入/LoRa设备接入物联网平台/设备接入物联网平台.md)

