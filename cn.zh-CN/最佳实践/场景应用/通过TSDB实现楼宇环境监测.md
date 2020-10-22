---
keyword: [物联网, IoT, 物联网平台, 规则引擎, 数据流转, 时序数据库]
---

# 通过TSDB实现楼宇环境监测

本章以采集楼层中传感器数据为例，介绍将数据转发至时序时空数据库（TSDB）的数据流转规则设置。

在控制台创建传感器产品和设备，并将设备连接到物联网平台。具体请参见[快速入门](/cn.zh-CN/入门教程/快速玩转物联网平台/创建产品与设备.md)。

**说明：** 本示例未使用物模型，设备使用自定义Topic上报数据。

在No-1大厦的两个楼层中（例如，F1和F2层），每层分布2个传感器来记录该楼层的温度、湿度、PM2.5、甲醛含量等环境信息。

传感器每5秒采集一次环境数据并上报至物联网平台，物联网平台通过设置好的数据流转规则将环境数据转发到TSDB。您可以利用TSDB的空间聚合和降采样能力轻松实现数据统计与分析。

## 上报数据说明

-   数据上报频率：1次/5s。
-   数据上报自定义Topic：`/${productKey}/${deviceName}/user/data`。
-   payload格式：

    ```
    {"temperature":25,"humidity":24,"pm25":11,"hcho":0.02}
    ```


## 配置规则

配置规则引擎数据流转规则，将设备上报的数据转发至TSDB。

1.  登录[物联网平台控制台](https://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  选择**规则引擎** \> **云产品流转**，再单击**创建规则**，创建JSON数据格式规则。

4.  请参见[设置数据流转规则](/cn.zh-CN/消息通信/云产品流转/设置数据流转规则.md)，编写处理数据的SQL。本示例中的SQL如下：

    ```
    SELECT deviceName() as deviceName, timestamp() as time, attribute('floor') as floor, attribute('building') as building, temperature, humidity, pm25, hcho FROM "/${productKey}/+/user/data"
    ```

5.  单击**转发数据**一栏的**添加操作**，设置数据转发目的地。

    设置参数，将数据转发到一个TSDB的VPC实例中。

    ![设备数据流转](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7731649951/p13340.png)


## 查询时序数据

查询物联网平台发送到TSDB实例中的数据。

1.  登录[时序时空数据库控制台](https://tsdb.console.aliyun.com)。

2.  在实例列表中找到存储数据的VPC实例，并单击右侧**管理**。

3.  参见[时序洞察](https://help.aliyun.com/document_detail/89056.html)中的查询数据操作步骤，查询物联网平台发送到实例中的数据。

    -   按楼层聚合对比数据，根据如下表格设置查询参数。

        |参数|取值|
        |--|--|
        |空间聚合函数|选择avg。|
        |标签|设置为`building=No-1`。|
        |分组|选择floor。|

        数据查询结果如下图所示。

        ![设备数据流转](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7731649951/p13341.png)

    -   按大厦聚合avg，降采样1分钟查询数据，根据如下表格设置查询参数。

        |参数|取值|
        |--|--|
        |空间聚合函数|选择avg。|
        |标签|此处添加如下两个标签：         -   `building=No-1`
        -   `floor=f1/f2` |
        |降采样|打开开关。|
        |降采样聚合函数|选择max。|
        |采样时间间隔|选择1分钟。|

        数据查询结果如下图所示。

        ![设备数据流转](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7731649951/p13342.png)


