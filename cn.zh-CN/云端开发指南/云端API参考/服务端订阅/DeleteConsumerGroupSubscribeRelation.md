# DeleteConsumerGroupSubscribeRelation

调用该接口从AMQP订阅中的多个消费组移除指定消费组。

## 限制说明

-   当AMQP订阅中只有一个消费组时，不能使用该接口移除消费组。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=DeleteConsumerGroupSubscribeRelation&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteConsumerGroupSubscribeRelation|系统规定参数。取值：DeleteConsumerGroupSubscribeRelation。 |
|ConsumerGroupId|String|是|nJRaJPn5U1JITGfjBO9l00\*\*\*\*|消费组ID。您可以调用[QuerySubscribeRelation](~~170352~~)查询AMQP订阅中的消费组ID，也可以在物联网平台控制台对应实例下，选择**规则引擎**\>**服务端订阅**，查看AMQP订阅中的消费组ID。 |
|ProductKey|String|是|a1fyXVF\*\*\*\*|该订阅中的产品的ProductKey。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见 [公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|系统异常|调用失败时返回的出错信息。 |
|RequestId|String|E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功：

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteConsumerGroupSubscribeRelation
&ProductKey=a1Zkii7****
&ConsumerGroupId=nJRaJPn5U1JITGfjBO9l00****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DeleteConsumerGroupSubscribeRelationResponse>
       <RequestId>73B9DF43-7780-47DE-8BED-077729D28BD2</RequestId>
       <Success>true</Success>
</DeleteConsumerGroupSubscribeRelationResponse>
```

`JSON` 格式

```
{
  "RequestId": "73B9DF43-7780-47DE-8BED-077729D28BD2",
  "Success": true
}
```

