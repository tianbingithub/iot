---
keyword: [Alink 协议, 物联网, IoT, 物联网平台, OTA, 固件升级, 升级包, 响应Topic, 请求Topic, 消息结构, 数据结构]
---

# OTA升级

物联网平台提供OTA升级与管理服务。下面介绍OTA升级消息的Topic和Alink数据格式，包括设备上报OTA模块版本、物联网平台推送升级包信息、设备上报升级进度和设备请求获取最新升级包信息。

关于OTA升级开发设置流程，请参见[设备端OTA升级](/cn.zh-CN/监控运维/OTA升级/设备端OTA升级.md)和[推送升级包到设备端](/cn.zh-CN/监控运维/OTA升级/推送升级包到设备端.md)。

## 设备上报OTA模块版本

数据上行。

Topic：`/ota/device/inform/${YourProductKey}/${YourDeviceName}`。

设备通过这个Topic上报当前的OTA模块版本信息。

**说明：** 本Topic只支持单个模块的版本上报。如果设备需要上报多个模块的版本，请分多次上报，每次上报一个模块的版本信息。

Alink请求数据格式：

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

## 物联网平台推送升级包信息

数据下行。

Topic：`/ota/device/upgrade/${YourProductKey}/${YourDeviceName}`。

物联网平台通过这个Topic推送升级包信息， 设备订阅该Topic可以获得升级包信息。

Alink请求数据格式：

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
|signMethod|String|签名方法。取值： -   SHA256
-   Md5

对于Android差分升级包类型，仅支持Md5签名方法。|
|md5|String|当签名方法为Md5时，除了会给sign赋值外还会给md5赋值。|
|module|String|升级包所属的模块名。 **说明：** 模块名为default时，云端不下发module参数。 |
|extData|Object|升级批次标签列表。单个标签格式：`"key":"value"`。 |

## 设备上报升级进度

数据上行。

Topic：`/ota/device/progress/${YourProductKey}/${YourDeviceName}`。

OTA升级过程中，设备可以通过这个Topic上报OTA升级的进度百分比。

Alink请求数据格式：

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

## 设备请求升级包信息

数据上行。

请求Topic：`/sys/{productKey}/{deviceName}/thing/ota/firmware/get`。

响应Topic：`/sys/{productKey}/{deviceName}/thing/ota/firmware/get_reply`。

Alink请求数据格式：

```
{
  "id": "123",
  "version": "1.0",
  "params": {
      "module": "MCU"
  },
  "method": "thing.ota.firmware.get"
}
```

|参数|类型|说明|
|:-|:-|:-|
|id|String|消息ID号。String类型的数字，取值范围0~4294967295，且每个消息ID在当前设备中具有唯一性。|
|version|String|Alink协议版本，目前协议版本号唯一取值为1.0。|
|params|String|请求参数。|
|module|String|升级包所属的模块名。 **说明：** 不指定则表示请求默认（default）模块的升级包信息。 |
|method|String|请求方法，取值thing.ota.firmware.get。|

物联网平台收到设备请求后，响应请求。

-   下发升级包信息。返回数据格式如下：

    ```
    {
      "id": "123",
      "code": 200,
      "data": {
        "size": 93796291,
        "sign": "f8d85b250d4d787a9f483d89a974****",
        "version": "1.0.1.9.20171112.1432",
        "isDiff": 1,
        "url": "https://the_firmware_url",
        "signMethod": "Md5",
        "md5": "f8d85b250d4d787a9f483d89a9747348",
        "module": "MCU",
        "extData":{
            "key1":"value1",
            "key2":"value2"
         }
      }
    }
    ```

    |参数|类型|说明|
    |:-|:-|:-|
    |id|String|消息ID号。String类型的数字，取值范围0~4294967295，且每个消息ID在当前设备中具有唯一性。|
    |code|Integer|状态码，200表示成功。|
    |version|String|设备升级包的版本信息。|
    |isDiff|Long|当升级包类型为差分时，消息包含isDiff参数，取值为1，表示升级包文件为差分包，仅包含新版本升级包与之前版本的差异部分，需要设备进行差分还原；当升级包类型为整包时，不含此参数。|
    |size|Long|升级包大小，单位：字节。|
    |url|String|升级包在对象存储（OSS）上的存储地址。|
    |sign|String|升级包签名。|
    |signMethod|String|签名方法。取值：     -   SHA256
    -   Md5
对于Android差分升级包类型，仅支持Md5签名方法。|
    |md5|String|当签名方法为Md5时，除了会给sign赋值外还会给md5赋值。|
    |module|String|升级包所属的模块名。 **说明：** 模块名为default时，云端不下发module参数。 |
    |extData|Object|升级批次标签列表。单个标签格式：`"key":"value"`。 |

-   无升级包信息下发。返回数据格式如下：

    ```
    {
      "id": "123",
      "code": 200,
      "data": {
      }
    }
    ```


