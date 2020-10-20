---
keyword: [设备, OTA, 物联网, IoT, 物联网平台, 空中下载技术, Over-the-Air Technology, 固件升级, Topic, 消息格式]
---

# 设备端OTA升级

OTA（Over-the-Air Technology）即空中下载技术。物联网平台支持通过OTA方式进行设备升级。本文以MQTT协议下的OTA升级为例，介绍OTA升级流程、数据流转使用的Topic和数据格式。

## OTA升级流程

MQTT协议下OTA升级流程如下图所示。

![流程](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0059232061/p172098.jpg)

**说明：**

-   升级前，设备端上报OTA模块当前版本为可选：

    -   整包升级前设备可以不上报。如不上报，配置批量升级时不能针对指定版本进行升级，详情请参见[推送升级包到设备端](/intl.zh-CN/监控运维/OTA升级/推送升级包到设备端.md)。
    -   差分升级前，设备必须上报。
    如设备需要在首次升级前上报版本号，建议只在系统启动过程中上报一次，不需要周期循环上报。

-   从物联网平台控制台发起批量升级后，设备升级操作记录状态是待升级。

    实际升级以物联网平台OTA系统接收到设备上报的升级进度开始。设备升级操作记录状态是升级中。

-   云端根据设备端上报的版本号来判断设备端OTA升级是否成功。
-   设备离线时，不能接收服务端推送的升级消息。

    通过MQTT协议接入物联网平台的设备再次上线后，物联网平台系统自动检测到设备上线，OTA服务端验证该设备是否需要升级。如果需要升级，再次推送升级消息给设备，否则不推送消息。


## 数据格式说明

设备端OTA开发流程和代码示例，请参见[Link SDK文档](https://www.alibabacloud.com/help/product/93051.htm)中，各语言SDK文档中的设备OTA开发章节。

OTA升级流程中使用的Topic和数据格式如下：

1.  （可选）设备连接OTA服务，上报版本号。

    设备端通过MQTT协议推送当前设备OTA模块版本号到Topic：`/ota/device/inform/${YourProductKey}/${YourDeviceName}`。消息格式如下：

    ```
    {
      "id": "123",
      "params": {
        "version": "1.0.1",
        "module": "MCU"
      }
    }
    ```

    |参数|类型|说明|
    |:-|:-|:-|
    |id|String|消息ID号。String类型的数字，取值范围0~4294967295，且每个消息ID在当前设备中具有唯一性。|
    |version|String|OTA模块版本。|
    |module|String|OTA模块名。 **说明：**

    -   上报默认（default）模块的版本号时，可以不上报module参数。
    -   设备的默认（default）模块的版本号代表整个设备的固件版本号。 |

2.  在物联网平台控制台上，添加升级包、验证升级包和发起批量升级。

    具体操作，请参见[推送升级包到设备端](/intl.zh-CN/监控运维/OTA升级/推送升级包到设备端.md)。

3.  您在控制台触发升级操作之后，设备会收到物联网平台OTA服务推送的升级包的URL地址。

    设备端订阅Topic：`/ota/device/upgrade/${YourProductKey}/${YourDeviceName}`。控制台对设备发起OTA升级请求后，设备端会通过该Topic收到升级包的存储地址URL。

    消息格式如下：

    ```
    {
      "code": "1000",
      "data": {
        "size": 432945,
        "version": "2.0.0",
        "isDiff": 1,
        "url": "https://iotx-ota-pre.oss-cn-shanghai.aliyuncs.com/nopoll_0.4.4.tar.gz?Expires=1502955804&OSSAccessKeyId=XXXXXXXXXXXXXXXXXXXX&Signature=XfgJu7P6DWWejstKJgXJEH0qAKU%3D&security-token=CAISuQJ1q6Ft5B2yfSjIpK6MGsyN1Jx5jo6mVnfBglIPTvlvt5D50Tz2IHtIf3NpAusdsv03nWxT7v4flqFyTINVAEvYZJOPKGrGR0DzDbDasumZsJbo4f%2FMQBqEaXPS2MvVfJ%2BzLrf0ceusbFbpjzJ6xaCAGxypQ12iN%2B%2Fr6%2F5gdc9FcQSkL0B8ZrFsKxBltdUROFbIKP%2BpKWSKuGfLC1dysQcO1wEP4K%2BkkMqH8Uic3h%2Boy%2BgJt8H2PpHhd9NhXuV2WMzn2%2FdtJOiTknxR7ARasaBqhelc4zqA%2FPPlWgAKvkXba7aIoo01fV4jN5JXQfAU8KLO8tRjofHWmojNzBJAAPpYSSy3Rvr7m5efQrrybY1lLO6iZy%2BVio2VSZDxshI5Z3McKARWct06MWV9ABA2TTXXOi40BOxuq%2B3JGoABXC54TOlo7%2F1wTLTsCUqzzeIiXVOK8CfNOkfTucMGHkeYeCdFkm%2FkADhXAnrnGf5a4FbmKMQph2cKsr8y8UfWLC6IzvJsClXTnbJBMeuWIqo5zIynS1pm7gf%2F9N3hVc6%2BEeIk0xfl2tycsUpbL2FoaGk6BAF8hWSWYUXsv59d5Uk%3D",
        "md5": "93230c3bde425a9d7984a594ac55ea1e",
        "sign": "93230c3bde425a9d7984a594ac55****",
        "signMethod": "Md5",
        "module": "MCU",
        "extData":{
            "key1":"value1",
            "key2":"value2"
         }
      },
      "id": "1507707025",
      "message": "success"
    }
    ```

    |参数|类型|说明|
    |:-|:-|:-|
    |id|String|消息ID号。String类型的数字，取值范围0~4294967295，且每个消息ID在当前设备中具有唯一性。|
    |message|String|结果信息。|
    |code|String|状态码。|
    |version|String|设备升级包的版本信息。|
    |size|Long|升级包大小，单位：字节。|
    |url|String|升级包在对象存储（OSS）上的存储地址。|
    |isDiff|Long|当升级包类型为差分时，消息包含isDiff参数，取值为1，表示升级包文件为差分包，仅包含新版本升级包与之前版本的差异部分，需要设备进行差分还原；当升级包类型为整包时，不含此参数。|
    |sign|String|升级包签名。|
    |signMethod|String|签名方法。取值：     -   SHA256
    -   Md5
对于Android差分升级包类型，仅支持Md5签名方法。|
    |md5|String|当签名方法为Md5时，除了会给sign赋值外还会给md5赋值。|
    |module|String|升级包所属的模块名。 **说明：** 模块名为default时，云端不下发module参数。 |
    |extData|Object|升级批次标签列表。单个标签格式：`"key":"value"`。 |

4.  设备收到URL之后，通过HTTPS协议根据URL下载升级包。

    **说明：** 设备需在升级包URL下发后的24小时内下载升级包，否则该URL失效。

5.  升级过程中，设备端向服务端推送升级进度到Topic：`/ota/device/progress/${YourProductKey}/${YourDeviceName}`。消息格式如下：

    ```
    {
      "id": "123",
      "params": {
        "step": "-1",
        "desc": "OTA升级失败，请求不到升级包信息。",
        "module": "MCU"
      }
    }
    ```

    |参数|类型|说明|
    |:-|:-|:-|
    |id|String|消息ID号。String类型的数字，取值范围0~4294967295，且每个消息ID在当前设备中具有唯一性。|
    |step|String|OTA升级进度信息。

取值范围：

    -   1~100的整数：升级进度百分比。
    -   -1：升级失败。
    -   -2：下载失败。
    -   -3：校验失败。
    -   -4：烧写失败。 |
    |desc|String|当前步骤的描述信息。如果发生异常，此字段可承载错误信息。|
    |module|String|升级包所属的模块名。 **说明：** 上报默认（default）模块的OTA升级进度时，可以不上报module参数。 |

6.  设备端完成OTA升级后，推送最新的版本信息到Topic：`/ota/device/inform/${YourProductKey}/${YourDeviceName}`。如果上报的版本与OTA服务要求的版本一致就认为升级成功，反之失败。

    **说明：** 升级成功的唯一判断标志是设备上报正确的版本号。即使升级进度上报为100%，如果不上报新的版本号，也视为升级失败。


## 常见下载升级包错误

-   签名错误。如果设备端获取的升级包的URL不全或者手动修改了URL内容，就会出现如下错误：

    ![脱敏处理](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0759549951/p94070.png)

-   拒绝访问。URL过期导致。目前，URL有效期为24小时。

    ![信息脱敏处理](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0759549951/p94092.png)


