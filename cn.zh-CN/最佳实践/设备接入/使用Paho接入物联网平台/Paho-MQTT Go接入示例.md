---
keyword: [IoT, 物联网平台, Paho, MQTT, Go]
---

# Paho-MQTT Go接入示例

本示例介绍如何调用Go语言的Paho MQTT类库连接阿里云物联网平台，并进行消息收发。

已在物联网平台中，创建了产品和设备。

请参见[创建产品](/cn.zh-CN/设备接入/创建产品.md)和[单个创建设备](/cn.zh-CN/设备接入/创建设备/单个创建设备.md)。

## 准备开发环境

安装Go语言包。

-   苹果电脑安装命令：

    ```
    brew install go
    ```

-   Ubuntu电脑安装命令：

    ```
    sudo apt-get install golang-go
    ```

-   Windows电脑请从[Golang官网](https://golang.google.cn/dl/)下载安装包安装。

    **说明：** 在中国内地境内，Golang官网需在VPN环境下访问。


## 下载Go语言Paho MQTT库

请访问[Eclipse Paho Downloads](https://www.eclipse.org/paho/downloads.php)了解Paho项目和支持的开发语言详情。

使用以下命令下载Go语言版本的Paho MQTT库和相关依赖的库：

```
go get github.com/eclipse/paho.mqtt.golang
go get github.com/gorilla/websocket
go get golang.org/x/net/proxy
```

## 接入物联网平台

1.  单击打开[MqttSign.go](http://code.aliyun.com/edward.yangx/public-docs/wikis/user-guide/linkkit/Paho_MQTT_Guide/MqttSign.go)，下载阿里云提供的计算MQTT连接参数所需的源码。

    MqttSign.go文件定义了用于计算设备接入物联网平台的MQTT连接参数的函数，您开发的设备端接入物联网平台程序需调用该函数，函数说明如下：

    -   原型：

        ```
        type AuthInfo struct {
            password, username, mqttClientId string;
        }
        
        func calculate_sign(clientId, productKey, deviceName, deviceSecret, timeStamp string) AuthInfo;
        ```

    -   功能：

        用于计算设备接入物联网平台的MQTT连接参数username、password和mqttClientId。

    -   输入参数：

        |参数|类型|说明|
        |--|--|--|
        |productKey|String|设备所属产品的ProductKey，该设备在物联网平台上的身份证书信息之一。|
        |deviceName|String|设备名称，该设备在物联网平台上的身份证书信息之一。|
        |deviceSecret|String|设备密钥，该设备在物联网平台上的身份证书信息之一。|
        |clientId|String|设备端ID。可以设置为64字符以内的字符串。建议设置为实际设备的SN码或MAC地址。|
        |timeStamp|String|当前时间的毫秒值时间戳。|

    -   输出参数：

        该函数的返回值为`AuthInfo`结构体，包含以下参数：

        |参数|类型|说明|
        |--|--|--|
        |username|String|MQTT连接所需的用户名。|
        |password|String|MQTT连接所需的密码。|
        |mqttClientId|String|MQTT客户端ID。|

2.  添加实现设备接入物联网平台的程序文件。

    您需编写程序调用MqttSign.go计算MQTT连接参数，实现接入物联网平台和通信。

    开发说明和代码示例如下：

    -   设置设备信息。

        ```
        // set the device info, include product key, device name, and device secret
            var productKey string = "a1Zd7n5***"
            var deviceName string = "testdevice"
            var deviceSecret string = "UrwclDV33NaFSmk0JaBxNTqgSrJW****"
        
            // set timestamp, clientid, subscribe topic and publish topic
            var timeStamp string = "1528018257135"
            var clientId string = "192.168.****"
            var subTopic string = "/" + productKey + "/" + deviceName + "/user/get";
            var pubTopic string = "/" + productKey + "/" + deviceName + "/user/update";
        ```

    -   设置MQTT连接信息。

        调用MqttSign.go中定义的calculate\_sign函数，根据传入的参数clientId、productKey、deviceName、deviceSecret和timeStamp计算出username、password和mqttClientId，并将这些信息都包含在opts中。

        ```
            // set the login broker url
            var raw_broker bytes.Buffer
            raw_broker.WriteString("tls://")
            raw_broker.WriteString(productKey)
            raw_broker.WriteString(".iot-as-mqtt.cn-shanghai.aliyuncs.com:1883")
            opts := MQTT.NewClientOptions().AddBroker(raw_broker.String());
        
            // calculate the login auth info, and set it into the connection options
            auth := calculate_sign(clientId, productKey, deviceName, deviceSecret, timeStamp)
            opts.SetClientID(auth.mqttClientId)
            opts.SetUsername(auth.username)
            opts.SetPassword(auth.password)
            opts.SetKeepAlive(60 * 2 * time.Second)
            opts.SetDefaultPublishHandler(f)
        ```

        **说明：** 代码中的地域代码（cn-shanghai），需设置为您的物联网平台设备所在地域代码。地域代码表达方法，请参见[地域和可用区]()。

    -   调用MQTT的Connect\(\)函数接入物联网平台。

        ```
            // create and start a client using the above ClientOptions
            c := MQTT.NewClient(opts)
            if token := c.Connect(); token.Wait() && token.Error() != nil {
                panic(token.Error())
            }
            fmt.Print("Connect aliyun IoT Cloud Sucess\n");
        ```

    -   调用Publish接口发布消息。需指定发布消息的目标Topic和消息payload。

        ```
            // publish 5 messages to pubTopic("/a1Zd7n5****/deng/user/update")
            for i := 0; i < 5; i++ {
                fmt.Println("publish msg:", i)
                text := fmt.Sprintf("ABC #%d", i)
                token := c.Publish(pubTopic, 0, false, text)
                fmt.Println("publish msg: ", text)
                token.Wait()
                time.Sleep(2 * time.Second)
            }
        ```

        通信Topic介绍，请参见[什么是Topic](/cn.zh-CN/设备接入/消息通信Topic/什么是Topic.md)。

    -   调用Subscribe接口订阅Topic，接收云端下发的消息。

        ```
            // subscribe to subTopic("/a1Zd7n5***/deng/user/get") and request messages to be delivered
            if token := c.Subscribe(subTopic, 0, nil); token.Wait() && token.Error() != nil {
                fmt.Println(token.Error())
                os.Exit(1)
            }
            fmt.Print("Subscribe topic " + subTopic + " success\n");
                                        
        ```

    关于设备、服务器和物联网平台的通信方式介绍，请参见[通信方式概述](/cn.zh-CN/消息通信/通信方式概述.md)。

3.  编译项目。


## 示例Demo

使用Demo代码程序接入物联网平台。

1.  [下载示例代码包](http://code.aliyun.com/edward.yangx/public-docs/wikis/user-guide/linkkit/Paho_MQTT_Guide/aiot-go-demo.zip)，并提取文件。

    aiot-go-demo中包含以下文件：

    |文件|说明|
    |--|--|
    |MqttSign.go|该文件包含以MQTT方式接入物联网平台的连接参数计算代码。iot.go运行时，会调用该文件中定义的calculate\_sign函数，计算出连接参数username、password和mqttClientId。|
    |iot.go|该文件包含设备与物联网平台连接和通信的逻辑代码。|
    |x509|物联网平台的根证书，是设备接入物联网平台的必须证书。|

2.  在iot.go中，修改设备信息为您的设备信息。

    可使用Linux vi等工具修改iot.go文件：

    -   将productKey、deviceName和deviceSecret替换为您的设备证书信息。
    -   （可选）替换timeStamp和clientId。clientId的值可以替换为您的实际设备的SN码和MAC地址。

        这两个参数值不替换也能接入物联网平台，但实际使用时，建议您替换为实际信息。

    -   将`raw_broker.WriteString(".iot-as-mqtt.cn-shanghai.aliyuncs.com:1883")`中的地域代码（cn-shanghai）替换为您的物联网平台设备所在地域代码。地域代码表达方法，请参见[地域和可用区]()。
3.  在命令行里使用以下命令运行iot.go：

    ```
    go run iot.go MqttSign.go 
    ```

    运行成功，接入物联网平台的本地日志如下：

    ```
    clientId192.168.****deviceNametestdeviceproductKeya1Zd7n5****timestamp1528018257135
    1b865320fc183cc747041c9faffc9055fc45****
    Connect aliyun IoT Cloud Sucess
    Subscribe topic /a1Zd7n5****/testdevice/user/get success
    publish msg: 0
    publish msg:  ABC #0
    publish msg: 1
    publish msg:  ABC #1
    publish msg: 2
    publish msg:  ABC #2
    publish msg: 3
    publish msg:  ABC #3
    publish msg: 4
    publish msg:  ABC #4
    publish msg: 5
    publish msg:  ABC #5
    ```

    登录[物联网平台控制台](http://iot.console.aliyun.com/)，在对应的实例下，可查看设备状态和日志。

    -   选择**设备管理** \> **设备**，可看到该设备的状态显示为**在线**。
    -   选择**监控运维** \> **日志服务**，可查看**云端运行日志**和**设备本地日志**日志。详情请参见[云端运行日志](/cn.zh-CN/监控运维/日志服务/云端运行日志.md)、[设备本地日志](/cn.zh-CN/监控运维/日志服务/设备本地日志.md)。

## 错误码

如果设备通过MQTT协议接入物联网平台失败，请根据错误码排查问题。服务端错误码说明，请参见[错误排查](/cn.zh-CN/最佳实践/设备接入/使用Paho接入物联网平台/错误排查.md)。

