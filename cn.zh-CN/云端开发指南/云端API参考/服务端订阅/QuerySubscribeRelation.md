# QuerySubscribeRelation

调用该接口查询MNS或AMQP服务端订阅。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=QuerySubscribeRelation&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|QuerySubscribeRelation|系统规定参数。取值：QuerySubscribeRelation。 |
|ProductKey|String|是|a1fyXVF\*\*\*\*|该订阅中的产品的ProductKey。 |
|Type|String|是|AMQP|订阅类型：

 -   **MNS**
-   **AMQP** |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见 [公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|DeviceDataFlag|Boolean|true|推送消息类型是否选择设备上报消息：

 -   **true**：是。
-   **false**：否。 |
|DeviceStatusChangeFlag|Boolean|true|推送消息类型是否选择设备状态变化通知：

 -   **true**：是。
-   **false**：否。 |
|FoundDeviceListFlag|Boolean|true|推送消息类型是否选择网关子设备发现上报：

 -   **true**：是。仅网关产品会返回**true**。
-   **false**：否。 |
|DeviceTopoLifeCycleFlag|Boolean|true|推送消息类型是否选择设备拓扑关系变更：

 -   **true**：是。仅网关产品会返回**true**。
-   **false**：否。 |
|DeviceLifeCycleFlag|Boolean|true|推送消息类型是否选择设备生命周期变更：

 -   **true**：是。
-   **false**：否。 |
|ThingHistoryFlag|Boolean|true|推送消息类型是否选择物模型历史数据上报：

 -   **true**：是。
-   **false**：否。 |
|OtaEventFlag|Boolean|true|推送消息类型是否选择固件升级状态通知：

 -   **true**：是。
-   **false**：否。 |
|DeviceTagFlag|Boolean|true|推送消息类型是否选择设备标签变更。可选值：

 -   **true**：是。仅当**Type**为**AMQP**时有效。
-   **false**：否。

 默认值为**false**。 |
|OtaVersionFlag|Boolean|true|推送消息类型是否选择OTA模块版本号上报。可选值：

 -   **true**：是。仅当**Type**为**AMQP**时有效。
-   **false**：否。

 默认值为**false**。 |
|OtaJobFlag|Boolean|true|推送消息类型是否选择OTA升级批次状态通知。可选值：

 -   **true**：是。仅当**Type**为**AMQP**时有效。
-   **false**：否。

 默认值为**false**。 |
|ProductKey|String|a1fyXVF\*\*\*\*|该订阅中的产品的ProductKey。 |
|Success|Boolean|true|是否调用成功：

 -   **true**：调用成功。
-   **false**：调用失败。 |
|ConsumerGroupIds|List|\[DEFAULT\_GROUP,br45A6A1amoRFGN7x1zP00\*\*\*\*\]|**Type**为**AMQP**时，返回AMQP订阅中的消费组ID。 |
|Type|String|AMQP|订阅类型：

 -   **MNS**
-   **AMQP** |
|RequestId|String|21D327AF-A7DE-4E59-B5D1-ACAC8C024555|阿里云为该请求生成的唯一标识符。 |
|MnsConfiguration|String|\{ "themeName": "mns-test-topic1", "regionName": "cn-shanghai", "role": \{ "roleArn": "acs:ram::5645\*\*\*:role/aliyuniotaccessingmnsrole", "roleName": "AliyunIOTAccessingMNSRole" \} \}|**Type**为**MNS**时，返回MNS队列的配置信息。

 具体组成和示例见下文返回参数补充说明。 |

**MnsConfiguration定义**

|名称

|描述 |
|----|----|
|themeName

|消息服务中用来接收信息的目标主题名称。 |
|regionName

|目标消息服务所在的阿里云地域代码，例如cn-shanghai。 |
|role

|授权角色信息。通过授予物联网平台指定的系统服务角色，您可以授权物联网平台访问您的消息服务。授权角色信息如下：

 `{"roleArn":"acs:ram::5645***:role/aliyuniotaccessingmnsrole","roleName": "AliyunIOTAccessingMNSRole"}`

 请将`5645***`替换成您的阿里云账号ID。您可以登录控制台，在账号安全设置页面查看您的账号ID。

 `AliyunIOTAccessingMNSRole`是访问控制中定义的服务角色。用于授予物联网平台访问消息服务。关于角色的更多信息，请在访问控制RAM控制台的角色管理页面进行角色管理。 |

**MnsConfiguration**示例：

```

{
    "themeName": "mns-test-topic1",
    "regionName": "cn-shanghai",
    "role": {
        "roleArn": "acs:ram::5645***:role/aliyuniotaccessingmnsrole",
        "roleName": "AliyunIOTAccessingMNSRole"
    }
}

```

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QuerySubscribeRelation
&ProductKey=a1Zkii7****
&Type=AMQP
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<QuerySubscribeRelationResponse>
       <ConsumerGroupIds>
              <e>Xs95KifeaSKbi8tKkcoD00****</e>
       </ConsumerGroupIds>
       <DeviceDataFlag>false</DeviceDataFlag>
       <DeviceLifeCycleFlag>false</DeviceLifeCycleFlag>
       <DeviceStatusChangeFlag>false</DeviceStatusChangeFlag>
       <DeviceTopoLifeCycleFlag>false</DeviceTopoLifeCycleFlag>
       <FoundDeviceListFlag>false</FoundDeviceListFlag>
       <OtaEventFlag>true</OtaEventFlag>
       <ProductKey>a1Zkii7****</ProductKey>
       <RequestId>73B9DF43-7780-47DE-8BED-077729D28BD2</RequestId>
       <Success>true</Success>
       <ThingHistoryFlag>false</ThingHistoryFlag>
       <Type>AMQP</Type>
</QuerySubscribeRelationResponse>
```

`JSON` 格式

```
{
  "DeviceLifeCycleFlag": false,
  "RequestId": "73B9DF43-7780-47DE-8BED-077729D28BD2",
  "DeviceDataFlag": false,
  "DeviceTopoLifeCycleFlag": false,
  "DeviceStatusChangeFlag": false,
  "ConsumerGroupIds": [
    "Xs95KifeaSKbi8tKkcoD00****"
  ],
  "Success": true,
  "ThingHistoryFlag": false,
  "Type": "AMQP",
  "FoundDeviceListFlag": false,
  "OtaEventFlag": true,
  "ProductKey": "a1Zkii7****"
}
```

