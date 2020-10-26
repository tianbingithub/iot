# DeleteOTAModule

调用该接口删除自定义OTA模块。

## 限制说明

-   默认（default）模块不允许删除。
-   若OTA模块下存在升级包，则不允许删除。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=DeleteOTAModule&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteOTAModule|系统规定参数。取值：DeleteOTAModule。 |
|ModuleName|String|是|barcodeScanner|要删除的OTA模块名称。 |
|ProductKey|String|是|a1uctKe\*\*\*\*|OTA模块所属产品的ProductKey。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

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
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteOTAModule
&ModuleName=barcodeScanner
&ProductKey=a1uctKe****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DeleteOTAModuleResponse>
       <RequestId>29EC7245-0FA4-4BB6-B4F5-5F04818FDFB1</RequestId>
       <Success>true</Success>
</DeleteOTAModuleResponse>
```

`JSON` 格式

```
{
   "RequestId": "9F41D14E-CB5F-4CCE-939C-057F39E688F5",
   "Success": true
}
```

