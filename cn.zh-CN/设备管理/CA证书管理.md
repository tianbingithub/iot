---
keyword: [IoT, 物联网平台, CA证书]
---

# CA证书管理

物联网平台支持使用数字证书进行设备接入认证。使用数字证书，需先向证书颁发机构申请CA证书，然后在物联网平台注册CA证书，最后将数字设备证书与设备身份相绑定。下面介绍如何在物联网平台注册CA证书和绑定设备证书。

使用私有CA证书进行设备认证，需在创建产品时，选择**认证方式**为**X.509证书**，并在**使用私有CA证书**下选择**是**。

![创建产品](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0888649951/p162333.png)

## 限制说明

-   仅MQTT协议直连的设备可使用私有CA证书。
-   目前仅华东2（上海）地域支持使用私有CA证书。
-   连网方式为LoRaWAN的产品不支持使用私有CA证书。
-   使用私有CA证书时，只支持RSA算法签名的设备证书。
-   一个阿里云账号最多可注册10个CA证书。

## 注册CA证书

1.  登录[物联网平台控制台](http://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  在左侧导航栏，选择**设备管理** \> **CA证书**。

4.  在CA证书管理页，单击**注册CA证书**。

5.  在注册CA证书对话框中，输入证书名称，上传您的CA证书和验证证书，单击**确认**。

    ![CA证书](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0888649951/p162326.png)

    |字段|说明|
    |--|--|
    |CA证书名称|支持中文汉字、英文字母、数字和下划线（\_），长度限制4~30字符。|
    |上传CA证书|上传您的CA证书。 CA证书文件仅支持`.cer`、`.crt`和`.pem`格式。 |
    |上传验证证书|上传使用CA证书对应的私钥创建的验证证书，用来证明您拥有该CA证书。 验证证书文件仅支持`.cer`、`.crt`和`.pem`格式。

下面以使用OpenSSL为例，介绍创建验证证书的操作步骤：

    1.  生成私钥验证证书的密钥对。

生成密钥对的命令如下：

        ```
openssl genrsa -out verificationCert.key 2048
        ```

    2.  使用注册CA证书对话框上方的**注册码**创建CSR。

创建CSR的命令如下：

        ```
openssl req -new -key verificationCert.key -out verificationCert.csr
        ```

从注册CA证书对话框中复制注册码，并粘贴为`Common Name`字段值。

        ```
……
Common Name (e.g. server FQDN or YOUR name) []: c7cdfde6775c4b408b69d7e3c865f21bafe67937dc9a483ebbf7e6997b7b****
……
        ```

    3.  使用由CA证书私钥签名的CSR创建验证证书。

创建验证证书的命令如下：

        ```
openssl x509 -req -in verificationCert.csr -CA yourCA.cer -CAkey yourPrivateKey.key -CAcreateserial -out verificationCert.crt -days 300 -sha512
        ``` |

    证书注册成功后，显示在CA证书管理页的CA证书列表中。

    ![CA证书列表](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2888649951/p70650.png)

    单击证书对应的**查看**，进入CA证书详情页，可查看证书信息和绑定设备证书。


## 获取设备证书SN

您需要为每个设备签发对应的设备证书，获取设备证书SN。

**说明：**

-   设备证书SN必须在同一颁发者下具有唯一性。
-   证书签发后，请记录设备证书SN。将物联网平台设备与您的设备证书相绑定时，需使用该信息。

签发设备证书需遵循以下要求：

|设备证书|设备证书文件格式要求：

-   后缀名：`.cer`。
-   文件命格式：`[ProductKey]_[DeviceName]_[CA_SN].cer` 。
-   内容格式：

    ```
-----BEGIN CERTIFICATE-----
证书内容：ASCII字符串
-----END CERTIFICATE-----
    ``` |
|设备证书私钥|设备证书私钥格式要求：

-   后缀名：`.key`。
-   文件名格式：`[ProductKey]_[DeviceName]_[CA_SN].key`。
-   内容格式：

    ```
-----BEGIN RSA PRIVATE KEY-----
私钥内容：二进制字符串
-----END RSA PRIVATE KEY-----
    ``` |

## 绑定设备与设备证书SN

CA证书注册成功后，需在该CA证书下，将您已获得的设备证书SN和物联网平台上的设备身份信息（ProductKey和DeviceName）相绑定。

1.  在CA证书管理页，单击证书对应的**查看**。

2.  在CA证书详情页，单击**证书绑定** \> **添加绑定关系**。

    ![添加绑定关系](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2888649951/p140113.png)

3.  单击**下载.CSV模板**，下载设备证书信息填写模板。

4.  在模板文件中，填入您要绑定的设备证书信息，包括ProductKey、DeviceName和CertSN（即您的设备证书SN）。填写并保存文件后，单击**点击上传**上传文件。

    设备证书SN是一个16进制的字符串，是您签发设备证书后，获取并记录下来的。您也可以通过读取您的设备证书的SerialNumber域获取。

    **说明：**

    -   一个文件中最多可包含10,000条绑定记录。
    -   列表中的所有设备必须在同一个产品下。
5.  单击**保存**，完成文件上传。

    物联网平台系统会对上传的文件进行解析和验证。验证通过后，才能进行绑定。


## 配置设备接入物联网平台

开发使用第三方CA证书进行身份验证的设备端时，无需配置设备的ProductKey和DeviceName信息，只需配置CA证书主题和设备证书SN。设备上线时，物联网平台根据设备上报的CA证书主题和设备证书SN进行身份验证。身份验证通过，则向设备下发ProductKey和DeviceName。具体设备端配置方法，请参见[设备端认证配置](/cn.zh-CN/设备接入/设备安全认证/使用X.509证书认证.md)。

