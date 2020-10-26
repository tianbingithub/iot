# CreateOTADynamicUpgradeJob

调用该接口创建动态升级任务批次。

## 限制说明

-   若未在调用[CreateOTAFirmware](~~147311~~)创建升级包时指定升级包可以不通过验证，则调用本接口创建批量升级批次前，必须保证升级包已验证成功。创建验证升级包任务，请参见[CreateOTAVerifyJob](~~147480~~)。
-   同一设备只能同时在一个升级批次中处于待升级或正在升级状态。对处于待升级或正在升级状态的设备发起新的升级任务，后发起的任务会直接失败。
-   同一升级包下，只能有一个状态为执行中的动态升级批次。
-   如果同一个设备被包含在不同升级包的动态升级策略中，则设备执行最新发起的动态升级。
-   创建动态升级批次后，系统将自动创建对应的动态升级策略。可以调用[CancelOTAStrategyByJob](~~147905~~)取消动态升级策略。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为20。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=CreateOTADynamicUpgradeJob&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateOTADynamicUpgradeJob|系统规定参数。取值：CreateOTADynamicUpgradeJob。 |
|FirmwareId|String|是|nx3xxVvFdwvn6dim50PY03\*\*\*\*|升级包ID，升级包的唯一标识符。

 升级包ID是调用[CreateOTAFirmware](~~147311~~)创建升级包时，返回的参数之一。

 可以调用[ListOTAFirmware](~~147450~~)，从返回参数中查看。 |
|ProductKey|String|是|a1Le6d0\*\*\*\*|升级包所属产品的ProductKey。 |
|Tag.N.Key|String|是|key1|批次标签key。仅支持英文字母、数字、点号（.）。长度限制1~30个字符。批次标签将在向设备推送升级通知时下发给设备。

 **说明：** 批次标签可以不传入。如果传入，**Tag.N.Value**与**Tag.N.Key**必须成对传入。 |
|Tag.N.Value|String|是|value1|批次标签value。长度限制为1~128个字符。

 **说明：** 批次标签可以不传入。如果传入，**Tag.N.Value**与**Tag.N.Key**必须成对传入。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|SrcVersion.N|RepeatList|否|V1.0.1|待升级版本号列表。

 **说明：**

-   对差分升级包发起动态升级任务时，该参数值必须与差分升级包的待升级版本号（**SrcVersion**）相同。
-   可以调用[QueryDeviceDetail](~~69594~~)，查看设备OTA模块版本号**FirmwareVersion**。
-   列表中不能有重复的版本号。
-   最多可传入10个版本号。 |
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
-   因超时而导致的升级失败，不会触发自动重试逻辑。 |
|MaximumPerMinute|Integer|否|1000|每分钟最多向多少个设备推升级包下载URL。取值范围：0~1,000。

 不传入该参数，则取默认值1,000。 |
|OverwriteMode|Integer|否|2|是否覆盖之前的升级任务。取值：

 -   **1**：不覆盖。若设备已有升级任务，则只执行已有任务。
-   **2**：覆盖。设备只执行新的升级任务。

 不传入该参数，则默认不覆盖。

 **说明：** 不覆盖升级中的任务。 |
|DynamicMode|Integer|否|1|动态升级模式。取值范围：

 -   **1**：除了升级当前满足升级条件的设备，还将持续检查设备是否满足升级条件，对满足升级条件的设备进行升级。
-   **2**：仅对后续上报新版本号的设备生效。

 不传入该参数，则默认为1。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Struct| |调用成功时，返回的升级批次信息。详情见以下Data包含的参数。 |
|JobId|String|XUbmsMHmkqv0PiAG\*\*\*\*010001|升级批次ID，升级批次的唯一标识符。 |
|UtcCreate|String|2019-05-10T02:18:53.000Z|升级批次的创建时间，UTC格式。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|9F41D14E-CB5F-4CCE-939C-057F39E688F5|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 示例

请求示例

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
&<公共请求参数>
```

正常返回示例

`XML` 格式

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

`JSON` 格式

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

