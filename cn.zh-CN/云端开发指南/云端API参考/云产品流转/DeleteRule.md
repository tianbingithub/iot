# DeleteRule

调用该接口删除指定的规则。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为50。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=DeleteRule&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteRule|系统规定参数。取值：DeleteRule。 |
|RuleId|Long|是|100000|要删除的规则ID。可在物联网平台控制台对应实例下，**规则引擎**\>**云产品流转**页查看，或调用[ListRule](~~69486~~)从返回结果中查看。 |
|IotInstanceId|String|否|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|公共实例不传此参数；您购买的实例需传入实例ID。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|A8F48485-44B9-40D8-A56D-F716F384F387|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteRule
&RuleId=100000
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DeleteRuleResponse>
      <RequestId>A8F48485-44B9-40D8-A56D-F716F384F387</RequestId>
      <Success>true</Success>
</DeleteRuleResponse>
```

`JSON` 格式

```
{
    "RequestId": "A8F48485-44B9-40D8-A56D-F716F384F387",
    "Success": true
}
```

