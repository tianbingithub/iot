# QueryDevicePropertyData

调用该接口查询指定设备的属性记录。

## 限制说明

-   仅能查询最近30天内的属性数据。

**说明：** 数据存储时间从属性时间戳表示的时间当日开始计算。

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为50。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=QueryDevicePropertyData&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|QueryDevicePropertyData|系统规定参数。取值：QueryDevicePropertyData。 |
|Asc|Integer|是|0|返回结果中属性记录的排序方式，取值：

 -   **0**：倒序。
-   **1**：正序。 |
|EndTime|Long|是|1579249499000|要查询的属性记录的结束时间。取值为毫秒值时间戳，例如1579249499000。 |
|Identifier|String|是|temperature|要查询的属性标识符。

 若设备有多个属性，您可以多次调用该接口进行查询，一次输入一个Identifier。

 设备的属性Identifier，可在控制台中设备所属的产品的功能定义中查看，或调用[QueryThingModel](~~150321~~)，从返回的物模型数据中查看。 |
|PageSize|Integer|是|10|返回结果中每页显示的记录数。数量限制：每页最多可显示50条。 |
|StartTime|Long|是|1579249499000|要查询的属性记录的开始时间。取值为毫秒值时间戳，例如1579249499000。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|IotId|String|否|Q7uOhVRdZRRlDnTLv\*\*\*\*00100|要查询的设备ID。物联网平台为该设备颁发的ID，设备的唯一标识符。

 **说明：** 如果传入该参数，则无需传入**ProductKey**和**DeviceName**。**IotId**作为设备唯一标识符，和**ProductKey**与**DeviceName**组合是一一对应的关系。如果您同时传入**IotId**和**ProductKey**与**DeviceName**组合，则以**IotId**为准。 |
|ProductKey|String|否|a1BwAGV\*\*\*\*|要查询设备所隶属的产品ProductKey。

 **说明：** 如果传入该参数，需同时传入**DeviceName**。 |
|DeviceName|String|否|airconditioning|要查询设备的名称。

 **说明：** 如果传入该参数，需同时传入**ProductKey**。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见 [公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Struct| |调用成功时，返回的设备属性记录。 |
|List|Array of PropertyInfo| |属性集合。每个元素代表一个属性。 |
|PropertyInfo| | | |
|Time|String|1516541885630|属性修改时间。 |
|Value|String|2|属性值。 |
|NextTime|Long|1579249499000|下一页面中的属性记录的起始时间。

 -   当属性记录的排序方式为倒序（入参**Asc**为**0**），调用本接口查询下一页属性记录时，该值可作为下次查询的入参**EndTime**的值。
-   当属性记录的排序方式为正序（入参**Asc**为**1**），调用本接口查询下一页属性记录时，该值可作为下次查询的入参**StartTime**的值。 |
|NextValid|Boolean|true|是否有下一页属性记录。

 -   **true**：有。
-   **false**：没有。 |
|ErrorMessage|String|系统异常|调用失败时返回的出错信息。 |
|RequestId|String|E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|表示是否调用成功。

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDevicePropertyData
&ProductKey=a1BwAGV****
&DeviceName=device1
&Identifier=lightLevel
&StartTime=1516538300303
&EndTime=1516541900303
&PageSize=10
&Asc=1
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<QueryDevicePropertyDataResponse>
  <Data>
        <NextValid>false</NextValid>
        <NextTime>1579249151177</NextTime>
        <List>
              <PropertyInfo>
                    <Value>32.46</Value>
                    <Time>1579249151178</Time>
              </PropertyInfo>
        </List>
  </Data>
  <RequestId>45391E10-446B-4986-863E-1BA8CC44748F</RequestId>
  <Success>true</Success>
</QueryDevicePropertyDataResponse>
```

`JSON` 格式

```
{
  "Data": {
    "NextValid": false, 
    "NextTime": 1579249151177, 
    "List": {
      "PropertyInfo": [
        {
          "Value": "32.46", 
          "Time": 1579249151178
        }
      ]
    }
  }, 
  "RequestId": "45391E10-446B-4986-863E-1BA8CC44748F", 
  "Success": true
}
```

