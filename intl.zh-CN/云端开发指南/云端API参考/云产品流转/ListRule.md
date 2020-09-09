# ListRule

调用该接口分页查询所有规则列表。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为20。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=ListRule&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListRule|系统规定参数。取值：ListRule。 |
|CurrentPage|Integer|是|1|显示返回结果中的第几页。最大取值 1000，默认值 1。 |
|PageSize|Integer|是|2|返回结果中每页显示的记录数量。最大取值100，默认值是10。 |
|IotInstanceId|String|否|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|公共实例不传此参数；您购买的实例需传入实例ID。 |
|ResourceGroupId|String|否|rg-acfmxazb4ph\*\*\*\*|规则所属资源组ID。 可在[资源管理控制台](https://resourcemanager.console.aliyun.com/resource-groups)查看资源组信息。

 若不传入此参数，则查询账号下所有资源组中的规则。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Array of RuleInfo| |调用成功时，返回的规则信息列表。详情参见下表RuleInfo。

 **说明：** 返回规则信息按照规则创建时间倒序排列。 |
|RuleInfo| | | |
|CreateUserId|Long|1231579085000000|创建该规则的用户ID。 |
|Created|String|Wed Feb 27 20:45:43 CST 2019|该规则创建时的CST（Central Standard Time）时间。 |
|DataType|String|JSON|该规则的数据类型，取值：**JSON**和**BINARY**。 |
|Id|Long|151454|规则ID。 |
|Modified|String|Wed Feb 27 20:45:43 CST 2019|该规则最近一次被修改时的CST （Central Standard Time）时间。 |
|Name|String|test123|规则名称。 |
|ProductKey|String|a1KiV\*\*\*\*\*\*|应用该规则的产品Key。 |
|RuleDesc|String|rule1Desc|规则的描述信息。 |
|Select|String|deviceName\(\) as deviceName|该规则SQL语句中的**Select**内容。 |
|ShortTopic|String|+/thing/event/property/post|应用该规则的具体Topic（不包含ProductKey类目），格式为：`${deviceName}/topicShortName`。其中，$\{deviceName\}是具体设备的名称，topicShortName是Topic余下部分。

 **说明：** 若Topic包含通配符`+`或`#`，请参见[Topic通配符说明](~~73731~~)。 |
|Status|String|STOP|该规则的运行状态。取值：

 -   **RUNNING**：运行中
-   **STOP**：停止 |
|Topic|String|/a1T27vz\*\*\*\*/+/thing/event/property/post|应用该规则的具体Topic，格式为：`${productKey}/${deviceName}/topicShortName`。

 **说明：** 若Topic包含通配符`+`或`#`，请参见[Topic通配符说明](~~73731~~)。 |
|UtcCreated|String|2019-02-27T12:45:43.000Z|该规则最近一次被修改时的UTC时间。 |
|UtcModified|String|2019-02-27T12:45:43.000Z|该规则创建时的UTC时间。 |
|Where|String|Temperature\>35|该规则SQL语句中的**Where**查询条件。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|Page|Integer|1|当前页码。 |
|PageSize|Integer|2|每页显示的记录数。 |
|RequestId|String|1564B626-DE97-452D-9E9B-305888AC6105|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |
|Total|Integer|25|总页数。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListRule
&PageSize=2
&CurrentPage=1
&ResourceGroupId=rg-acfmxazb4ph****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ListRuleResponse>
  <Data>
        <RuleInfo>
              <DataType>JSON</DataType>
              <ProductKey>a1T27vz****</ProductKey>
              <CreateUserId>198426864326****</CreateUserId>
              <UtcModified>2020-02-26T06:38:27.000Z</UtcModified>
              <UtcCreated>2020-02-26T02:50:44.000Z</UtcCreated>
              <Where>Temperature&gt;35</Where>
              <Name>testrule2</Name>
              <Status>STOP</Status>
              <Select>deviceName() as DeviceName</Select>
              <Created>Wed Feb 26 10:50:44 CST 2020</Created>
              <Modified>Wed Feb 26 14:38:27 CST 2020</Modified>
              <Topic>/a1T27vz****/+/thing/event/property/post</Topic>
              <Id>497350</Id>
        </RuleInfo>
  </Data>
  <Page>1</Page>
  <PageSize>1</PageSize>
  <RequestId>1A6131EC-7504-4673-B997-DEFC6B363A37</RequestId>
  <Success>true</Success>
  <Total>5</Total>
</ListRuleResponse>
```

`JSON` 格式

```
{
	"Data": {
		"RuleInfo": [
			{
				"DataType": "JSON",
				"ProductKey": "a1T27vz****",
				"CreateUserId": "198426864326****",
				"UtcModified": "2020-02-26T06:38:27.000Z",
				"UtcCreated": "2020-02-26T02:50:44.000Z",
				"Where": "Temperature>35",
				"Name": "testrule2",
				"Status": "STOP",
				"Select": "deviceName() as DeviceName",
				"Created": "Wed Feb 26 10:50:44 CST 2020",
				"Modified": "Wed Feb 26 14:38:27 CST 2020",
				"Topic": "/a1T27vz****/+/thing/event/property/post",
				"Id": 497350
			}
		]
	},
	"Page": 1,
	"PageSize": 1,
	"RequestId": "1A6131EC-7504-4673-B997-DEFC6B363A37",
	"Success": true,
	"Total": 5
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/Iot)查看更多错误码。

