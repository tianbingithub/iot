# CreateOTAFirmware

将升级包文件上传到对象存储（OSS）后，调用该接口创建升级包。

## 限制说明

-   单个阿里云账号下最多可有500个升级包。
-   在调用此接口创建升级包前，已调用[GenerateOTAUploadURL](~~147310~~)生成了升级包上传信息，并已调用OSS [PostObject](~~31988~~)接口上传了升级包文件。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=CreateOTAFirmware&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateOTAFirmware|系统规定参数。取值：CreateOTAFirmware。 |
|DestVersion|String|是|2.0.0|当前升级包的版本号，仅支持英文字母、数字、点号（.）、短划线（-）和下划线（\_）。长度限制为1~64个字符。 |
|FirmwareName|String|是|Firmware2|升级包名称。支持中文、英文字母、日文、数字、短划线（-）、下划线（\_）和圆括号\(\)，必须以中文、英文、日文或数字开头，长度限制为1~40个字符。 |
|FirmwareUrl|String|是|https://iotx-ota.oss-cn-shanghai.aliyuncs.com/ota/bcd6142594d0183a16d825ad8225\*\*\*\*/A6B3400B70CA4D6D872160D1A91A\*\*\*\*.bin|升级包的URL，即升级包文件在对象存储（OSS）上的存储地址。调用[GenerateOTAUploadURL](~~147310~~)生成升级包上传信息，返回的参数。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|FirmwareSign|String|否|93230c3bde425a9d7984a594ac55\*\*\*\*|升级包签名值。使用**SignMethod**对升级包文件内容加签计算得出的值。

 不传入此参数，则采用对象存储（OSS）中升级包文件的MD5值作为升级包签名值。 |
|SignMethod|String|否|MD5|升级包签名方法。目前仅支持取值为**MD5**：MD5签名。

 不传入此参数，默认为**MD5**。 |
|FirmwareSize|Integer|否|900|升级包大小，单位：字节。

 不传入此参数，则采用对象存储（OSS）中升级包文件的大小作为升级包大小。 |
|ProductKey|String|否|a1uctKe\*\*\*\*|升级包所属产品的ProductKey。 |
|FirmwareDesc|String|否|OTA function updated|升级包描述。长度不可超过100个字符。一个中文汉字算一个字符。 |
|Type|Integer|否|0|升级包类型。可选：

 -   **0**：整包升级包，您上传的升级包文件包含完整的升级包，将推送整包升级包给设备进行升级。
-   **1**：差分升级包，您上传的升级包文件仅包含新版本升级包与之前版本的差异部分，仅推送差异部分给设备进行升级。

 不传入此参数，则默认值为**0**：整包升级包。 |
|SrcVersion|String|否|1.0.0|待升级OTA模块版本号，即待升级设备的当前OTA模块版本号。

 可以调用[QueryDeviceDetail](~~69594~~)，查看设备OTA模块版本号（**FirmwareVersion**）。

 **说明：**

-   **Type**为**1**（差分升级包）时，必须传入该参数，且取值不能与当前升级包版本（**DestVersion**）相同。
-   **Type**为**0**（整包升级包）时，可不传入该参数。 |
|ModuleName|String|否|WifiConfigModify|OTA模块名称。OTA模块是同产品下设备的不同可升级模块。

 **说明：**

-   不传入该参数，则使用default模块，表示整个设备的固件。
-   可调用[CreateOTAModule](~~186066~~)创建自定义OTA模块，调用[ListOTAModuleByProduct](~~186532~~)查询产品下已创建的OTA模块。 |
|NeedToVerify|Boolean|否|true|是否需要在创建批量升级任务前通过升级包验证。

 -   true：需要
-   false：不需要

 默认为true。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Struct| |调用成功时，返回的升级包信息。详情见以下Data。 |
|FirmwareId|String|s8SSHiKjpBfrM3BSN0z803\*\*\*\*|升级包ID，物联网平台为升级包颁发的唯一标识符。 |
|UtcCreate|String|2019-11-04T06:21:54.607Z|升级包的创建时间，UTC格式。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|291438BA-6E10-4C4C-B761-243B9A0D324F|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateOTAFirmware
&ProductKey=a1uctKe****
&FirmwareName=Firmware2
&DestVersion=2.0.0
&FirmwareUrl=https%3A%2F%2iotx-ota.oss-cn-shanghai.aliyuncs.com%2Fota%2F****%2F****.bin
&SignMethod=MD5
&FirmwareSign=93230c3bde425a9d7984a594ac55****
&FirmwareSize=900
&FirmwareDesc=OTA function updated
&Type=0
&ModuleName=WifiConfigModify
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<CreateOTAFirmwareResponse>
    <Data>
        <FirmwareId>s8SSHiKjpBfrM3BSN0z803****</FirmwareId>
        <UtcCreate>2019-11-04T06:21:54.607Z</UtcCreate>
    </Data>
    <RequestId>E4BD5A12-7C1D-4712-A7D5-B2432331165E</RequestId>
    <Success>true</Success>
</CreateOTAFirmwareResponse>
```

`JSON` 格式

```
{  
  "Data": {
    "FirmwareId": "s8SSHiKjpBfrM3BSN0z803****",
    "UtcCreate": "2019-11-04T06:21:54.607Z"
  },
  "RequestId": "291438BA-6E10-4C4C-B761-243B9A0D324F",
  "Success": true
}
```

