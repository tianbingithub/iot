# ListOTATaskByJob

Queries the specific tasks of an update batch.

## Limits

Each Alibaba Cloud account can run a maximum of 10 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=ListOTATaskByJob&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ListOTATaskByJob|The operation that you want to perform. Set the value to ListOTATaskByJob. |
|CurrentPage|Integer|Yes|1|The number of the page to return. Pages start from page 1. |
|JobId|String|Yes|7glPHmaDYLAYMD1HHutT02\*\*\*\*|The ID of the update batch. After you call the [CreateOTAVerifyJob](~~147480~~), [CreateOTAStaticUpgradeJob](~~147496~~), or [CreateOTADynamicUpgradeJob](~~147887~~) operation, you can obtain the **JobId** parameter. You can also view the batch ID on the**Firmware Details** page of the IoT Platform console. |
|PageSize|Integer|Yes|10|The number of entries returned per page. Maximum value: 100. |
|IotInstanceId|String|No|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|The ID of the IoT instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |
|TaskStatus|String|No|FAILED|If you specify this parameter, only the update tasks that are in the specified status are queried.

-   **QUEUED**: The update task is being queued.
-   **NOTIFIED**: The device has notified the server for an update.
-   **IN\_PROGRESS**: The update task is in progress.
-   **SUCCEEDED**: The update task is successful.
-   **FAILED**: The update task failed.
-   **CANCELED**: The update task is canceled.

If you do not specify this parameter, all update tasks of the specified batch are queried. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For more information about common request parameters, see [Common parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|CurrentPage|Integer|1|The page number of the returned page. |
|Data|Array of SimpleOTATaskInfo| |The update task information returned if the call is successful. For more information, see **SimpleOTATaskInfo**. |
|SimpleOTATaskInfo| | | |
|DestVersion|String|1.0.1|The destination firmware version of the update. |
|DeviceName|String|testDevice2|The name of the device. |
|FirmwareId|String|q3j9OYBjUAZMv1hlMgdo03\*\*\*\*|The ID of the firmware. |
|IotId|String|nadRdeffljdEndlfadgadfse\*\*\*\*|The ID of the device. |
|JobId|String|7glPHmaDYLAYMD1HHutT02\*\*\*\*|The ID of the update batch. |
|ProductKey|String|a1GUfrM\*\*\*\*|The ProductKey of the product to which the device belongs. |
|ProductName|String|MyProduct|The name of the product to which the device belongs. |
|Progress|String|0.00|The current update progress. |
|SrcVersion|String|1.0.0|The original firmware version of the device. |
|TaskDesc|String|report version is not conform|The description of the update task. This parameter displays an error message when the device update times out or the update task is canceled. |
|TaskId|String|y3tOmCDNgpR8F9jnVEzC01\*\*\*\*|The ID of the update task. |
|TaskStatus|String|FAILED|The update status of the device.

-   **QUEUED**: The update task is being queued.
-   **NOTIFIED**: The device has notified the server for an update.
-   **IN\_PROGRESS**: The update task is in progress.
-   **SUCCEEDED**: The update task is successful.
-   **FAILED**: The update task failed.
-   **CANCELED**: The update task is canceled. |
|UtcCreate|String|2019-11-04T03:38:22.000Z|The time when the update task was created. The time is displayed in UTC. |
|UtcModified|String|2019-11-04T03:38:22.000Z|The time when the update task was last modified. The time is displayed in UTC. |
|ErrorMessage|String|A system exception occurred.|The error message returned if the call fails. |
|PageCount|Integer|1|The total number of pages returned. |
|PageSize|Integer|10|The number of entries returned per page. |
|RequestId|String|A59D3BE1-E9A3-43F3-9B50-B7C8DE165D9B|The ID of the request. |
|Success|Boolean|true|Indicates whether the call was successful.**true** indicates that the call was successful.**false**indicates that the call failed. |
|Total|Integer|2|The total number of update tasks. |

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListOTATaskByJob
&JobId=7glPHmaDYLAYMD1HHutT02****
&PageSize=10
&CurrentPage=1
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ListOTATaskByJobResponse>
    <PageCount>1</PageCount>
    <Data>
        <SimpleOTATaskInfo>
            <SrcVersion>1.0.0</SrcVersion>
            <DeviceName>testDevice1</DeviceName>
            <FirmwareId>q3j9OYBjUAZMv1hlMgdo03****</FirmwareId>
            <IotId>SR8FiTu1R9tlUR2V1bmi00105****</IotId>
            <ProductKey>a1GUfrM****</ProductKey>
            <JobId>7glPHmaDYLAYMD1HHutT02****</JobId>
            <TaskDesc>report version is not conform</TaskDesc>
            <DestVersion>1.0.1</DestVersion>
            <UtcCreate>2019-11-04T03:38:15.000Z</UtcCreate>
            <UtcModified>2019-11-04T03:38:15.000Z</UtcModified>
            <TaskStatus>FAILED</TaskStatus>
            <ProductName>MyProduct</ProductName>
            <TaskId>y3tOmCDNgpR8F9jnVEzC01****</TaskId>
            <Progress>0.00</Progress>
        </SimpleOTATaskInfo>
        <SimpleOTATaskInfo>
            <SrcVersion>1.0.0</SrcVersion>
            <DeviceName>testDevice2</DeviceName>
            <FirmwareId>q3j9OYBjUAZMv1hlMgdo03****</FirmwareId>
            <IotId>nadRdeffljdEndlfadgadfse****</IotId>
            <ProductKey>a1GUfrM****</ProductKey>
            <JobId>7glPHmaDYLAYMD1HHutT02****</JobId>
            <TaskDesc></TaskDesc>
            <DestVersion>1.0.1</DestVersion>
            <UtcCreate>2019-11-04T03:38:22.000Z</UtcCreate>
            <UtcModified>2019-11-04T03:38:22.000Z</UtcModified>
            <TaskStatus>SUCCEEDED</TaskStatus>
            <ProductName>MyProduct</ProductName>
            <TaskId>ZS9sNBb1ahsu6khqr9II01****</TaskId>
            <Progress>100.00</Progress>
        </SimpleOTATaskInfo>
    </Data>
    <PageSize>10</PageSize>
    <RequestId>A01829CE-75A1-4920-B775-921146A1AB79</RequestId>
    <Success>true</Success>
    <CurrentPage>1</CurrentPage>
    <Total>2</Total>
</ListOTATaskByJobResponse>
```

`JSON` format

```
{
  "PageCount": 1,
  "Data": {
    "SimpleOTATaskInfo": [{
      "SrcVersion": "1.0.0",
      "DeviceName": "testDevice1",
      "FirmwareId": "q3j9OYBjUAZMv1hlMgdo03****",
      "IotId": "SR8FiTu1R9tlUR2V1bmi00105****",
      "ProductKey": "a1GUfrM****",
      "JobId": "7glPHmaDYLAYMD1HHutT02****",
      "TaskDesc": "report version is not conform",
      "DestVersion": "1.0.1",
      "UtcCreate": "2019-11-04T03:38:15.000Z",
      "UtcModified": "2019-11-04T03:38:15.000Z",
      "TaskStatus": "FAILED",
      "ProductName": "MyProduct",
      "TaskId": "y3tOmCDNgpR8F9jnVEzC01****",
      "Progress": "0.00"
    }, {
      "SrcVersion": "1.0.0",
      "DeviceName": "testDevice2",
      "FirmwareId": "q3j9OYBjUAZMv1hlMgdo03****",
      "IotId": "nadRdeffljdEndlfadgadfse****",
      "ProductKey": "a1GUfrM****",
      "JobId": "7glPHmaDYLAYMD1HHutT02****",
      "TaskDesc": "",
      "DestVersion": "1.0.1",
      "UtcCreate": "2019-11-04T03:38:22.000Z",
      "UtcModified": "2019-11-04T03:38:22.000Z",
      "TaskStatus": "SUCCEEDED",
      "ProductName": "MyProduct",
      "TaskId": "ZS9sNBb1ahsu6khqr9II01****",
      "Progress": "100.00"
    }]
  },
  "PageSize": 10,
  "RequestId": "A59D3BE1-E9A3-43F3-9B50-B7C8DE165D9B",
  "CurrentPage": 1,
  "Success": true,
  "Total": 2
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

