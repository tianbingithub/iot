# QueryOTAJob

调用该接口查询指定升级批次的详情。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=QueryOTAJob&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|QueryOTAJob|系统规定参数。取值：QueryOTAJob。 |
|JobId|String|是|wahVIzGkCMuAUE2gDERM02\*\*\*\*|升级批次ID。

 您调用[CreateOTAVerifyJob](~~147480~~)、[CreateOTAStaticUpgradeJob](~~147496~~)或[CreateOTADynamicUpgradeJob](~~147887~~)创建升级任务批次后，返回的**JobId**。您也可以在物联网平台控制台的**升级包详情**页查看。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Struct| |调用成功时，返回的升级批次信息。详情见以下Data所包含的参数。 |
|DestVersion|String|1.0.1|升级目标版本号。 |
|DynamicMode|Integer|1|动态升级模式。取值范围：

 -   **1**：除了升级当前满足升级条件的设备，还将持续检查设备是否满足升级条件，对满足升级条件的设备进行升级。
-   **2**：仅对后续上报新版本号的设备生效。

 仅升级策略为动态升级时，返回该参数。 |
|FirmwareId|String|UfuxnwygsuSkVE0VCN\*\*\*\*0100|升级包ID。 |
|GrayPercent|String|50.00|灰度升级的灰度比例。

 仅升级范围为灰度升级时，返回该参数。 |
|JobDesc|String|batch upgrade|升级批次描述。 |
|JobId|String|HvKuBpuk3rdk6E92CP\*\*\*\*0200|升级批次ID，批次的唯一标识符。 |
|JobStatus|String|IN\_PROGRESS|批次的状态。

 -   **PLANNED**：计划中。批次已创建，但是定时时间未到。仅定时静态升级的批次可能返回该值。
-   **IN\_PROGRESS**：执行中。
-   **COMPLETED**：已完成。
-   **CANCELED**：已取消。 |
|JobType|String|UPGRADE\_FIRMWARE|批次类型。

 -   **VERIFY\_FIRMWARE**：升级包验证批次。
-   **UPGRADE\_FIRMWARE**：批量升级批次。 |
|MaximumPerMinute|Integer|1000|每分钟最多向多少个设备推送升级包下载URL。 |
|Name|String|Firmware2|升级包名称。 |
|OverwriteMode|Integer|1|是否覆盖之前的升级任务。取值：

 -   **1**：不覆盖。若设备已有升级任务，则只执行已有任务。
-   **2**：覆盖。设备只执行新的升级任务。

 不覆盖升级中的任务。 |
|ProductKey|String|a19mzPZ\*\*\*\*|升级包所属产品的ProductKey。 |
|RetryCount|Integer|1|设备升级失败后，自动重试次数。

 创建升级批次时，设置了失败重试策略，则返回该参数。 |
|RetryInterval|Integer|60|设备升级失败后，自动重试时间间隔，单位：分钟。

 创建升级批次时，设置了失败重试策略，则返回该参数。 |
|SelectionType|String|STATIC|升级策略。

 -   **DYNAMIC**：动态升级。调用[CreateOTADynamicUpgradeJob](~~147887~~)创建的升级批次，该参数返回该值。
-   **STATIC**：静态升级。调用[CreateOTAStaticUpgradeJob](~~147496~~)创建的升级批次，该参数返回该值。 |
|SrcVersions|List|\{"SrcVersion": \["1.0.0"\]\}|待升级版本号列表。 |
|Tags|Array of OtaTagDTO| |升级批次标签。 |
|OtaTagDTO| | | |
|Key|String|key1|标签名。 |
|Value|String|value1|标签值。 |
|TargetSelection|String|SPECIFIC|升级范围。

 -   **ALL**：全量升级。
-   **SPECIFIC**：定向升级。
-   **GRAY**：灰度升级。

 **说明：** 调用[CreateOTADynamicUpgradeJob](~~147887~~)创建的动态升级批次，该参数仅返回ALL。 |
|TimeoutInMinutes|Integer|5|设备升级超时时间，单位：分钟。

 创建升级批次时，设置了超时时间，则返回该参数。 |
|UtcCreate|String|2019-12-28T02:43:10.000Z|批次创建时的时间，UTC格式。 |
|UtcEndTime|String|2019-12-29T02:43:10.000Z|该批次任务执行结束时的时间，UTC格式。

 仅已执行结束的升级批次才返回此参数。 |
|UtcModified|String|2019-12-28T02:43:10.000Z|批次最后一次修改时的时间，UTC格式。 |
|UtcScheduleFinishTime|String|2019-12-30T02:43:10.000Z|定时升级结束的时间。仅定时升级任务且设置了定时升级结束时间时会返回该参数。 |
|UtcScheduleTime|String|2019-12-29T02:43:10.000Z|定时升级发起的时间。仅定时升级任务会返回该参数。 |
|UtcStartTime|String|2019-12-28T02:43:10.000Z|该批次任务开始执行时的时间，UTC格式。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|30F1BB8D-EDBF-44FD-BBC0-BE97DEA73991|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryOTAJob
&JobId=wahVIzGkCMuAUE2gDERM02****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<QueryOTAJobResponse>
  <Data>
        <TimeoutInMinutes>5</TimeoutInMinutes>
        <JobDesc>batch upgrade</JobDesc>
        <UtcStartTime>2019-12-28T02:43:10.000Z</UtcStartTime>
        <UtcEndTime>2019-12-29T02:43:10.000Z</UtcEndTime>
        <ProductKey>a19mzPZ****</ProductKey>
        <UtcModified>2019-12-28T02:43:10.000Z</UtcModified>
        <JobStatus>IN_PROGRESS</JobStatus>
        <UtcCreate>2019-12-28T02:43:10.000Z</UtcCreate>
        <SelectionType>STATIC</SelectionType>
        <TargetSelection>SPECIFIC</TargetSelection>
        <JobType>UPGRADE_FIRMWARE</JobType>
        <RetryInterval>60</RetryInterval>
        <RetryCount>1</RetryCount>
        <OverwriteMode>1</OverwriteMode>
        <MaximumPerMinute>1000</MaximumPerMinute>
        <SrcVersions>
              <SrcVersion>1.0.0</SrcVersion>
        </SrcVersions>
        <Name>firmware2</Name>
        <FirmwareId>UfuxnwygsuSkVE0VCN****0100</FirmwareId>
        <JobId>HvKuBpuk3rdk6E92CP****0200</JobId>
        <DestVersion>1.0.1</DestVersion>
  </Data>
  <RequestId>30F1BB8D-EDBF-44FD-BBC0-BE97DEA73991</RequestId>
  <Success>true</Success>
</QueryOTAJobResponse>
```

`JSON` 格式

```
{
  "Data": {
    "TimeoutInMinutes": 5,
    "JobDesc": "batch upgrade",
    "UtcStartTime": "2019-12-28T02:43:10.000Z",
    "UtcEndTime": "2019-12-29T02:43:10.000Z",
    "ProductKey": "a19mzPZ****",
    "UtcModified": "2019-12-28T02:43:10.000Z",
    "JobStatus": "IN_PROGRESS",
    "UtcCreate": "2019-12-28T02:43:10.000Z",
    "SelectionType": "STATIC",
    "TargetSelection": "SPECIFIC",
    "JobType": "UPGRADE_FIRMWARE",
    "RetryInterval":60,
    "RetryCount":1,
    "OverwriteMode":1,
    "MaximumPerMinute":1000,
    "SrcVersions": {
      "SrcVersion": ["1.0.0"]
    },
    "Name":"firmware2",
    "FirmwareId": "UfuxnwygsuSkVE0VCN****0100",
    "JobId": "HvKuBpuk3rdk6E92CP****0200",
    "DestVersion": "1.0.1"
  },
  "RequestId": "30F1BB8D-EDBF-44FD-BBC0-BE97DEA73991",
  "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/Iot)查看更多错误码。

