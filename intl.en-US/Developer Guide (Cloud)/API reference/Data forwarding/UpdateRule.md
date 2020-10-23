# UpdateRule

Modifies a specified rule.

## Limits

Each Alibaba Cloud account can run a maximum of 50 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=UpdateRule&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpdateRule|The operation that you want to perform. Set the value to UpdateRule. |
|RuleId|Long|Yes|100000|The ID of the rule that you want to modify. You can log on to the IoT Platform console and choose **Rule Engine**\>**Data Forwarding** to view the rule ID. You can also call the [ListRule](~~69486~~) operation and view the rule ID in the response. |
|IotInstanceId|String|No|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|The ID of the instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |
|Select|String|No|deviceName\(\) as deviceName, items.Humidity.value as Humidity, items.Temperature.value as Temperature|The SQL SELECT statement that you want to execute. For more information, see [SQL expressions](~~30554~~).

 **Note:** Specify the fields that follow the **Select** keyword for this parameter. For example, if the **Select** statement is `Select a,b,c`, specify `a,b,c` for this parameter. |
|ShortTopic|String|No|+/thing/event/property/post|The topic to which this rule is applied. Syntax: `${deviceName}/topicShortName`. `${deviceName}` specifies the name of the device, and `topicShortName` specifies the custom name of the device.

 -   Basic communication topics or Thing Specification Language \(TSL\)-based communication topics. Syntax: `${deviceName}/topicShortName`. You can replace `${deviceName}` with the `+` wildcard. The wildcard indicates that the topic applies to all devices under the product. Valid values of `topicShortName`:
    -   `/thing/event/property/post`: submits the property data of a device.
    -   `/thing/event/${tsl.event.identifier}/post`: submits the event data of a device. `${tsl.event.identifier}` specifies the identifier of an event in the TSL.
    -   `/thing/lifecycle`: submits device lifecycle changes.
    -   `/thing/downlink/reply/message`: sends a response to a request from IoT Platform.
    -   `/thing/list/found`: submits the data when a gateway detects a new sub-device.
    -   `/thing/topo/lifecycle`: submits device topology changes.
    -   `/thing/event/property/history/post`: submits historical property data of a device.
    -   `/thing/event/${tsl.event.identifier}/post`: submits the historical event data of a device. `${tsl.event.identifier}` specifies the identifier of an event in the TSL.
    -   `/ota/upgrade`: submits firmware update status.
    -   `/ota/version/post`: submits OTA module versions.
    -   `/thing/deviceinfo/update`: submits device tag changes.
    -   `/edge/driver/${driver_id}/point_post`: submits pass-through data from Link IoT Edge. `${driver_id}` specifies the ID of the driver that a device uses to access Link IoT Edge.

`${packageId}/${jobId}/ota/job/status`: submits the status of OTA update batches. This topic is a basic communication topic. `${packageId}` specifies the ID of the firmware. `${jobId}` specifies the ID of the update batch.

-   Custom topics. Example: `${deviceName}/user/get`.

You can call the [QueryProductTopic](~~69647~~) operation to view all custom topics of the product.

When you specify a custom topic, you can use the `+` and `#` wildcards.

    -   You can replace `${deviceName}` with the `+` wildcard. The wildcard indicates that the topic applies to all devices under the product.
    -   You can replace the fields that follow $\{deviceName\} with `/user/#`. The `#` wildcard indicates that the topic applies whatever values are specified for the fields that follow`/user`.

For more information about how to use wildcards, see[Wildcards in topic categories](~~85539~~).

-   Topic that is used to submit device status changes: `${deviceName}`.

You can use the `+` wildcard. In this case, the status changes of all devices under the product are submitted. |
|Where|String|No|Temperature\>35|The condition that is used to trigger the rule. For more information, see [SQL expressions](~~30554~~).

 **Note:** Specify the fields that follow the **Where** keyword for this parameter. For example, if the **Where** statement is `Where a>10`, specify `a>10` for this parameter. |
|ProductKey|String|No|aladaeW\*\*\*\*|The ProductKey of the product to which the rule applies. |
|Name|String|No|test\_2|The name of the rule. The name must be 1 to 30 characters in length and can contain English letters, digits, underscores \(\_\), and hyphens \(-\). Chinese language is also supported. Each Chinese symbol occupies 2 characters. |
|RuleDesc|String|No|test|The description of the rule. The description can be up to 100 characters in length. Each Chinese symbol occupies 1 characters. |
|TopicType|Integer|No|1|-   **0**: The topic is a basic communication topic or TSL-based communication topic.****
-   **1**: The topic is a custom topic.
-   **2**: The topic is used to submit device status changes. Syntax: `/as/mqtt/status/${productKey}/${deviceName}`. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For more information, see [Common request parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|ErrorMessage|String|A system exception occurred.|The error message returned if the call fails. |
|RequestId|String|9A2F243E-17FE-4846-BAB5-D02A25155AC4|The ID of the request. |
|Success|Boolean|true|Indicates whether the call is successful. **true** indicates that the call is successful. **false** indicates that the call fails. |

## Examples

Sample requests

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
&<Common request parameters>
```

Sample success responses

`XML` format

```
<UpdateRuleResponse>
      <RequestId>9A2F243E-17FE-4874-QBB5-D02A25155AC8</RequestId>
      <Success>true</Success>
</UpdateRuleResponse>
```

`JSON` format

```
{
    "RequestId":"9A2F243E-17FE-4846-BAB5-D02A25155AC4",
    "Success":true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

