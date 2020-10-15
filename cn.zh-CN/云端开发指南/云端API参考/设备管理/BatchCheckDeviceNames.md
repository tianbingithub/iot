# BatchCheckDeviceNames

调用该接口在指定产品下批量自定义设备名称。物联网平台将检查名称的合法性。

## 接口使用说明

该接口和**BatchRegisterDeviceWithApplyId**接口结合使用，在一个产品下批量注册多个设备，并且为每个设备单独命名。

批量注册设备流程：

1. 调用本接口，传入要批量注册的设备的名称，物联网平台返回申请批次ID（**ApplyId**）。返回成功结果，表示批量校验设备名称的申请已经提交成功。实际的校验是异步执行的，会有一个过程。

2. 调用[QueryBatchRegisterDeviceStatus](~~69483~~)查看名称设置结果。

3. 调用[BatchRegisterDeviceWithApplyId](~~69514~~)批量注册设备。

4. （可选）调用[QueryBatchRegisterDeviceStatus](~~69483~~)查看设备注册结果。

5. 调用[QueryPageByApplyId](~~69518~~)查看批量注册的设备信息。

## 限制说明

-   单次调用，最多能定义1,000 个设备名称。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=BatchCheckDeviceNames&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|BatchCheckDeviceNames|系统规定参数。取值：BatchCheckDeviceNames。 |
|ProductKey|String|是|a1BwAGV\*\*\*\*|要注册的设备所属的产品ProductKey。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|DeviceName.N|RepeatList|否|light|要注册的设备名称。设备名称在产品内具有唯一性。支持英文字母、数字、短划线（-）、下划线（\_）、at符号（@）、点号（.）和英文冒号（:），长度限制为4~32个字符。

 该参数与**DeviceNameList.N.DeviceName**必须传入一种。若您同时传入该参数与**DeviceNameList.N.DeviceName**，则以**DeviceNameList.N.DeviceName**为准。

 **说明：** 单次调用，最多能传入1,000个设备名称。 |
|DeviceNameList.N.DeviceName|String|否|light1|要注册的设备名称。设备名称在产品内具有唯一性。支持英文字母、数字、短划线（-）、下划线（\_）、at符号（@）、点号（.）和英文冒号（:），长度限制为4~32个字符。

 该参数与**DeviceName.N**必须传入一种。若您同时传入该参数与**DeviceName.N**，则以该参数为准。

 **说明：** 单次调用，最多能传入1,000个设备名称。 |
|DeviceNameList.N.DeviceNickname|String|否|智能灯1|要注册的设备的备注名称。支持中文、英文字母、日文、数字和下划线（\_），备注名称长度为4~64个字符，一个中文或日文占2个字符。

 若传入该参数，则必须同时传入**DeviceNameList.N.DeviceName**。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见 [公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Struct| |返回的数据。 |
|ApplyId|Long|1295006|调用成功时，系统返回的申请批次ID。使用该ApplyId，调用[BatchRegisterDeviceWithApplyId](~~69514~~)接口来批量创建设备。 |
|InvalidDeviceNameList|List|\{ "InvalidDeviceName": \[ "APT$", "aw" \] \}|调用失败时，返回的不合法设备名称列表。 |
|InvalidDeviceNicknameList|List|\{ "InvalidDeviceNickname": \[ "APT$", "aw" \] \}|调用失败时，返回的不合法设备备注名称列表。 |
|ErrorMessage|String|系统异常|调用失败时返回的出错信息。 |
|RequestId|String|E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|表示是否调用成功。

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchCheckDeviceNames
&productKey=a1BwAGV****
&DeviceNameList.1.DeviceName=light1
&DeviceNameList.2.DeviceName=light2
&DeviceNameList.3.DeviceName=light3
&DeviceNameList.3.DeviceNickname=智能灯3
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<BatchCheckDeviceNamesResponse>
<Data>
    <ApplyId>1234567</ApplyId>
</Data>
<RequestId>E976E36B-6874-4FA4-8BC0-55F9BEC5E2EF</RequestId>
<Success>true</Success>
<BatchCheckDeviceNamesResponse>
```

`JSON` 格式

```
{
	"Data": {
		"ApplyId": 1234567
	},
	"RequestId": "E976E36B-6874-4FA4-8BC0-55F9BEC5E2EF",
	"Success": true
}
```

