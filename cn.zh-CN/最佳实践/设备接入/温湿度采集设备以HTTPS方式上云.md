# 温湿度采集设备以HTTPS方式上云

物联网平台华东2（上海）、华北2（北京）、华南1（深圳）地域支持设备使用HTTPS协议接入。设备与物联网平台通过HTTPS协议进行连接通信仅适用于单纯的设备上报数据场景。请求方式仅支持POST，且设备上报的数据不超过128 KB。

本实践案例以温湿度采集器为例，介绍设备通过HTTPS协议连接物联网平台并上报数据的配置和开发方法。

![iot](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6431649951/p71251.png)

## 创建产品和设备

在物联网平台控制台创建产品和设备，获取设备证书信息（ProdcutKey、DeviceName和DeviceSecret），并定义物模型。

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

4.  产品创建成功后，单击**前往定义物模型**。

5.  在产品详情页的功能定义页签下，选择**编辑草稿** \> **添加自定义功能**，添加以下属性。

    本示例中，温湿度采集器会上报温度和湿度，因此需为该产品定义对应的两个属性。

    |功能类型|功能名称|标识符|数据类型|取值范围|步长|读写类型|
    |----|----|---|----|----|--|----|
    |属性|温度|temperature|int32|-10~50|1|只读|
    |属性|湿度|humidity|int32|1~100|1|只读|

6.  物模型编辑完成后，单击**发布上线**，将物模型发布为正式版。

7.  在左侧导航栏，选择**设备**，单击**添加设备**，在刚创建的产品下添加设备。

    设备创建成功后，获取设备证书信息（ProductKey、DeviceName和DeviceSecret）。


## 开发设备端

开发设备端实现设备通过HTTPS协议连接物联网平台，并上报温湿度属性数据。

1.  配置设备身份认证。

    设备请求与物联网平台建立连接时，物联网平台会进行设备身份认证。认证通过后，下发设备token。设备token将在设备上报数据时使用。

    设备身份认证请求参数如下表。

    |参数|说明|
    |--|--|
    |method|请求方法。必须指定为POST。|
    |uri|指定为https://iot-as-http.cn-shanghai.aliyuncs.com/auth。|
    |productKey|设备所属产品的Key。可从物联网平台的控制台对应实例下的设备详情页获取。|
    |deviceName|设备名称。从物联网平台的控制台对应实例下的设备详情页获取。|
    |clientId|客户端ID。长度为64字符内，可使用设备的MAC地址或SN码。本示例中，使用函数random\(\)生成随机数。|
    |timestamp|时间戳。本示例中使用函数now\(\)获取当前时间戳。|
    |signmethod|算法类型，支持hmacmd5和hmacsha1。|
    |sign|签名，即计算出的password。password计算方法示例如下。     ```
password = signHmacSha1(params, deviceConfig.deviceSecret)
    ``` |

    设备身份认证示例代码如下。

    ```
    var rp = require('request-promise');
    const crypto = require('crypto');
    
    const deviceConfig = {
        productKey: "<yourProductKey>",
        deviceName: "<yourDeviceName>",
        deviceSecret: "<yourDeviceSecret>"
    }
    
    //获取身份token。
    rp(getAuthOptions(deviceConfig))
        .then(function(parsedBody) {
            console.log('Auth Info :',parsedBody)
        })
        .catch(function(err) {
            console.log('Auth err :'+JSON.stringify(err))
        });
    
    //生成Auth认证的参数。
    function getAuthOptions(deviceConfig) {
    
        const params = {
            productKey: deviceConfig.productKey,
            deviceName: deviceConfig.deviceName,
            timestamp: Date.now(),
            clientId: Math.random().toString(36).substr(2),
        }
    
        //生成clientId、username和password。
        var password = signHmacSha1(params, deviceConfig.deviceSecret);
    
        var options = {
            method: 'POST',
            uri: 'https://iot-as-http.cn-shanghai.aliyuncs.com/auth',
            body: {
                "version": "default",
                "clientId": params.clientId,
                "signmethod": "hmacsha1",
                "sign": password,
                "productKey": deviceConfig.productKey,
                "deviceName": deviceConfig.deviceName,
                "timestamp": params.timestamp
            },
            json: true
        };
    
        return options;
    }
    
    //HmacSha1 sign
    function signHmacSha1(params, deviceSecret) {
    
        let keys = Object.keys(params).sort();
        // 按字典序排序。
        keys = keys.sort();
        const list = [];
        keys.map((key) => {
            list.push(`${key}${params[key]}`);
        });
        const contentStr = list.join('');
        return crypto.createHmac('sha1', deviceSecret).update(contentStr).digest('hex');
    }
    ```

    配置完成后，可运行以上程序代码，进行设备认证测试。认证成功，则获得token。

    ![iot](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6431649951/p71260.png)

    **说明：** 设备认证返回的token会在一定周期后失效（目前token有效期是7天），请务必考虑token失效逻辑的处理。

2.  配置设备上报数据。

    认证通过，设备获得token后，便可使用token作为上报数据的password。

    设备上报数据的请求参数如下表。

    |参数|说明|
    |--|--|
    |method|请求方法。必须指定为POST。|
    |uri|endpoint地址和Topic组成uri：`https://iot-as-http.cn-shanghai.aliyuncs.com/topic + topic`。 后一个topic需指定为设备上报属性的Topic：

    ```
/sys/${deviceConfig.productKey}/${deviceConfig.deviceName}/thing/event/property/post
    ``` |
    |body|设备上报的消息内容。|
    |password|指定为设备认证返回的token。|
    |Content-Type|设备上报的数据的编码格式。目前仅支持：application/octet-stream。|

    设备上报数据示例代码如下。

    ```
    const topic = `/sys/${deviceConfig.productKey}/${deviceConfig.deviceName}/thing/event/property/post`;
    //上报数据。
    pubData(topic, token, getPostData())
    
    function pubData(topic, token, data) {
    
        const options = {
            method: 'POST',
            uri: 'https://iot-as-http.cn-shanghai.aliyuncs.com/topic' + topic,
            body: data,
            headers: {
                password: token,
                'Content-Type': 'application/octet-stream'
            }
        }
    
        rp(options)
            .then(function(parsedBody) {
                console.log('publish success :' + parsedBody)
            })
            .catch(function(err) {
                console.log('publish err ' + JSON.stringify(err))
            });
    
    }
    //模拟物模型数据。
    function getPostData() {
        var payloadJson = {
            id: Date.now(),
            params: {
                humidity: Math.floor((Math.random() * 20) + 60),
                temperature: Math.floor((Math.random() * 20) + 10)
            },
            method: "thing.event.property.post"
        }
    
        console.log("===postData\n topic=" + topic)
        console.log(payloadJson)
    
        return JSON.stringify(payloadJson);
    }
    ```

    配置完成后，可运行以上代码程序，进行设备上报数据测试。运行程序后，可在本地日志中查看运行结果。

    ![iot](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6431649951/p71261.png)

    在物联网平台控制台上，在对应实例下，该设备的设备详情页运行状态页签下，可查看设备上报的温湿度属性数据。说明设备端已通过HTTPS协议成功接入物联网平台，并上报了数据。


使用HTTPS连接通信的更多说明，请参见[HTTP连接通信](/cn.zh-CN/设备接入/使用开放协议自主接入/HTTP协议接入/HTTP连接通信.md)。

