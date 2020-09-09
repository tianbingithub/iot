# PubBroadcast

调用该接口向指定产品所有设备，或向订阅了指定Topic的所有设备发布广播消息。

## 限制说明

-   指定Topic订阅广播，单阿里云账号调用该接口的每秒请求数（QPS）最大限制为1。
-   全量在线设备广播，单阿里云账号调用该接口的每分钟内请求数（QPM）最大限制为1。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=PubBroadcast&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|PubBroadcast|系统规定参数。取值：PubBroadcast。 |
|MessageContent|String|是|aGVsbG93b3JsZA|要发送的消息主体，最大报文64 KB。

 您需要将消息原文转换成二进制数据，并进行Base64编码，从而生成消息主体。 |
|ProductKey|String|是|aldeji3\*\*\*\*\*|要发送广播消息的产品Key。 |
|IotInstanceId|String|否|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|公共实例不传此参数；您购买的实例需传入实例ID。 |
|TopicFullName|String|否|/broadcast/UPqSxj2vXXX/xxx|可选参数：

 -   不赋值，表示全量在线设备广播，推送消息到指定**ProductKey**的全量在线设备。设备端收到的广播Topic格式为`/sys/${productKey}/${deviceName}/broadcast/request/${MessageId}`，其中**MessageId**由物联网平台生成。
-   赋值，表示指定Topic订阅广播，推送消息到指定**ProductKey**的已订阅广播Topic的在线设备。传入要接收广播消息的Topic全称，格式为：`/broadcast/${productKey}/自定义字段`。其中，**$\{productKey\}**是要接收广播消息的具体产品**ProductKey**；自定义字段中您可以指定任意字段。

 **说明：**

-   广播Topic是在设备开发时编码定义的，无需控制台创建。
-   一个广播Topic最多可被1,000个设备订阅。如果您的设备超过数量限制，您可以对设备进行分组。例如，如果您有5,000个设备，您可以将设备按每组1,000个，而分成5组。您需要分5次调用广播Topic，自定义字段分别设置为group1/2/3/4/5，然后让每组设备分别订阅各自分组的广播Topic。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|MessageId|Long|1234291569964771840|成功发送消息后，云端生成的消息ID，用于标识该消息。 |
|RequestId|String|BB71E443-4447-4024-A000-EDE09922891E|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=PubBroadcast
&ProductKey=al**********
&TopicFullName=/broadcast/UPqSxj2vXXX/xxx
&MessageContent=aGVsbG93b3JsZA
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<PubBroadcastResponse>
        <RequestId>BB71E443-4447-4024-A000-EDE09922891E</RequestId>
        <MessageId>1234291569964771840</MessageId>
        <Success>true</Success>
  </PubBroadcastResponse>
```

`JSON` 格式

```
{
      "RequestId":"BB71E443-4447-4024-A000-EDE09922891E",
      "MessageId":1234291569964771840,
      "Success":true
}
```

