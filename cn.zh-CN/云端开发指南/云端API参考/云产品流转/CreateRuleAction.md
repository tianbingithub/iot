# CreateRuleAction

调用该接口在指定的规则下创建一个规则动作，定义将处理后的Topic数据转发至物联网平台的其他Topic或所支持的其他阿里云服务。

## 限制说明

-   服务地域不同，所支持的目标云产品有所不同。规则引擎支持的地域及目标云产品，请参见[地域与可用区](~~85669~~)。
-   一个规则下面最多可创建10个规则动作。
-   您可以通过调用该API创建规则动作，定义将数据转发至物联网平台其他Topic和其他阿里云产品，包括DataHub、消息服务、函数计算、表格存储和消息队列RocketMQ。但是，如果您想将数据转发至时序时空数据库（TSDB）和云数据库RDS版，请在[物联网平台控制台](https://iot.console.aliyun.com)进行操作。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为50。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=CreateRuleAction&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateRuleAction|系统规定参数。取值：CreateRuleAction。 |
|Configuration|String|是|\{"topic":"/a1POX0c\*\*\*\*/device1/user/get","topicType":1\}|该规则动作的配置信息，传入格式为JSON String。不同规则动作类型所需内容不同，具体要求和示例见下文请求参数补充说明。 |
|RuleId|Long|是|100000|要为其创建动作的规则ID。可在物联网平台控制台对应实例下，**规则引擎**\>**云产品流转**页查看规则ID，或调用[ListRule](~~69486~~)从返回结果中查看。 |
|Type|String|是|REPUBLISH|规则动作类型，取值：

 -   **DATAHUB**：将根据规则处理后的Topic数据转发至阿里云DataHub，进行流式数据处理。
-   **ONS**：将根据规则处理后的Topic数据转发至阿里云消息队列RocketMQ，进行消息分发。
-   **MNS**：将根据规则处理后的Topic数据发送至阿里云消息服务中，进行消息传输。
-   **FC**：将根据规则处理后的Topic数据发送至阿里云函数计算服务，进行事件计算。
-   **REPUBLISH**：将根据规则处理后的Topic数据转发至另一个物联网平台 Topic。
-   **AMQP**：数据流转到AMQP消费组。
-   **OTS**：将根据规则处理后的Topic数据发送至阿里云表格存储，进行NoSQL数据存储。

 **说明：**

-   数据格式为二进制的规则（即规则的**DataType**参数是**BINARY**）不支持转发数据至OTS（表格存储）。
-   服务地域不同，规则引擎所支持的数据转发目标云产品不同。具体请参见规则引擎相关[地域与可用区](~~85669~~)。 |
|IotInstanceId|String|否|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|ErrorActionFlag|Boolean|否|false|该规则动作是否为转发错误操作数据的转发动作，即转发流转到其他云产品失败且重试失败的数据。 可选值：

 -   **true**：该规则动作转发错误操作数据。
-   **false**：该规则动作不转发错误操作数据，而是正常转发操作。

 默认值为**false**。 |

**REPUBLISH类型Configuration定义**

|名称

|描述 |
|----|----|
|topic

|转发至的目标Topic（物模型通信Topic或自定义Topic）。支持将数据转发至数据下行的物模型通信Topic：

 `/sys/${YourProductKey}/${YourDeviceName}/thing/service/property/set`

 `/sys/${YourProductKey}/${YourDeviceName}/thing/service/${tsl.service.identifier}`，变量`${tsl.service.identifier}`的内容由该产品物模型中的服务决定。

| |
|topicType

|Topic的类型，取值：

 0：表示数据下行的物模型通信Topic。

 1：表示自定义Topic。

| |

REPUBLISH 类型**Configuration**示例：

```

sys类型：{"topic":"/sys/a1TXXXXXWSN/xxx_cache001/thing/service/property/set","topicType":0}
自定义类型：{"topic":"/a1TXXXXXWSN/xxx_cache001/user/update","topicType":1}

```

**DATAHUB类型Configuration定义**

|名称

|描述 |
|----|----|
|projectName

|目标DataHub中用来接收信息的具体Project。 |
|topicName

|目标DataHub中用来接收信息的具体Topic。 |
|regionName

|目标DataHub所在的阿里云地域代码，例如cn-shanghai。 |
|role

|授权角色信息。通过授予IoT指定的系统服务角色，您可以授权物联网平台访问您的DataHub。授权角色信息格式如下：

 `{"roleArn":"acs:ram::6541***:role/aliyuniotaccessingdatahubrole","roleName": "AliyunIOTAccessingDataHubRole"}`

 请将`6541***`替换成您的阿里云账号ID。您可以登录控制台，在账号安全设置页面查看您的账号ID。

 `AliyunIOTAccessingDataHubRole`是访问控制中定义的服务角色。用于授予物联网平台访问DataHub。关于角色的更多信息，请在访问控制RAM控制台的角色管理页面进行角色管理。 |
|schemaVals

|目标DataHub中的Schema列表，详情参见下表schemaVals。 |

schemaVals

|名称

|描述 |
|----|----|
|name

|列名。 |
|value

|列值。 |
|type

|列类型，取值：

 BIGINT：大整数型

 DOUBLE：双精度浮点型

 BOOLEAN：布尔型

 TIMESTAMP：时间戳型

 STRING：字符串型

 DECIMAL：小数型 |

DATAHUB类型**Configuration**示例：

```

{
    "schemaVals": [
        {
            "name": "devicename",
            "value": "${deviceName}",
            "type": "STRING"
        },
        {
            "name": "msgtime",
            "value": "${msgTime}",
            "type": "TIMESTAMP"
        }
    ],
    "role": {
        "roleArn": "acs:ram::6541***:role/aliyuniotaccessingdatahubrole",
        "roleName": "AliyunIOTAccessingDataHubRole"
    },
    "projectName": "iot_datahub_stream",
    "topicName": "device_message",
    "regionName": "cn-shanghai"
}

```

**OTS类型Configuration定义**

|名称

|描述 |
|----|----|
|instanceName

|表格存储中用来接收信息的实例名称。 |
|tableName

|表格存储中用来接收信息的数据表名称。 |
|regionName

|目标实例所在的阿里云地域代码，例如cn-shanghai。 |
|role

|授权角色信息。通过授予物联网平台指定的系统服务角色，您可以授权物联网平台访问您的表格存储。授权角色信息如下：

 `{"roleArn":"acs:ram::6541***:role/aliyuniotaccessingotsrole","roleName": "AliyunIOTAccessingOTSRole"}`

 请将`6541***`替换成您的阿里云账号ID。您可以登录控制台，在账号安全设置页面查看您的账号ID。

 `AliyunIOTAccessingOTSRole`是访问控制中定义的服务角色。用于授予物联网平台访问表格存储。关于角色的更多信息，请在访问控制RAM控制台的角色管理页面进行角色管理。 |
|primaryKeys

|目标表中的主键列表。详情参见下表PrimaryKeys。 |

PrimaryKeys

|名称

|描述 |
|----|----|
|columnType

|主键类型，取值：

 INTEGER：整型

 STRING：字符串

 BINARY：二进制 |
|columnName

|主键名称。 |
|columnValue

|主键值。 |
|option

|主键是否为自增列，取值：AUTO\_INCREMENT或为空。当主键类型为INTEGER，且该字段为AUTO\_INCREMENT时，主键为自增列。 |

OTS类型**Configuration**示例：

```

{
    "instanceName": "testaaa",
    "tableName": "tt",
    "primaryKeys": [
        {
            "columnType": "STRING",
            "columnName": "ttt",
            "columnValue": "${tt}",
            "option": ""
        },
        {
            "columnType": "INTEGER",
            "columnName": "id",
            "columnValue": "",
            "option": "AUTO_INCREMENT"
        }
    ],
    "regionName": "cn-shanghai",
    "role": {
        "roleArn": "acs:ram::5645***:role/aliyuniotaccessingotsrole",
        "roleName": "AliyunIOTAccessingOTSRole"
    }
}

```

**MNS类型Configuration定义**

|名称

|描述 |
|----|----|
|themeName

|消息服务中用来接收信息的目标主题名称。 |
|regionName

|目标消息服务所在的阿里云地域代码，例如cn-shanghai。 |
|role

|授权角色信息。通过授予物联网平台指定的系统服务角色，您可以授权物联网平台访问您的消息服务。授权角色信息如下：

 `{"roleArn":"acs:ram::6541***:role/aliyuniotaccessingmnsrole","roleName": "AliyunIOTAccessingMNSRole"}`

 请将`6541***`替换成您的阿里云账号ID。您可以登录控制台，在账号安全设置页面查看您的账号ID。

 `AliyunIOTAccessingMNSRole`是访问控制中定义的服务角色。用于授予物联网平台访问消息服务。关于角色的更多信息，请在访问控制RAM控制台的角色管理页面进行角色管理。 |

MNS类型**​Configuration**​​示例：

```

{
    "themeName": "mns-test-topic1",
    "regionName": "cn-shanghai",
    "role": {
        "roleArn": "acs:ram::5645***:role/aliyuniotaccessingmnsrole",
        "roleName": "AliyunIOTAccessingMNSRole"
    }
}

```

**FC类型Configuration定义**

|名称

|描述 |
|----|----|
|functionName

|函数服务中用来接收信息的目标函数名称。 |
|serviceName

|函数服务中用来接收信息的目标服务名称。 |
|regionName

|目标函数服务实例所在阿里云地域的代码，如cn-shanghai。 |
|role

|授权角色信息。通过授予物联网平台指定的系统服务角色，您可以授权物联网平台访问您的函数计算服务。授权角色信息如下：

 `{"roleArn":"acs:ram::6541***:role/aliyuniotaccessingfcrole","roleName": "AliyunIOTAccessingFCRole"}`

 请将`6541***`替换成您的阿里云账号ID。您可以登录控制台，在账号安全设置页面查看您的账号ID。

 `AliyunIOTAccessingFCRole`是访问控制中定义的服务角色。用于授予物联网平台访问函数计算。关于角色的更多信息，请在访问控制RAM控制台的角色管理页面进行角色管理。 |

FC类型**Configuration**示例：

```

{
    "regionName": "cn-shanghai",
    "role": {
        "roleArn": "acs:ram::5645***:role/aliyuniotaccessingfcrole",
        "roleName": "AliyunIOTAccessingFCRole"
    },
    "functionName": "weatherForecast",
    "serviceName": "weather"
}

```

`<props=china>`**ONS类型Configuration定义**

**说明：** 您需通过调用消息队列RocketMQ的SDK，或在消息队列RocketMQ控制台，授权物联网平台访问消息队列RocketMQ（至少要授予物联网平台发布权限），然后才能够成功创建将Topic数据转发至消息队列RocketMQ的规则动作。

|名称

|描述 |
|----|----|
|instanceId

|RocketMQ中用来接收消息的目标Topic所属的实例ID。 |
|topic

|RocketMQ中用来接收信息的目标Topic。 |
|regionName

|目标RocketMQ实例所在的阿里云地域代码，例如cn-shanghai。

 \> 公网和同区流转，使用普通版RocketMQ实例即可；如果您需要跨区流转，则RocketMQ实例必需是铂金版实例。 |
|tag

|（可选）设置标签。长度限制为128字节。 |
|role

|授权角色信息。通过授予物联网平台指定的系统服务角色，您可以授权物联网平台访问您的消息队列RocketMQ服务。授权角色信息如下：

 `{"roleArn":"acs:ram::6541***:role/aliyuniotaccessingonsrole","roleName": "AliyunIOTAccessingONSRole"}`

 请将`6541***`替换成您的阿里云账号ID。您可以登录控制台，在账号安全设置页面查看您的账号ID。

 `AliyunIOTAccessingONSRole`是访问控制中定义的服务角色。用于授予物联网平台访问消息队列RocketMQ。关于角色的更多信息，请在访问控制RAM控制台的角色管理页面进行角色管理。 |

ONS类型**Configuration**示例：

```

{
    "instanceId": "MQ_INST_123157908552****_XXXXXX"
    "topic": "aliyun-iot-XXXXX",
    "regionName": "cn-hangzhou",
    "role": {
        "roleArn": "acs:ram::6541***:role/aliyuniotaccessingonsrole",
        "roleName": "AliyunIOTAccessingONSRole"
    }
}

```

**AMQP类型Configuration定义**

|名称

|描述 |
|----|----|
|groupId

|消费组ID。 |

AMQP类型**Configuration**示例：

```

{
    "groupId":"ZTh1JmyLGuZcUfv44p4z00****"
}

```

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|ActionId|Long|10003|调用成功时，规则引擎为该规则动作生成的规则动作ID，作为其标识符。

 **说明：** 请妥善保管该信息。在调用与规则动作相关的接口时，您可能需要提供对应的规则动作ID。 |
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|21D327AF-A7DE-4E59-B5D1-ACAC8C024555|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateRuleAction
&RuleId=100000
&Type=REPUBLISH
&Configuration={"topic":"/a1POX0c****/device1/user/get","topicType":1}
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<CreateRuleActionResponse>
      <RequestId>21D327AF-A7DE-4E59-B5D1-ACAC8C024555</RequestId>
      <ActionId>10003</ActionId>
      <Success>true</Success>
</CreateRuleActionResponse>
```

`JSON` 格式

```
{
    "RequestId": "21D327AF-A7DE-4E59-B5D1-ACAC8C024555",
    "ActionId": 10003,
    "Success": true
}
```

