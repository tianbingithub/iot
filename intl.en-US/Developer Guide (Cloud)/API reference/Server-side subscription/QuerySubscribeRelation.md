# QuerySubscribeRelation

Queries a Message Service \(MNS\) or AMQP server-side subscription.

## Limits

Each Alibaba Cloud account can run a maximum of five queries per second \(QPS\).

**Note:** The RAM users of an Alibaba Cloud account share the quota of the account.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=QuerySubscribeRelation&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|QuerySubscribeRelation|The operation that you want to perform. Set the value to QuerySubscribeRelation. |
|ProductKey|String|Yes|a1fyXVF\*\*\*\*|The key of the product that is specified for the subscription. |
|Type|String|Yes|AMQP|The type of the subscription. Valid values:

-   **MNS**
-   **AMQP** |
|IotInstanceId|String|No|iot-cn-0pp1n8t\*\*\*\*|The instance ID. This parameter is not required for the public instance. However, the parameter is required for your purchased instances. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For more information about common request parameters, see [Common parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code that is returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|ErrorMessage|String|A system exception has occurred.|The error message that is returned if the call fails. |
|DeviceDataFlag|Boolean|true|Indicates whether upstream device messages are pushed. Valid values:

-   **true**: yes
-   **false**: no |
|DeviceStatusChangeFlag|Boolean|true|Indicates whether notifications about device status changes are pushed. Valid values:

-   **true**: yes
-   **false**: no |
|FoundDeviceListFlag|Boolean|true|Indicates whether messages are pushed if a gateway detects new sub-devices. Valid values:

-   **true**: yes. The value **true** is returned only when you query a gateway product.
-   **false**: no. |
|DeviceTopoLifeCycleFlag|Boolean|true|Indicates whether messages about topological relationship changes of devices are pushed. Valid values:

-   **true**: yes. The value **true** is returned only when you query a gateway product.
-   **false**: no. |
|DeviceLifeCycleFlag|Boolean|true|Indicates whether messages about device lifecycle changes are pushed. Valid values:

-   **true**: yes
-   **false**: no |
|ThingHistoryFlag|Boolean|true|Indicates whether upstream historical Thing Specification Language \(TSL\) data is pushed. Valid values:

-   **true**: yes
-   **false**: no |
|OtaEventFlag|Boolean|true|Indicates whether notifications about firmware updates are pushed. Valid values:

-   **true**: yes
-   **false**: no |
|DeviceTagFlag|Boolean|true|Indicates whether messages about device tag changes are pushed. Valid values:

-   **true**: yes. This parameter is valid only when the **Type** parameter is set to **AMQP**.
-   **false**: no.

Default value: **false**. |
|OtaVersionFlag|Boolean|true|Indicates whether messages about the version number of the OTA module are pushed. Valid values:

-   **true**: yes. This parameter is valid only when the **Type** parameter is set to **AMQP**.
-   **false**: no.

Default value: **false**. |
|OtaJobFlag|Boolean|true|Indicates whether notifications about OTA batch updates are pushed. Valid values:

-   **true**: yes. This parameter is valid only when the **Type** parameter is set to **AMQP**.
-   **false**: no.

Default value: **false**. |
|ProductKey|String|a1fyXVF\*\*\*\*|The product key of the product that is specified for the subscription. |
|Success|Boolean|true|Indicates whether the call succeeded. Valid values:

-   **true**: The call succeeded.
-   **false**: The call failed. |
|ConsumerGroupIds|List|\[DEFAULT\_GROUP,br45A6A1amoRFGN7x1zP00\*\*\*\*\]|The IDs of the consumer groups that are created in the AMQP subscription. This parameter is required if the **Type** parameter is set to **AMQP**. |
|Type|String|AMQP|The type of the subscription. Valid values:

-   **MNS**
-   **AMQP** |
|RequestId|String|21D327AF-A7DE-4E59-B5D1-ACAC8C024555|The unique identifier that Alibaba Cloud generates for the request. |
|MnsConfiguration|String|\{ "themeName": "mns-test-topic1", "regionName": "cn-shanghai", "role": \{ "roleArn": "acs:ram::5645\*\*\*:role/aliyuniotaccessingmnsrole", "roleName": "AliyunIOTAccessingMNSRole" \} \}|The configurations of the MNS queue. This parameter is required if the **Type** parameter is set to **MNS**.

For more information about the structure of this parameter and an example, see the following section. |

**Composition of the MnsConfiguration parameter**

|Field

|Description |
|-------|-------------|
|themeName

|The name of the MNS topic that is used to receive messages. |
|regionName

|The ID of the region in which MNS is deployed, for example, cn-shanghai. |
|role

|The information of the RAM role. To grant IoT Platform access to MNS, you can assign a RAM role to IoT Platform. The following script shows the syntax of a RAM role:

`{"roleArn":"acs:ram::5645***:role/aliyuniotaccessingmnsrole","roleName": "AliyunIOTAccessingMNSRole"}`

Replace`5645***`with your Alibaba Cloud account ID. You can log on to the Alibaba Cloud Management Console and view the account ID on the Security Settings page.

`AliyunIOTAccessingMNSRole`is a service role that is defined in RAM. This role is used to grant IoT Platform access to MNS. You can log on to the RAM console and manage the RAM role on the RAM Roles page. |

Example of the **MnsConfiguration** parameter:

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

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QuerySubscribeRelation
&ProductKey=a1Zkii7****
&Type=AMQP
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

