# CreateOTAStaticUpgradeJob

Creates a static update batch.

## Limits

-   The firmware must be verified before you call this operation. For more information, see [CreateOTAVerifyJob](~~147480~~).
-   You can initiate firmware update tasks for up to 200 devices in each call.
-   A device can be in the pending or updating state only in one update task. If you initiate another update task for a device that is in the pending or updating state, the update task fails.
-   You can initiate multiple static update tasks for a single firmware.
-   Each Alibaba Cloud account can run a maximum of 20 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.


## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=CreateOTAStaticUpgradeJob&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateOTAStaticUpgradeJob|The operation that you want to perform. The operation that you want to perform. Set the value to CreateOTAStaticUpgradeJob. |
|FirmwareId|String|Yes|nx3xxVvFdwvn6dim50PY03\*\*\*\*|The ID of the firmware.

A firmware ID is returned when you call the [CreateOTAFirmware](~~147311~~) operation to create the firmware.

You can call the [ListOTAFirmware](~~147450~~) operation and view the firmware ID in the response. |
|ProductKey|String|Yes|a1Le6d0\*\*\*\*|The ProductKey of the product to which the firmware belongs. |
|TargetSelection|String|Yes|ALL|The scope of the update. Valid values:

-   **ALL**: complete update
-   **SPECIFIC**: specific update
-   **GRAY**: phased update |
|IotInstanceId|String|No|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|The ID of the instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |
|SrcVersion.N|RepeatList|No|V1.0.1|The list of firmware versions to be updated.

You can call the [QueryDeviceDetail](~~69594~~) operation and view the **FirmwareVersion** parameter in the response.

**Note:**

-   You must specify this parameter if you set the TargetSelection parameter to `ALL` or `GRAY`. In this case, do not specify the **TargetDeviceNames** parameter.
-   You cannot specify this parameter if you set the value of the TargetSelection parameter to `SPECIFIC`.
-   When you use differential firmware to perform complete update or phased update tasks, the value of this parameter must be the same as that of the **SrcVersion** parameter.
-   The list cannot contain duplicate versions.
-   A maximum of 10 versions can be specified. |
|ScheduleTime|Long|No|1577808000000|The time to update the device firmware.

The scheduled time ranges from 5 minutes later than the current time to 7 days. The value must be a 13-bit timestamp.

If you do not specify this parameter, the update starts immediately. |
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
|GrayPercent|String|No|33.33|The phase ratio of the phased update. The value is a percentage in the string format. It can be up to three decimal places. The calculated number of devices is rounded down to the nearest integer. You must specify at least one device for a phased update.

For example, if you set the phased update ratio to 33.33 for 100 devices, the number of devices to be updated is 33.

You must specify this parameter if you specify the TargetSelection parameter to `GRAY`. |
|TargetDeviceName.N|RepeatList|No|deviceName1|The list of devices.``

**Note:**

-   You must specify this parameter if you set the TargetSelection parameter to `SPECIFIC`. In this case, do not specify the **SrcVersions** parameter.
-   When you use differential firmware to perform complete update or phased update tasks, the value of this parameter must be the same as that of the **SrcVersion** parameter.
-   You can call the [QueryDeviceDetail](~~69594~~)operation and view the **FirmwareVersion**parameter in the response.
-   The devices in the list must belong to the same product as the firmware.
-   The list cannot contain duplicate device names.
-   The list can contain up to 200 device names. |
|ScheduleFinishTime|Long|No|1577909000000|The time to end the update.

The end time must be 1 hour to 30 days later than the start time that is specified by the **ScheduleTime** parameter. The value must be a 13-bit timestamp.

If you do not specify this parameter, the update does not end. |
|OverwriteMode|Integer|No|1|Specifies whether to overwrite the previous update task. Valid values:

-   1: do not overwrite. If a device already has an update task, the previous task is implemented.
-   2: overwrite. Only the current task is implemented.

If you do not specify this parameter, the previous update task is not overwritten by default.

**Note:** The update task that is in progress is not overwritten. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For more information about common request parameters, see [Common parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|MissingFirmwareId|The error code returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|Data|Struct| |The update batch information returned if the call is successful. A data struct contains the following parameters. |
|JobId|String|wahVIzGkCMuAUE2gDERM02\*\*\*\*|The ID of the update batch. |
|UtcCreate|String|2019-11-04T06:22:19.566Z|The time when the update batch was created. The time is displayed in UTC. |
|ErrorMessage|String|FirmwareId is mandatory for this action.|The error message returned if the call fails. |
|RequestId|String|29EC7245-0FA4-4BB6-B4F5-5F04818FDFB1|The ID of the request. |
|Success|Boolean|true|Indicates whether the call was successful.**true** indicates that the call was successful.**false**indicates that the call failed. |

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateOTAStaticUpgradeJob
&FirmwareId=nx3xxVvFdwvn6dim50PY03****
&MaximumPerMinute=1000
&ProductKey=a1Le6d0****
&RetryCount=1
&RetryInterval=60
&TargetSelection=ALL
&TimeoutInMinutes=1440
&SrcVersion.1=V1.0.1
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreateOTAStaticUpgradeJobResponse>
   <Data>
       <JobId>wahVIzGkCMuAUE2gDERM02****</JobId>
       <UtcCreate>2019-11-04T06:22:19.566Z</UtcCreate>
   </Data>
   <RequestId>29EC7245-0FA4-4BB6-B4F5-5F04818FDFB1</RequestId>
   <Success>true</Success>
</CreateOTAStaticUpgradeJobResponse>
```

`JSON` format

```
{
  "Data": {
    "JobId": "wahVIzGkCMuAUE2gDERM02****",
    "UtcCreate": "2019-11-04T06:22:19.566Z"
  },
  "RequestId": "29EC7245-0FA4-4BB6-B4F5-5F04818FDFB1",
  "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

