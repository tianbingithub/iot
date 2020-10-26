# ListOTAJobByFirmware

调用该接口获取升级包下的升级任务批次列表。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=ListOTAJobByFirmware&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListOTAJobByFirmware|系统规定参数。取值：ListOTAJobByFirmware。 |
|CurrentPage|Integer|是|1|指定从返回结果中的第几页开始显示。页数从1开始排序。 |
|FirmwareId|String|是|FJFx8JzpnhpIsKftRjjm03\*\*\*\*|升级包ID，升级包的唯一标识符。

 升级包ID是调用[CreateOTAFirmware](~~147311~~)创建升级包时，返回的参数之一。

 可以调用[ListOTAFirmware](~~147450~~)，从返回参数中查看。 |
|PageSize|Integer|是|10|指定返回结果中，每页显示的升级批次数量。最大值200。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|CurrentPage|Integer|1|当前页码。 |
|Data|Array of SimpleOTAJobInfo| |调用成功时，返回的批次信息。详情请参见**SimpleOTAJobInfo**下的参数。 |
|SimpleOTAJobInfo| | | |
|FirmwareId|String|UfuxnwygsuSkVE0VCN\*\*\*\*0100|升级包ID。 |
|JobId|String|HvKuBpuk3rdk6E92CP\*\*\*\*0200|升级批次ID，批次的唯一标识符。 |
|JobStatus|String|IN\_PROGRESS|批次的状态。

 -   **PLANNED**：计划中。批次已创建，但是定时时间未到。仅定时静态升级的批次可能返回该值。
-   **IN\_PROGRESS**：执行中。
-   **COMPLETED**：已完成。
-   **CANCELED**：已取消。 |
|JobType|String|UPGRADE\_FIRMWARE|批次类型。

 -   **VERFIY\_FIRMWARE**：升级包验证批次。
-   **UPGRADE\_FIRMWARE**：批量升级批次。 |
|ProductKey|String|a19mzPZ\*\*\*\*|升级包所属产品的唯一标识。 |
|SelectionType|String|STATIC|升级策略。

 -   **DYNAMIC**：动态升级。调用[CreateOTADynamicUpgradeJob](~~147887~~)创建的升级批次，该参数返回该值。
-   **STATIC**：静态升级。调用[CreateOTAStaticUpgradeJob](~~147496~~)创建的升级批次，该参数返回该值。 |
|Tags|Array of OtaTagDTO| |升级批次标签。 |
|OtaTagDTO| | | |
|Key|String|key1|标签名。 |
|Value|String|value1|标签值。 |
|TargetSelection|String|SPECIFIC|升级范围。

 -   **ALL**：全量升级。
-   **SPECIFIC**：定向升级。
-   **GRAY**：灰度升级。

 **说明：** 调用[CreateOTADynamicUpgradeJob](~~147887~~)创建的动态升级批次，该参数仅返回ALL。 |
|UtcCreate|String|2019-12-28T02:43:10.000Z|批次创建时的时间，UTC格式。 |
|UtcEndTime|String|2019-12-29T02:43:10.000Z|该批次任务执行结束时的时间，UTC格式。

 仅已执行结束的升级批次才返回此参数。 |
|UtcModified|String|2019-12-28T02:43:10.000Z|批次最后一次修改时的时间，UTC格式。 |
|UtcStartTime|String|2019-12-28T02:43:10.000Z|该批次任务开始执行时的时间，UTC格式。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|PageCount|Integer|1|总页数。 |
|PageSize|Integer|10|每页显示的设备升级作业数量。 |
|RequestId|String|5D58AC86-D5BF-4B39-834E-913E7F2C985D|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |
|Total|Integer|1|设备升级作业数量总计。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListOTAJobByFirmware
&FirmwareId=FJFx8JzpnhpIsKftRjjm03****
&PageSize=10
&CurrentPage=1
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ListOTAJobByFirmwareResponse>
    <PageCount>1</PageCount>
    <Data>
        <SimpleOTAJobInfo>
            <SelectionType>STATIC</SelectionType>
            <TargetSelection>SPECIFIC</TargetSelection>
            <JobType>UPGRADE_FIRMWARE</JobType>
            <FirmwareId>yLYuCqfqQNQ1JOqkDa****0100</FirmwareId>
            <UtcStartTime>2019-12-28T02:43:10.000Z</UtcStartTime>
            <UtcEndTime>2019-12-29T02:43:10.000Z</UtcEndTime>
            <ProductKey>a19mzPZ****</ProductKey>
            <JobId>HvKuBpuk3rdk6E92CP****0200</JobId>
            <UtcModified>2019-12-28T02:43:10.000Z</UtcModified>
            <JobStatus>IN_PROGRESS</JobStatus>
            <UtcCreate>2019-12-28T02:43:10.000Z</UtcCreate>
        </SimpleOTAJobInfo>
    </Data>
    <PageSize>10</PageSize>
    <RequestId>5D58AC86-D5BF-4B39-834E-913E7F2C985D</RequestId>
    <CurrentPage>1</CurrentPage>
    <Success>true</Success>
    <Total>1</Total>
</ListOTAJobByFirmwareResponse>
```

`JSON` 格式

```
{
  "PageCount": 1,
  "Data": {
    "SimpleOTAJobInfo": [{
      "SelectionType": "STATIC",
      "TargetSelection": "SPECIFIC",
      "JobType": "UPGRADE_FIRMWARE",
      "FirmwareId": "UfuxnwygsuSkVE0VCN****0100",
      "UtcStartTime": "2019-12-28T02:43:10.000Z",
      "UtcEndTime": "2019-12-29T02:43:10.000Z",
      "ProductKey": "a19mzPZ****",
      "JobId": "HvKuBpuk3rdk6E92CP****0200",
      "UtcModified": "2019-12-28T02:43:10.000Z",
      "JobStatus": "IN_PROGRESS",
      "UtcCreate": "2019-12-28T02:43:10.000Z"
    }]
  },
  "PageSize": 10,
  "RequestId": "5D58AC86-D5BF-4B39-834E-913E7F2C985D",
  "CurrentPage": 1,
  "Success": true,
  "Total": 1
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/Iot)查看更多错误码。

