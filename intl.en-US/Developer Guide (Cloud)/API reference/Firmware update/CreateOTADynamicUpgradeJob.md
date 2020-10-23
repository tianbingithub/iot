# CreateOTADynamicUpgradeJob

Creates a dynamic update batch.

## Limits

-   The firmware must be verified before you call this operation. For more information, see [CreateOTAVerifyJob](~~147480~~).
-   Each device can be in the pending or updating state only in one update task. If you initiate another update task for a device that is in the pending or updating state, the update task fails.
-   Each firmware can have only one dynamic update task that is in the running state.
-   If a device is included in dynamic update policies of different firmware, the device performs the latest dynamic update.
-   After a dynamic update batch is created, IoT Platform automatically creates the corresponding dynamic update policy. You can call [CancelOTAStrategyByJob](~~147905~~) operation to cancel a dynamic update policy.
-   Each Alibaba Cloud account can run a maximum of 20 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.


## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=CreateOTADynamicUpgradeJob&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateOTADynamicUpgradeJob|The operation that you want to perform. Set the value to CreateOTADynamicUpgradeJob. |
|FirmwareId|String|Yes|nx3xxVvFdwvn6dim50PY03\*\*\*\*|The ID of the firmware.

A firmware ID is returned when you call the [CreateOTAFirmware](~~147311~~) operation to create the firmware.

You can call the [ListOTAFirmware](~~147450~~) operation and view the firmware ID in the response. |
|ProductKey|String|Yes|a1Le6d0\*\*\*\*|The ProductKey of the product to which the firmware belongs. |
|IotInstanceId|String|No|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|The ID of the IoT instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |
|SrcVersion.N|RepeatList|No|V1.0.1|The list of firmware versions to be updated.

You can call the [QueryDeviceDetail](~~69594~~) operation and view the**FirmwareVersion** parameter in the response.

**Note:**

-   When you use differential firmware to perform complete update or phased update tasks, the value of this parameter must be the same as that of the **SrcVersion** parameter.
-   The list cannot contain duplicate versions.
-   A maximum of 10 versions can be specified. |
|RetryInterval|Integer|No|60|The automatic retry interval after a device fails to be updated. Unit: minutes. Valid values:

-   **0**: An automatic retry is immediately performed.
-   **10**: An automatic retry is performed after 10 minutes.
-   **30**: An automatic retry is performed after 30 minutes.
-   **60**: An automatic retry is performed after 60 minutes \(1 hour\).
-   **1440**: An automatic retry is performed after 1,440 minutes \(24 hours\).

If you do not specify this parameter, no retry is performed. |
|RetryCount|Integer|No|1|The number of automatic retries.

You must specify this parameter if you specify the **RetryInterval** parameter.

Valid values:

-   **1**: retries once
-   **2**: retries twice
-   **5**: retries five times |
|TimeoutInMinutes|Integer|No|1440|The timeout period of the update. Unit: minutes. Valid values: 1 to 1,440.

If an update is not completed after the specified timeout period, the update fails.

**Note:**

-   The timeout period starts from the time when the specified device submits the update progress for the first time.
-   If an update fails due to timeout, no retry is triggered. |
|MaximumPerMinute|Integer|No|1000|The maximum number of devices to which the download URL of the firmware is pushed per minute. Valid values: 0 to 1,000.

If you do not specify this parameter, the default value 1,000 is used. |
|OverwriteMode|Integer|No|2|Specifies whether to overwrite the previous update task. Valid values:

-   **1**: do not overwrite. If a device already has an update task, the previous task is implemented.
-   **2**: overwrite. Only the current task is implemented.

If you do not specify this parameter, the previous update task is not overwritten by default.

**Note:** The update task that is in progress is not overwritten. |
|DynamicMode|Integer|No|1|The mode of dynamic update. Valid values:

-   **1**: constantly updates the devices that meet the conditions.
-   **2**: updates only the devices that subsequently submit the latest firmware versions.

If you do not specify this parameter, the default value 1 is used. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For more information about common request parameters, see [Common parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|Data|Struct| |The update batch information returned if the call is successful. A data struct contains the following parameters. |
|JobId|String|XUbmsMHmkqv0PiAG\*\*\*\*010001|The ID of the update batch. |
|UtcCreate|String|2019-05-10T02:18:53.000Z|The time when the update batch was created. The time is displayed in UTC. |
|ErrorMessage|String|A system exception occurred.|The error message returned if the call fails. |
|RequestId|String|9F41D14E-CB5F-4CCE-939C-057F39E688F5|The ID of the request. |
|Success|Boolean|true|Indicates whether the call was successful.**true**indicates that the call was successful.**false** indicates that the call failed. |

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateOTADynamicUpgradeJob
&FirmwareId=nx3xxVvFdwvn6dim50PY03****
&MaximumPerMinute=1000
&ProductKey=a1Le6d0****
&RetryCount=1
&RetryInterval=60
&TimeoutInMinutes=1440
&SrcVersion.1=V1.0.1
&OverwriteMode=2
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreateOTADynamicUpgradeJobResponse>
   <Data>
       <JobId>wahVIzGkCMuAUE2gDERM02****</JobId>
       <UtcCreate>2019-11-04T06:22:19.566Z</UtcCreate>
   </Data>
   <RequestId>29EC7245-0FA4-4BB6-B4F5-5F04818FDFB1</RequestId>
   <Success>true</Success>
</CreateOTADynamicUpgradeJobResponse>
```

`JSON` format

```
{
  "Data": {
    "JobId": "XUbmsMHmkqv0PiAG****010001",
    "UtcCreate": "2019-05-10T02:18:53.000Z"
  },
  "RequestId": "9F41D14E-CB5F-4CCE-939C-057F39E688F5",
  "Success": true
}
```

## Errors

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

