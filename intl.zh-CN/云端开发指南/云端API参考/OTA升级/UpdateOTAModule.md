# UpdateOTAModule

调用该接口修改OTA模块别名、描述。

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=UpdateOTAModule&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UpdateOTAModule|系统规定参数。取值：UpdateOTAModule。 |
|ModuleName|String|是|barcodeScanner|OTA模块名称。 |
|ProductKey|String|是|a1Le6d0\*\*\*\*|OTA模块所属产品的ProductKey。 |
|AliasName|String|否|条码扫描仪2|新的模块别名。支持英文字母、数字、点号（.）、短划线（-）和下划线（\_），长度限制为1~64个字符。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|Desc|String|否|条码扫描仪的固件模块|新的模块描述信息，支持最多100个字符。 |

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
https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateOTAModule
&ModuleName=barcodeScanner
&ProductKey=a1Le6d0****
&AliasName=条码扫描仪2
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<UpdateOTAModuleResponse>
     <RequestId>29EC7245-0FA4-4BB6-B4F5-5F04818FDFB1</RequestId>
     <Success>true</Success>
</UpdateOTAModuleResponse>
```

`JSON` 格式

```
{
  "RequestId": "9F41D14E-CB5F-4CCE-939C-057F39E688F5",
  "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/Iot)查看更多错误码。

