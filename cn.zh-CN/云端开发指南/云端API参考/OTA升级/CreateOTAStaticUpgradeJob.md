# CreateOTAStaticUpgradeJob

调用该接口创建静态升级批次。

## 限制说明

-   若未在调用[CreateOTAFirmware](~~147311~~)创建升级包时指定升级包可以不通过验证，则调用本接口创建批量升级批次前，必须保证升级包已验证成功。创建验证升级包任务，请参见[CreateOTAVerifyJob](~~147480~~)。
-   单次调用，对于定向升级，若直接传入设备名称，则最多可对200个设备发起升级任务；若使用待升级设备列表文件，则最多可对10,000个设备发起升级任务，需提前调用[GenerateDeviceNameListURL](~~186062~~)生成文件URL，并按说明上传设备列表文件。
-   同一设备只能同时在一个升级批次中处于待升级或正在升级状态。对处于待升级或正在升级状态的设备发起新的升级任务，后发起的任务会直接失败。
-   可以对单个升级包，同时发起多个静态升级批次。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为20。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=CreateOTAStaticUpgradeJob&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateOTAStaticUpgradeJob|系统规定参数。取值：CreateOTAStaticUpgradeJob。 |
|FirmwareId|String|是|nx3xxVvFdwvn6dim50PY03\*\*\*\*|升级包ID，升级包的唯一标识符。

 升级包ID是调用[CreateOTAFirmware](~~147311~~)创建升级包时，返回的参数之一。

 可以调用[ListOTAFirmware](~~147450~~)，从返回参数中查看。 |
|ProductKey|String|是|a1Le6d0\*\*\*\*|升级包所属产品的ProductKey。 |
|Tag.N.Key|String|是|key1|批次标签key。仅支持英文字母、数字、点号（.）。长度限制1~30个字符。批次标签将在向设备推送升级通知时下发给设备。

 **说明：** 批次标签可以不传入。如果传入，**Tag.N.Value**与**Tag.N.Key**必须成对传入。 |
|Tag.N.Value|String|是|value1|批次标签value。长度限制为1~128个字符。

 **说明：** 批次标签可以不传入。如果传入，**Tag.N.Value**与**Tag.N.Key**必须成对传入。 |
|TargetSelection|String|是|ALL|升级范围。

 -   **ALL**：全量升级。
-   **SPECIFIC**：定向升级。
-   **GRAY**：灰度升级。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|SrcVersion.N|RepeatList|否|V1.0.1|待升级版本号列表。

 **说明：**

-   发起全量升级（`TargetSelection=ALL`）和灰度升级（`TargetSelection=GRAY`）任务时，可以传入该参数。
-   使用差分升级包发起全量升级和灰度升级任务时，该参数值需指定为差分升级包的待升级版本号（**SrcVersion**）。
-   发起定向升级（`TargetSelection=SPECIFIC`）任务时，不能传入该参数。
-   可以调用[QueryDeviceDetail](~~69594~~)，查看设备OTA模块版本号（**FirmwareVersion**）。
-   列表中不能有重复的版本号。
-   最多可传入10个版本号。 |
|ScheduleTime|Long|否|1577808000000|指定发起OTA升级的时间。

 定时时间范围需为当前时间的5分钟后至7天内。取值为13位毫秒值时间戳。

 不传入该参数，则表示立即升级。 |
|RetryInterval|Integer|否|60|设备升级失败后，自动重试的时间间隔，单位：分钟。可选值：

 -   **0**：立即重试。
-   **10**：10分钟后重试。
-   **30**：30分钟后重试。
-   **60**：60分钟（即1小时）后重试。
-   **1440**：1,440分钟（即24小时）后重试。

 不传入此参数，则表示不重试。 |
|RetryCount|Integer|否|1|自动重试次数。

 如果传入**RetryInterval**参数，则需传入该参数。

 可选值：

 -   **1**：1次。
-   **2**：2次。
-   **5**：5次。 |
|TimeoutInMinutes|Integer|否|1440|设备升级超时时间。单位：分钟，取值范围：1~1,440。

 超过指定时间后，设备未完成升级，则升级失败。

 **说明：**

-   从设备首次上报进度开始计算时间。
-   因超时而导致的升级失败，云端不会触发自动重试逻辑。 |
|MaximumPerMinute|Integer|否|1000|每分钟最多向多少个设备推送升级包下载URL。取值范围：0~1,000。

 不传入该参数，则取默认值1,000。 |
|GrayPercent|String|否|33.33|设置灰度比例。取值为字符串格式的百分比，小数点后最多3位小数，系统计算结果向下取整。灰度升级的设备至少为1个。

 例如有100个待升级设备，设置灰度升级的灰度比例为33.33，则系统计算结果为33。

 升级范围指定为灰度升级（`TargetSelection=GRAY`）时，需传入此参数。 |
|TargetDeviceName.N|RepeatList|否|deviceName1|定向升级的设备名称列表。

 **说明：**

-   发起定向升级（`TargetSelection=SPECIFIC`）任务时，需传入该参数或**DnListFileUrl**，不可同时传入。
-   使用差分升级包进行定向升级时，要升级的设备的当前OTA模块版本号需与差分升级包的待升级版本号（**SrcVersion**）相同。
-   可以调用[QueryDeviceDetail](~~69594~~)，查看设备OTA模块版本号（**FirmwareVersion**）。
-   列表中的设备所属的产品必须与升级包所属产品一致。
-   列表中不能有重复的设备名称。
-   最多可传入100,000个设备名称。 |
|ScheduleFinishTime|Long|否|1577909000000|指定结束升级的时间。

 结束时间距发起时间（**ScheduleTime**）最少1小时，最多为30天。取值为13位毫秒值时间戳。

 不传入该参数，则表示不会强制结束升级。 |
|OverwriteMode|Integer|否|1|是否覆盖之前的升级任务。取值：

 -   1：不覆盖。若设备已有升级任务，则只执行已有任务。
-   2：覆盖。设备只执行新的升级任务。

 不传入该参数，则默认不覆盖。

 **说明：** 不覆盖升级中的任务。 |
|DnListFileUrl|String|否|https://iotx-ota.oss-cn-shanghai.aliyuncs.com/ota/65dfcda0473be29836dfde585472\*\*\*\*/ck2nfzljo00023g7kysg0\*\*\*\*.bin|定向升级设备列表文件的URL。

 **说明：**

-   发起定向升级（`TargetSelection=SPECIFIC`）任务时，需传入该参数或**TargetDeviceName.N**，不可同时传入。
-   请调用[GenerateDeviceNameListURL](~~186062~~)生成文件URL，并按说明上传设备列表文件。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|MissingFirmwareId|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Struct| |调用成功时，返回的升级批次信息。详情见以下Data所包含的参数。 |
|JobId|String|wahVIzGkCMuAUE2gDERM02\*\*\*\*|升级批次ID，升级批次的唯一标识符。 |
|UtcCreate|String|2019-11-04T06:22:19.566Z|升级批次的创建时间，UTC格式。 |
|ErrorMessage|String|FirmwareId is mandatory for this action.|调用失败时，返回的出错信息。 |
|RequestId|String|29EC7245-0FA4-4BB6-B4F5-5F04818FDFB1|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 示例

请求示例

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
&<公共请求参数>
```

正常返回示例

`XML` 格式

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

`JSON` 格式

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

