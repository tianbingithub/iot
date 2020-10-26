# ListOTAJobByDevice

调用该接口获取设备所在的升级包升级批次列表。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=ListOTAJobByDevice&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListOTAJobByDevice|系统规定参数。取值：ListOTAJobByDevice。 |
|CurrentPage|Integer|是|1|显示返回结果的第几页。返回结果页数从1开始排序。 |
|DeviceName|String|是|light1|设备名称。 |
|FirmwareId|String|是|FJFx8JzpnhpIsKftRjjm03\*\*\*\*|升级包ID。升级包的唯一标识符。

 升级包ID是调用[CreateOTAFirmware](~~147311~~)创建升级包时，返回的参数之一。可以调用[ListOTAFirmware](~~147450~~)，从返回参数中查看。 |
|PageSize|Integer|是|10|指定返回结果中每页显示的升级包数量。最大限制为100。 |
|ProductKey|String|是|a19mzPZ\*\*\*\*|设备所属产品的ProductKey。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|MissingFirmwareId|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|CurrentPage|Integer|1|当前页码。 |
|Data|Array of SimpleOTAJobInfo| |调用成功时，返回的升级批次信息。详情请参见以下**SimpleOTAJobInfo**。 |
|SimpleOTAJobInfo| | | |
|FirmwareId|String|FJFx8JzpnhpIsKftRjjm03\*\*\*\*|升级包ID。 |
|JobId|String|HvKuBpuk3rdk6E92CP\*\*\*\*|升级任务批次ID。 |
|JobStatus|String|COMPLETED|升级任务批次的状态。

 -   **IN\_PROGRESS**：执行中。
-   **COMPLETED**：已完成。
-   **CANCELED**：已取消。 |
|JobType|String|UPGRADE\_FIRMWARE|批次类型。

 -   **VERIFY\_FIRMWARE**：升级包验证批次。
-   **UPGRADE\_FIRMWARE**：批量升级批次。 |
|ProductKey|String|a19mzPZ\*\*\*\*|升级包所属产品的唯一标识。 |
|SelectionType|String|STATIC|升级策略。

 -   DYNAMIC：动态升级。调用[CreateOTADynamicUpgradeJob](~~147887~~)创建的升级批次，该参数返回该值。
-   STATIC：静态升级。调用[CreateOTAStaticUpgradeJob](~~147496~~)创建的升级批次，该参数返回该值。 |
|Tags|Array of OtaTagDTO| |升级批次标签。 |
|OtaTagDTO| | | |
|Key|String|key1|标签名。 |
|Value|String|value1|标签值。 |
|TargetSelection|String|ALL|升级范围。

 -   **ALL**：全量升级。
-   **SPECIFIC**：定向升级。
-   **GRAY**：灰度升级。

 **说明：** 调用[CreateOTADynamicUpgradeJob](~~147887~~)创建的动态升级批次，该参数仅返回ALL。 |
|UtcCreate|String|2019-12-28T02:43:10.000Z|批次创建时的时间，UTC格式。 |
|UtcEndTime|String|2019-12-29T02:43:10.000Z|该批次任务执行结束时的时间，UTC格式。

 **说明：** 仅已执行结束的升级批次才返回此参数。 |
|UtcModified|String|2019-12-29T02:43:10.000Z|批次最后一次修改时的时间，UTC格式。 |
|UtcStartTime|String|2019-12-29T02:43:10.000Z|该批次任务开始执行时的时间，UTC格式。 |
|ErrorMessage|String|FirmwareId is mandatory for this action|调用失败时，返回的出错信息。 |
|PageCount|Integer|1|总页数。 |
|PageSize|Integer|10|每页显示的升级包数量。 |
|RequestId|String|A01829CE-75A1-4920-B775-921146A1AB79|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |
|Total|Integer|1|升级包数量总计。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListOTAJobByDevice
&FirmwareId=FJFx8JzpnhpIsKftRjjm03****
&ProductKey=a19mzPZ****
&DeviceName=light
&PageSize=10
&CurrentPage=1
&公共请求参数
```

正常返回示例

`XML` 格式

```
<ListOTAJobByDeviceResponse>
  <PageCount>1</PageCount>
  <Data>
        <SimpleOTAJobInfo>
              <SelectionType>STATIC</SelectionType>
              <TargetSelection>SPECIFIC</TargetSelection>
              <JobType>UPGRADE_FIRMWARE</JobType>
              <FirmwareId>FJFx8JzpnhpIsKftRjjm03****</FirmwareId>
              <UtcStartTime>2019-12-28T02:43:10.000Z</UtcStartTime>
              <ProductKey>a19mzPZ****</ProductKey>
              <JobId>HvKuBpuk3rdk6E92CPQN02****</JobId>
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
</ListOTAJobByDeviceResponse>
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
      "FirmwareId": "FJFx8JzpnhpIsKftRjjm03****",
      "UtcStartTime": "2019-12-28T02:43:10.000Z",
      "ProductKey": "a19mzPZ****",
      "JobId": "HvKuBpuk3rdk6E92CPQN02****",
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

