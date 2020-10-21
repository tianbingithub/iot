---
keyword: [MQTT, 物联网, 通信协议, IoT, 物联网平台, X.509]
---

# 使用X.509证书认证

X.509证书是一种用于通信实体鉴别的数字证书。物联网平台支持基于MQTT协议直连的设备使用X.509证书进行认证。

## 限制说明

-   仅MQTT协议直连的设备可使用X.509证书认证。
-   目前仅华东2（上海）地域支持X.509证书认证。
-   连网方式为LoRaWAN的产品不支持X.509证书认证。
-   设备身份认证方式设置后，不可更改。

## 生成X.509证书

设备的X.509证书由物联网平台颁发。

1.  登录[物联网平台控制台](http://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在**设备管理** \> **产品**，创建认证方式为**X.509证书**的产品，详细操作请参见[创建产品](/cn.zh-CN/设备接入/创建产品.md)。

    ![iot x509](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4545559951/p112906.png)

4.  在**设备管理** \> **设备**，在新建的产品下创建设备，详细操作请参见[单个创建设备](/cn.zh-CN/设备接入/创建设备/单个创建设备.md)、[批量创建设备](/cn.zh-CN/设备接入/创建设备/批量创建设备.md)。

    物联网平台创建设备时，会为设备颁发X.509证书和密钥。

    **说明：** 对于使用X.509证书进行安全认证的设备，您需要在设备端烧录X.509证书。如果同时设备使用一型一密认证或子设备通过网关接入物联网平台功能，则您还需分别烧录ProductSecret或DeviceSecret。

5.  下载设备的X.509证书和密钥。

    -   下载批量创建设备的X.509证书和密钥。
        1.  在**设备管理** \> **设备** \> **批次管理**，单击设备批次对应的**下载CSV**按钮，下载设备证书信息表格。
        2.  根据下载表格中的CertUrl地址，下载设备X.509证书和密钥。

            **说明：** CertUrl地址有效期为30天。请在批量创建设备后的30天内下载X.509证书信息。

    -   下载单个设备的X.509证书和密钥。两种方式：
        -   在**设备管理** \> **设备**，单击设备对应的**查看**，进入设备详情页。单击**X.509证书**对应的**下载**按钮，下载证书信息。
        -   调用云端API [QueryDeviceCert](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceCert.md)获取证书信息。

## 设备端认证配置

目前仅C语言版的设备端Link SDK支持X.509认证方式。请访问[C SDK获取](https://help.aliyun.com/document_detail/96623.html)，下载开发代码Demo。

使用X.509证书的设备连接物联网平台的域名和端口如下：

-   连接域名：x509.itls.cn-shanghai.aliyuncs.com
-   端口：1883

以下以C语言版的设备端Link SDK为例。

1.  下载[根证书](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt)，用于双向认证时，校验服务端证书。

2.  配置设备认证的MQTT连接。

    如果您使用C语言Link SDK Demo，请在\\src\\mqtt\\examples目录下mqtt\_example.c文件中进行配置。

    -   将设备证书信息（ProductKey、DeviceName和DeviceSecret）设置为空字符串。

        使用X.509证书认证，建立认证MQTT连接时，不传入设备证书（ProductKey、DeviceName和DeviceSecret）。设备连接物联网平台成功后，由云端下发设备证书信息（ProductKey和DeviceName）给设备。

        示例：

        ```
        char g_product_key[IOTX_PRODUCT_KEY_LEN + 1]       = "";
        char g_device_name[IOTX_DEVICE_NAME_LEN + 1]       = "";
        char g_device_secret[IOTX_DEVICE_SECRET_LEN + 1]   = "";
        ```

    -   注册ITE\_IDENTITY\_RESPONSE事件，用于MQTT建连成功后，接收云端下发的ProductKey和DeviceName。

        示例：

        ```
        int identity_response_handle(const char *payload)
        {
            EXAMPLE_TRACE("identify: %s", payload);
        
            return 0;
        }
        
        IOT_RegisterCallback(ITE_IDENTITY_RESPONSE, identity_response_handle);
        ```

        **说明：** 您还可以配置设备端将接收到的ProductKey和DeviceName在进行固化。如果不做固化，设备每次连接物联网平台都会重新获取ProductKey和DeviceName。

3.  配置设备的X.509证书和其对应的密钥（PrivateKey），用于连接认证。

    如果您使用C语言Link SDK Demo，请在\\wrappers\\tls目录下的HAL\_TLS\_mbedtls.c文件中替换以下信息。

    -   将g\_cli\_crt取值替换为您的设备X.509证书。
    -   将g\_cli\_key取值替换为X.509证书的密钥。
    示例：

    ```
    const char *g_cli_crt = \
    {
        \
        "-----BEGIN CERTIFICATE-----\r\n"
        "Your X.509 certificate"
        "-----END CERTIFICATE-----\r\n"
    };
    
    const char *g_cli_key = \
    {
        \
        "-----BEGIN RSA PRIVATE KEY-----\r\n"
        "Your X.509 PrivateKey"
        "-----END RSA PRIVATE KEY-----\r\n"
    };
    ```


**说明：** 如果您不使用阿里云提供的设备端Link SDK，而是自己开发设备端，需：

-   将设备X.509证书和密钥配置到安全库中。
-   将设备证书信息（ProductKey、DeviceName和DeviceSecret）设置为空字符串，认证通过后由云端下发ProductKey和DeviceName。

    设备端从Topic `/ext/auth/identity/response`中获取云端下发的ProductKey和DeviceName。该Topic无需订阅。

    消息payload格式：

    ```
    {
        "productKey":"***",
        "deviceName":"***"
    }
    ```


## 设备再次连接

设备认证通过，获得ProductKey和DeviceName，并将ProductKey和DeviceName固化后，如果设备下线，再重新建立与物联网平台的MQTT连接时，传入的CONNECT报文参数：

|连接域名|`x509.itls.cn-shanghai.aliyuncs.com:1883`|
|可变报头（variable header）：Keep Alive|CONNECT指令中需包含Keep Alive（保活时间）。保活心跳时间取值范围为30秒至1,200秒。如果心跳时间不在此区间内，物联网平台会拒绝连接。建议取值300秒以上。如果网络不稳定，将心跳时间设置高一些。|
|MQTT的CONNECT报文参数|```
mqttClientId: clientId+"|securemode=2,signmethod=hmacsha1,timestamp=132323232|"
mqttUsername: deviceName+"&"+productKey
mqttPassword: ""
```

mqttClientId包含的参数说明如下。其中，`||`内为扩展参数。

-   clientId：表示客户端ID，建议使用设备的MAC地址或SN码，64字符内。
-   timestamp：表示当前时间毫秒值，可以不传递。
-   signmethod：表示签名算法类型。支持hmacmd5，hmacsha1和hmacsha256。
-   securemode：取值为2，表示TLS直连模式。

**说明：** 因为使用X.509数字证书认证，物联网平台不再校验签名值，mqttPassword设置为空。 |

