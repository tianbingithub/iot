# ListRuleActions

调用该接口查询指定规则下的所有转发数据动作列表。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为50。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=ListRuleActions&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListRuleActions|系统规定参数。取值：ListRuleActions。 |
|RuleId|Long|是|10000|要查询的规则ID。可在物联网平台控制台对应实例下，**规则引擎**\>**云产品流转**页查看规则ID，或调用[ListRule](~~69486~~)从返回结果中查看。 |
|IotInstanceId|String|否|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|共享实例不传此参数；仅您购买的实例需传入实例ID。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|22254BDB-3DC1-4643-8D1B-EE0437EF09A9|阿里云为该请求生成的唯一标识符。 |
|RuleActionList|Array of RuleActionInfo| |调用成功时，返回的规则动作信息列表。详情请参见以下**RuleActionInfo**所包含的参数。 |
|RuleActionInfo| | | |
|Configuration|String|\{\\"endPoint\\":\\"http://ShanghaiRegion.cn-shanghai.ots.aliyuncs.com\\",\\"instanceName\\":\\"ShanghaiRegion\\",\\"primaryKeys\\":\[\{\\"columnName\\":\\"temperature\\",\\"columnType\\":\\"INTEGER\\",\\"columnValue\\":\\"$\{deviceName\}\\"\}\],\\"regionName\\":\\"cn-shanghai\\",\\"role\\":\{\\"roleArn\\":\\"acs:ram::1231579085\*\*\*\*\*\*:role/aliyuniotaccessingotsrole\\",\\"roleName\\":\\"AliyunIOTAccessingOTSRole\\"\},\\"tableName\\":\\"iottest\\",\\"uid\\":\\"1231579085\*\*\*\*\*\*\\"\}|该规则动作的配置信息。 |
|ErrorActionFlag|Boolean|false|该规则动作是否为转发错误操作数据的转发动作，即转发流转到其他云产品失败且重试失败的数据。

 -   **true**：该规则动作转发错误操作数据。
-   **false**：该规则动作不转发错误操作数据，而是正常转发操作。 |
|Id|Long|139099|规则动作ID。 |
|RuleId|Long|10000|该规则动作对应的规则ID。 |
|Type|String|OTS|规则动作类型。取值：

 -   **REPUBLISH**：转发到另一个topic。
-   **OTS**：存储到表格存储。
-   **MNS**：发送消息到消息服务。
-   **ONS**：发送数据到消息队列。
-   **TSDB**：存储到高性能时间序列数据库。
-   **FC**：发送数据到函数计算。
-   **DATAHUB**：发送数据到DataHub中。
-   **RDS**：存储数据到云数据库中。
-   **AMQP**：数据流转到AMQP消费组。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListRuleActions
&RuleId=10000
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ListRuleActionsResponse>
      <RuleActionList>
            <RuleActionInfo>
                  <Type>REPUBLISH</Type>
                  <RuleId>152323</RuleId>
                  <Id>142401</Id>
                  <Configuration>
                        <topic>/sys/a1zSA28HUyy/device/thing/service/property/set</topic>
                        <topicType>0</topicType>
                        <uid>1231579*******</uid>
                  </Configuration>
                  <ErrorActionFlag>false</ErrorActionFlag>
            </RuleActionInfo>
      </RuleActionList>
      <RequestId>22254BDB-3DC1-4643-8D1B-EE0437EF09A9</RequestId>
      <Success>true</Success>
</ListRuleActionsResponse>
```

`JSON` 格式

```
{
  "RuleActionList": {
    "RuleActionInfo": [
      {
        "Type": "OTS", 
        "RuleId": 10000, 
        "Id": 139099, 
        "Configuration": "{\"endPoint\":\"http://ShanghaiRegion.cn-shanghai.ots.aliyuncs.com\",\"instanceName\":\"ShanghaiRegion\",\"primaryKeys\":[{\"columnName\":\"temperature\",\"columnType\":\"INTEGER\",\"columnValue\":\"${deviceName}\"}],\"regionName\":\"cn-shanghai\",\"role\":{\"roleArn\":\"acs:ram::1231579085******:role/aliyuniotaccessingotsrole\",\"roleName\":\"AliyunIOTAccessingOTSRole\"},\"tableName\":\"iottest\",\"uid\":\"1231579085******\"}", 
        "ErrorActionFlag": false
      }, 
      {
        "Type": "REPUBLISH", 
        "RuleId": 152323, 
        "Id": 142401, 
        "Configuration": "{\"topic\":\"/sys/a1zSA28H***/device/thing/service/property/set\",\"topicType\":0,\"uid\":\"1231579085******\"}", 
        "ErrorActionFlag": false
      }
    ]
  }, 
  "RequestId": "22254BDB-3DC1-4643-8D1B-EE0437EF09A9", 
  "Success": true
}
```

