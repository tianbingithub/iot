# 配置LoRa设备接入物联网平台

配置LoRa网关后，您需要在物联网平台上创建LoRa产品和设备，定义物模型，编写、提交LoRa设备的数据解析脚本。

## 创建产品和设备

1.  登录[物联网平台控制台](https://iot.console.aliyun.com)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在左侧导航栏，选择**设备管理** \> **产品**。

4.  在产品页，单击**创建产品**，创建一个连网方式为**LoRaWAN**的产品。

    |参数|说明|
    |--|--|
    |产品名称|自定义产品名称。|
    |所属品类|选择为**自定义品类**。|
    |节点类型|选择**直连设备**。|
    |连网方式|选择为**LoRaWAN**。|
    |入网凭证|选择您在物联网络平台中创建并已授权的入网凭证。|
    |数据格式|选择为**透传/自定义**。|
    |认证方式|选择为**设备密钥**。|

5.  产品创建成功后，单击添加设备栏下的**前往添加**，添加一个设备。

    设备的DevEUI和PIN Code，请在您的设备标签上查看。

6.  测试设备连接物联网平台。

    按照设备上的标识，为设备连接天线、GPS天线、电池或电源。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8831649951/p68732.png)

    设备上电约2分钟后，在物联网平台控制台对应的实例下的设备页的设备列表中，该设备的状态会显示为**在线**。


## 定义物模型

物模型是将物理空间中的实体进行数字化，并在云端构建该实体的数据模型。在物联网平台中，定义物模型即定义产品功能（包括属性、事件、服务）。完成功能定义后，系统将自动生成该产品的物模型。本示例中，气象监测设备上报温度、湿度、气压、地理位置坐标等信息。因此，先在物联网平台上，为这些信息定义数据模型，即定义对应的属性。

1.  在物联网平台控制台对应实例下的左侧导航栏，选择**设备管理** \> **产品** 。

2.  在产品页，找到之前创建的产品，单击对应的**查看**。

3.  在产品详情页功能定义页签下，选择**编辑草稿** \> **添加自定义功能**，添加以下自定义功能。

    |属性名|标识符|类型|取值范围|步长|单位|读写类型|
    |---|---|--|----|--|--|----|
    |温度|Temperature|double|-99~100|0.01|℃|读写|
    |湿度|Humidity|double|1~100|0.01|%|读写|
    |大气压|Atmosphere|float|550 ~1060|0.01|hPa|读写|
    |经度|Longitude|double|-180~180|0.01|°|读写|
    |纬度|Latitude|double|-90~90|0.01|°|读写|
    |海拔|Altitude|float|0~9999|0.01|m|读写|
    |X加速度|Acceleration\_X|float|-1000~1000|0.01|mg|读写|
    |Y加速度|Acceleration\_Y|float|-1000~1000|0.01|mg|读写|
    |Z加速度|Acceleration\_Z|float|-1000~1000|0.01|mg|读写|
    |运行速度|Speed|float|-10000 ~10000|0.01|Km/h|读写|
    |电池电压|Battery\_voltage|float|0~100000|0.01|V|读写|
    |气体阻力|Gas\_resistance|float|-10000 ~10000|0.01|无|读写|

    新增物模型的详细操作说明，请参见[单个添加物模型](/cn.zh-CN/设备管理/物模型/单个添加物模型.md)。

4.  单击**发布上线**将物模型发布为正式版。


## 编写数据解析脚本

本示例中，LoRa设备上报的数据是二进制格式，如`01880537A5109D5A00846C`。其中 1、2 字节为数据标识码`01 88`；3、4、5字节为海拔数据`altitude:339m`；6、7、8字节为纬度数据`latitude:34.1925`；9、10、11字节为经度数据`longitude:108.8858`。

阿里云物联网平台的标准数据格式为Alink JSON格式，不能直接使用二进制数据进行业务处理；并且物联网平台下发的数据也是Alink JSON格式。您需要根据您的设备数据格式和定义的物模型，编写数据解析脚本，提交到物联网平台，以供物联网平台调用来解析上下行数据。

1.  登录[物联网平台控制台](https://iot.console.aliyun.com)，在对应实例的产品详情页，选择**数据解析**页签。

2.  在**编辑脚本**输入框中，输入解析脚本。

    **说明：** 脚本代码中属性的标识符必须与定义物模型时定义的一致。

    详细的数据解析脚本编写指导，请参见[LoRaWAN设备数据解析](/cn.zh-CN/设备管理/数据解析/LoRaWAN设备数据解析.md)。

    本示例的数据解析脚本如下：

    ```
    // var COMMAND_REPORT = 02;
    // var COMMAND_SET = 01;
    var ALINK_PROP_REPORT_METHOD = 'thing.event.property.post'; //标准ALink JSON格式Topic，设备上传属性数据到云端。
    var ALINK_PROP_SET_METHOD = 'thing.service.property.set';
    var ALINK_VERSION = "1.1";
    function rawDataToProtocol(bytes) {
        var uint8Array = new Uint8Array(bytes.length);
        for (var i = 0; i < bytes.length; i++) {
            uint8Array[i] = bytes[i] & 0xff;
        }
    
        var dataView = new DataView(uint8Array.buffer, 0);
        var jsonMap = {};
        // var fHead = uint8Array[0]; // 第0个BYTE为上报协议。// if (fHead == COMMAND_REPORT)
        {
            jsonMap['method'] = ALINK_PROP_REPORT_METHOD; //ALink JSON格式 - 属性上报。
            jsonMap['version'] = ALINK_VERSION; //ALink JSON格式 - 协议版本号固定字段。
            jsonMap['id'] = '' + 12345; //ALink JSON格式 - 标示该次请求id值。
            var params = {};
            switch (dataView.getInt16(0)) {
            case 0x0267:
                params['Temperature'] = Math.floor(dataView.getInt16(2) * 0.1 * 10) / 10;//保留两位小数。
                params['Humidity'] = Math.floor(100 * dataView.getUint8(6) * 0.01 / 2 * 10) / 10;
                params['Atmosphere'] = Math.floor(dataView.getInt16(9) * 0.1 * 10) / 10;
                break;
            case 0x0188:
                var buffer = new Uint8Array(4);
                buffer[0] = 0;
                buffer[1] = uint8Array[2];
                buffer[2] = uint8Array[3];
                buffer[3] = uint8Array[4];
                var latitude = new DataView(buffer.buffer, 0);
                params['Latitude'] = Math.floor(latitude.getInt32(0) * 0.0001 * 10000) / 10000;
                buffer[0] = 0;
                buffer[1] = uint8Array[5];
                buffer[2] = uint8Array[6];
                buffer[3] = uint8Array[7];
                var longitude = new DataView(buffer.buffer, 0);
                params['Longitude'] = Math.floor(longitude.getInt32(0) * 0.0001 * 10000) / 10000;
                buffer[0] = 0;
                buffer[1] = uint8Array[8];
                buffer[2] = uint8Array[9];
                buffer[3] = uint8Array[10];
                var altitude = new DataView(buffer.buffer, 0);
                params['Altitude'] = Math.floor(altitude.getInt32(0) * 0.01 * 100) / 100;
                break;
            case 0x0371:
                params['Acceleration_X'] = dataView.getInt16(2);
                params['Acceleration_Y'] = dataView.getInt16(4);
                params['Acceleration_Z'] = dataView.getInt16(6);
                break;
            case 0x0702:
                params['Battery_voltage'] = dataView.getInt16(2)/10;
                params['Speed'] = Math.floor(dataView.getInt16(6) * 0.01 * 100) / 100;
                break;
            case 0x0902:
                params['Gas_resistance'] = dataView.getInt16(2);
                break;
            }
            jsonMap['params'] = params; //ALink JSON 格式 - params 标准字段 }
            return jsonMap;
        }
        function protocolToRawData(bytes) {
            var method = json['method'];
            var id = json['id'];
            var version = json['version'];
            var payloadArray = [];
            return payloadArray;
        }
    }
                            
    ```

3.  测试脚本。

    1.  选择模拟类型为**设备上报数据**。

    2.  在模拟输入下的输入框中，输入一个模拟数据：`01880537A5109D5A00846C`。

    3.  单击**执行**。

    解析结果显示在运行结果栏中。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8831649951/p68735.png)

4.  确认脚本能正确解析数据后，单击**提交**，将脚本提交到物联网平台系统。

    **说明：** 物联网平台不能调用草稿状态的脚本，只有已提交的脚本才会被调用来解析数据。

    设备上报的属性数据经脚本成功解析后，您可以在该设备的设备详情页**物模型数据** \> **运行状态**页签下，查看设备上报的属性数据。


