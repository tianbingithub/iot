# QueryConsumerGroupStatus

使用AMQP服务端订阅时，调用该接口查询某个消费组的状态，包括在线客户端信息、消息消费速率、消息堆积数、最近消息消费时间。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=QueryConsumerGroupStatus&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|QueryConsumerGroupStatus|系统规定参数。取值：QueryConsumerGroupStatus。 |
|GroupId|String|是|nJRaJPn5U1JITGf\*\*\*\*\*\*|消费组ID。调用[CreateConsumerGroup](~~170388~~)创建消费组成功后，会返回消费组ID。您可以调用[QueryConsumerGroupList](~~170419~~)按消费组名称查询消费组ID，也可以在物联网平台控制台对应实例下，选择**规则引擎**\>**服务端订阅**\>**消费组列表**，查看消费组ID。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见 [公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|AccumulationCount|Integer|13|消费组消息堆积数。 |
|ClientConnectionStatusList|Array of ConsumerGroupClientConnectionInfo| |消费组的在线客户端信息，请参见ConsumerGroupClientConnectionInfo。 |
|ConsumerGroupClientConnectionInfo| | | |
|ClientId|String|868575026\*\*\*\*\*\*|在线客户端ID。 |
|ClientIpPort|String|192.168.xxx.xxx:36918|在线客户端IP和端口。 |
|OnlineTime|Long|1591240546649|在线客户端的最后上线时间。取值为1970年01月01日00时00分00秒000毫秒以来的毫秒数。 |
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ConsumerSpeed|Integer|14|消费组消息消费速率，单位：条/分钟。 |
|ErrorMessage|String|系统异常|调用失败时返回的出错信息。 |
|LastConsumerTime|String|2020-05-29T03:37:56.000Z|最近消息消费时间。为UTC时间，以毫秒计，格式为“yyyy-MM-dd'T'HH:mm:ss.SSSZ”。 |
|RequestId|String|E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功：

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryConsumerGroupStatus
&GroupId=nJRaJPn5U1JITGf******
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<QueryConsumerGroupStatusResponse>
    <AccumulationCount>13</AccumulationCount>
    <ConsumerSpeed>14</ConsumerSpeed>
    <ClientConnectionStatusList>
        <ConsumerGroupClientConnectionInfo>
            <ClientId>868575026******</ClientId>
            <OnlineTime>1591240546649</OnlineTime>
            <ClientIpPort>192.168.xxx.xxx:36918</ClientIpPort>
        </ConsumerGroupClientConnectionInfo>
    </ClientConnectionStatusList>
    <RequestId>73B9DF43-7780-47DE-8BED-077729D28BD2</RequestId>
    <Success>true</Success>
</QueryConsumerGroupStatusResponse>
```

`JSON` 格式

```
{
  "AccumulationCount": 13,
  "ConsumerSpeed": 14,
  "ClientConnectionStatusList": {
        "ConsumerGroupClientConnectionInfo": [
            {
                "ClientId": "868575026******",
                "OnlineTime": 1591240546649,
                "ClientIpPort": "192.168.xxx.xxx:36918"
            }
        ]
    },
  "RequestId": "73B9DF43-7780-47DE-8BED-077729D28BD2",
  "Success": true
}
```

