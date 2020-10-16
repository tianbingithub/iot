# UpdateRule

调用该接口修改指定的规则。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为50。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=UpdateRule&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UpdateRule|系统规定参数。取值：UpdateRule。 |
|RuleId|Long|是|100000|要修改的规则ID。可在物联网平台控制台**规则引擎**\>**云产品流转**页查看规则ID，或调用[ListRule](~~69486~~)从返回结果中查看。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|Select|String|否|deviceName\(\) as deviceName, items.Humidity.value as Humidity, items.Temperature.value as Temperature|要执行的SQL Select语句。具体内容参照[SQL表达式](~~30554~~)。

 **说明：** 此处传入的是**Select**下的内容。例如，如果**Select**语句为`Select a,b,c`，则此处传入`a,b,c`。 |
|ShortTopic|String|否|+/thing/event/property/post|应用该规则的具体Topic，格式一般为：`${deviceName}/topicShortName`。其中，`${deviceName}`是具体设备的名称，`topicShortName`是Topic短名称。

 -   基础通信Topic或物模型通信Topic的ShortTopic，格式一般为：`${deviceName}/topicShortName`。其中，`${deviceName}`可以使用通配符`+`代替，表示产品下所有设备名称。`topicShortName`取值如下：
    -   `/thing/event/property/post`设备上报的属性消息。
    -   `/thing/event/${tsl.event.identifier}/post`设备上报的事件消息，`${}`中是产品物模型中事件identifier。
    -   `/thing/lifecycle` 设备生命周期变更消息。
    -   `/thing/downlink/reply/message`设备响应云端指令的结果消息。
    -   `/thing/list/found`网关上报发现子设备消息。
    -   `/thing/topo/lifecycle`设备拓扑关系变更消息。
    -   `/thing/event/property/history/post`设备历史属性上报消息。
    -   `/thing/event/${tsl.event.identifier}/history/post`设备历史事件上报消息，`${}`中是产品物模型中事件identifier。
    -   `/ota/upgrade`设备OTA升级状态通知消息。
    -   `/ota/version/post`设备OTA模块版本号上报消息。
    -   `/thing/deviceinfo/update`设备标签变更消息。
    -   `/edge/driver/${driver_id}/point_post`物联网边缘计算的透传模式Topic消息，`${}`中是物联网边缘计算的设备接入驱动ID。

OTA升级批次状态通知Topic也属于基础通信Topic，ShortTopic格式为：`${packageId}/${jobId}/ota/job/status`。其中，`${packageId}`是升级包ID，`${jobId}`是升级批次ID。

-   自定义Topic的ShortTopic，如：`${deviceName}/user/get`。

调用[QueryProductTopic](~~69647~~)接口，可以查看产品下的所有自定义Topic类。

指定自定义Topic时，可以使用通配符`+`和`#`。

    -   `${deviceName}`可以使用通配符`+`代替，表示产品下所有设备；
    -   之后字段可以用`/user/#`，`#`表示`/user`层级之后的所有层级名称。

使用通配符，请参见[Topic类中的通配符](~~85539~~)。

-   设备状态变化通知Topic的ShortTopic：`${deviceName}`。

可以直接使用通配符`+`，表示产品下所有设备的状态变化通知。 |
|Where|String|否|Temperature\>35|规则的触发条件。具体内容参照[SQL表达式](~~30554~~)。

 **说明：** 此处传入的是**Where**中的内容。例如，如果**Where**语句为`Where a>10`，则此处传入`a>10`。 |
|ProductKey|String|否|aladaeW\*\*\*\*|应用该规则的产品ProductKey。 |
|Name|String|否|test\_2|规则名称。支持中文、英文字母、日文、数字、下划线（\_）和短划线（-），长度为1~30个字符，一个中文或日文占2个字符。 |
|RuleDesc|String|否|test|规则的描述信息。长度限制为100个字符，一个中文字符计为1个字符。 |
|TopicType|Integer|否|1|-   **0**：**ShortTopic**参数描述中的基础通信Topic或物模型通信Topic，包含OTA升级批次状态通知Topic。
-   **1**：自定义Topic。
-   **2**：设备状态变化通知Topic：`/as/mqtt/status/${productKey}/${deviceName}`。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|9A2F243E-17FE-4846-BAB5-D02A25155AC4|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateRule
&RuleId=100000
&Name=test_2
&ProductKey=aladaeW****
&ShortTopic=+/thing/event/property/post
&Select=deviceName() as deviceName, items.Humidity.value as Humidity, items.Temperature.value as Temperature
&RuleDesc=test
&Where=a>10
&TopicType=1
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<UpdateRuleResponse>
      <RequestId>9A2F243E-17FE-4874-QBB5-D02A25155AC8</RequestId>
      <Success>true</Success>
</UpdateRuleResponse>
```

`JSON` 格式

```
{
    "RequestId":"9A2F243E-17FE-4846-BAB5-D02A25155AC4",
    "Success":true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/Iot)查看更多错误码。

