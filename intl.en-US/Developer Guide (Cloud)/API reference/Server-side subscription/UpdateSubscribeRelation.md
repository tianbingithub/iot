# UpdateSubscribeRelation

Modifies a Message Service \(MNS\) or AMQP server-side subscription.

## Limits

Each Alibaba Cloud account can run a maximum of 5 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=UpdateSubscribeRelation&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpdateSubscribeRelation|The operation that you want to perform. Set the value to UpdateSubscribeRelation. |
|ProductKey|String|Yes|a1fyXVF\*\*\*\*|The ProductKey of the product that is specified for the subscription. |
|Type|String|Yes|AMQP|The type of the subscription. Valid values:

 -   **MNS**
-   **AMQP** |
|IotInstanceId|String|No|iot-cn-0pp1n8t\*\*\*\*|The ID of the instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |
|DeviceDataFlag|Boolean|No|true|Specifies whether to push upstream device messages. Valid values:

 -   **true**: yes
-   **false**: no

 Default value: **false** |
|DeviceStatusChangeFlag|Boolean|No|true|Specifies whether to push messages about device status changes. Valid values:

 -   **true**: yes
-   **false**: no

 Default value: **false** |
|FoundDeviceListFlag|Boolean|No|true|Specifies whether to push messages if a gateway detects new sub-devices. Valid values:

 -   **true**: yes. This parameter is valid only for gateway products.
-   **false**: no

 Default value: **false** |
|DeviceTopoLifeCycleFlag|Boolean|No|true|Specifies whether to push messages about topological relationship changes of devices. Valid values:

 -   **true**: yes. This parameter is valid only for gateway products.
-   **false**: no

 Default value: **false** |
|DeviceLifeCycleFlag|Boolean|No|true|Specifies whether to push messages about device lifecycle changes. Valid values:

 -   **true**: yes
-   **false**: no

 Default value: **false** |
|ThingHistoryFlag|Boolean|No|true|Specifies whether to push upstream historical Thing Specification Language \(TSL\) data. Valid values:

 -   **true**: yes
-   **false**: no

 Default value: **false** |
|OtaEventFlag|Boolean|No|true|Specifies whether to push notifications about firmware updates. Valid values:

 -   **true**: yes
-   **false**: no

 Default value: **false** |
|DeviceTagFlag|Boolean|No|true|Specifies whether to push messages about device tag changes. Valid values:

 -   **true**: yes. This parameter is valid only if the **Type** parameter is set to **AMQP**.
-   **false**: no

 Default value: **false** |
|OtaVersionFlag|Boolean|No|true|Specifies whether to push messages about OTA module version numbers. Valid values:

 -   **true**: yes This parameter is valid only if the **Type** parameter is set to **AMQP**.
-   **false**: no

 Default value: **false** |
|OtaJobFlag|Boolean|No|true|Specifies whether to push notifications about the status of OTA update batches. Valid values:

 -   **true**: yes. This parameter is valid only if the **Type** parameter is set to **AMQP**.
-   **false**: no

 Default value: **false** |
|MnsConfiguration|String|No|\{ "themeName": "mns-test-topic1", "regionName": "cn-shanghai", "role": \{ "roleArn": "acs:ram::5645\*\*\*:role/aliyuniotaccessingmnsrole", "roleName": "AliyunIOTAccessingMNSRole" \} \}|The configurations of the MNS queue. This parameter is required if the **Type** parameter is set to **AMQP**.

 For more information about specific requirements and examples of MNS queues, see the following table. |
|ConsumerGroupIds.N|RepeatList|No|nJRaJPn5U1JITGfjBO9l00\*\*\*\*|The IDs of the consumer groups that are created in the AMQP subscription. This parameter is required if the **Type** parameter is set to **AMQP**.

 After you call the [CreateConsumerGroup](~~170388~~) operation to create a consumer group, the consumer group ID is returned. You can call the [QueryConsumerGroupList](~~170419~~) operation to query the consumer group ID based on the group name. You can also log on to the IoT Platform console and choose **Rules**\>**Server-side Subscription**\>**Consumer Groups** to view the consumer group ID. |

**Note:** You must set at least one Flag-related parameter to **true**.****

**Definition of the MnsConfiguration parameter**

|Field

|Description |
|-------|-------------|
|themeName

|The name of the MNS topic that is used to receive messages. |
|regionName

|The ID of the region in which MNS is deployed. Example: cn-shanghai. |
|role

|The information of the RAM role. To grant IoT Platform the access to MNS, you can assign a RAM role to IoT Platform. The following script shows the syntax of a RAM role:

 `{"roleArn":"acs:ram::5645***:role/aliyuniotaccessingmnsrole","roleName": "AliyunIOTAccessingMNSRole"}`

 Replace `5645***` with your Alibaba Cloud account ID. You can log on to the Alibaba Cloud Management Console and view the account ID on the Security Settings page.

 `AliyunIOTAccessingMNSRole` indicates a service role that is defined in RAM. This role is used to grant IoT Platform the access to MNS. You can go to the RAM Roles page of the RAM console to manage RAM roles. |

**Example of the MnsConfiguration parameter**

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

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For more information about common request parameters, see [Common parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|ErrorMessage|String|A system exception occurred.|The error message returned if the call fails. |
|RequestId|String|21D327AF-A7DE-4E59-B5D1-ACAC8C024555|The ID of the request. |
|Success|Boolean|true|Indicates whether the call was successful.

 -   **true**: The call was successful.
-   **false**: The call failed. |

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateSubscribeRelation
&OtaEventFlag=true
&ProductKey=a1Zkii7****
&Type=AMQP
&ConsumerGroupIds.1=Xs95KifeaSKbi8tKkcoD00****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<UpdateSubscribeRelationResponse>
        <RequestId>C21DA94F-07D7-482F-8A0C-5BB0E3CC1A82</RequestId>
        <Success>true</Success>
</UpdateSubscribeRelationResponse>
```

`JSON` format

```
{
    "RequestId": "C21DA94F-07D7-482F-8A0C-5BB0E3CC1A82",
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

