# CreateOTAModule

调用该接口创建产品的OTA模块。

## 限制说明

OTA模块是同产品下设备的不同可升级模块。默认（default）模块表示整个设备的固件。您可以调用该接口创建自定义OTA模块。

-   同一产品下最多自定义10个OTA模块。
-   创建后，OTA模块名称不可更改，可调用[UpdateOTAModule](~~186061~~)修改模块别名、模块描述。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=CreateOTAModule&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateOTAModule|系统规定参数。取值：CreateOTAModule。 |
|ModuleName|String|是|barcodeScanner|OTA模块名称，产品下唯一且不可修改。仅支持英文字母、数字、点号（.）、短划线（-）和下划线（\_）。长度限制为1~64个字符。 |
|ProductKey|String|是|a1Le6d0\*\*\*\*|OTA模块所属产品的ProductKey。 |
|AliasName|String|否|条码扫描仪|OTA模块别名。支持英文字母、数字、点号（.）、短划线（-）和下划线（\_），长度限制为1~64个字符。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|Desc|String|否|条码扫描仪的固件模块|OTA模块的描述信息，支持最多100个字符。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|74C2BB8D-1D6F-41F5-AE68-6B2310883F63|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateOTAModule
&ModuleName=barcodeScanner
&ProductKey=a1Le6d0****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<CreateOTAModuleResponse>
     <RequestId>29EC7245-0FA4-4BB6-B4F5-5F04818FDFB1</RequestId>
     <Success>true</Success>
</CreateOTAModuleResponse>
```

`JSON` 格式

```
{
  "RequestId": "9F41D14E-CB5F-4CCE-939C-057F39E688F5",
  "Success": true
}
```

