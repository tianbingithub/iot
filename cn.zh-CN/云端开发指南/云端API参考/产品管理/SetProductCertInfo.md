# SetProductCertInfo

调用该接口设置产品的X.509证书信息。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为30。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=SetProductCertInfo&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|SetProductCertInfo|系统规定参数。取值：SetProductCertInfo。 |
|IssueModel|Integer|是|1|证书颁发模式。

 -   **1**：由物联网平台颁发X.509证书。
-   **3**：由第三方平台颁发X.509证书。 |
|ProductKey|String|是|aS2dewS\*\*\*\*|产品的ProductKey。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|公共实例不传此参数；您购买的实例需传入实例ID。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|MissingProductKey|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|ProductKey is mandatory for this action.|调用失败时，返回的出错信息。 |
|RequestId|String|57b144cf-09fc-4916-a272-a62902d5b207|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。

 -   **true**：表示调用成功。
-   **false**：表示调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=SetProductCertInfo
&IssueModel=1
&ProductKey=aS2dewS****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<SetProductCertInfo>
  <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
  <Success>true</Success>
</SetProductCertInfo>
```

`JSON` 格式

```
{
    "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
    "Success": true
}
```

