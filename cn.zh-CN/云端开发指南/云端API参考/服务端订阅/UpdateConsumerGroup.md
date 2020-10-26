# UpdateConsumerGroup

使用AMQP服务端订阅时，可以调用该接口修改消费组名称。

## 限制说明

-   默认消费组无法修改。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=UpdateConsumerGroup&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UpdateConsumerGroup|系统规定参数。取值：UpdateConsumerGroup。 |
|GroupId|String|是|nJRaJPn5U1JITGf\*\*\*\*\*\*|消费组ID。调用[CreateConsumerGroup](~~170388~~)创建消费组成功后，会返回消费组ID。您可以调用[QueryConsumerGroupList](~~170419~~)按消费组名称查询消费组ID，也可以在物联网平台控制台对应实例下，选择**规则引擎**\>**服务端订阅**\>**消费组列表**，查看消费组ID。 |
|NewGroupName|String|是|消费组2|消费组的新名称。支持中文汉字、英文字母、数字和下划线（\_），长度为4~30个字符，一个汉字计为两个字符。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|73B9DF43-7780-47DE-8BED-077729D28BD2|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功：

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateConsumerGroup
&GroupId=nJRaJPn5U1JITGf******
&NewGroupName=消费组2
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<UpdateConsumerGroupNameResponse>
    <RequestId>73B9DF43-7780-47DE-8BED-077729D28BD2</RequestId>
    <Success>true</Success>
</UpdateConsumerGroupNameResponse>
```

`JSON` 格式

```
{
  "RequestId": "73B9DF43-7780-47DE-8BED-077729D28BD2",
  "Success": true
}
```

