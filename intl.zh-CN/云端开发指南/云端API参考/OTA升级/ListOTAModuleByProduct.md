# ListOTAModuleByProduct

调用该接口查询产品下的OTA模块列表。

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=ListOTAModuleByProduct&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListOTAModuleByProduct|系统规定参数。取值：ListOTAModuleByProduct。 |
|ProductKey|String|是|a1uctKe\*\*\*\*|产品的ProductKey。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Array of OtaModuleDTO| |调用成功时，返回的OTA模块列表。 |
|AliasName|String|条码扫描仪|模块别名。 |
|Desc|String|这个模块对应于条码扫描仪的固件|模块描述。 |
|GmtCreate|String|Wed, 20-Feb-2019 02:16:09 GMT|模块创建时间，GMT格式。 |
|GmtModified|String|Wed, 20-Feb-2019 02:16:09 GMT|模块信息最后一次更新时间，GMT格式。 |
|ModuleName|String|barcodeScanner|模块名称。 |
|ProductKey|String|aluctKe\*\*\*\*|产品的ProductKey。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|74C2BB8D-1D6F-41F5-AE68-6B2310883F63|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListOTAModuleByProduct
&ProductKey=a1uctKe****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ListOTAModuleByProductResponse>
      <RequestId>74C2BB8D-1D6F-41F5-AE68-6B2310883F63</RequestId>
      <Data>
            <Desc>这个模块对应于条码扫描仪的固件</Desc>
            <GmtCreate>Wed, 20-Feb-2019 02:16:09 GMT</GmtCreate>
            <ModuleName>barcodeScanner</ModuleName>
            <AliasName>条码扫描仪</AliasName>
            <GmtModified>Wed, 20-Feb-2019 02:16:09 GMT</GmtModified>
            <ProductKey>a1uctKe****</ProductKey>
      </Data>
      <Success>true</Success>
</ListOTAModuleByProductResponse>
```

`JSON` 格式

```
{
    "Data": {
        "Desc": "这个模块对应于条码扫描仪的固件",
        "GmtCreate": "Wed, 20-Feb-2019 02:16:09 GMT",
        "ModuleName": "barcodeScanner",
        "AliasName": "条码扫描仪",
        "GmtModified": "Wed, 20-Feb-2019 02:16:09 GMT",
        "ProductKey": "a1uctKe****"
    },
    "RequestId": "74C2BB8D-1D6F-41F5-AE68-6B2310883F63",
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/Iot)查看更多错误码。

