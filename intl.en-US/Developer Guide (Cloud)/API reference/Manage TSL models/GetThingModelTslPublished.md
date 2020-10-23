# GetThingModelTslPublished

Queries the published TSL of a product.

Each Alibaba Cloud account can run a maximum of 20 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=GetThingModelTslPublished&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetThingModelTslPublished|The operation that you want to perform. Set the value to GetThingModelTslPublished. |
|ProductKey|String|Yes|a1BwAGV\*\*\*\*|The ProductKey of the product. |
|IotInstanceId|String|No|iot-cn-0pp1n8t\*\*\*\*|The ID of the instance. The ID of the instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |
|ResourceGroupId|String|No|rg-acfm4l5tcwd\*\*\*\*|The ID of the resource group.

**Note:** You cannot specify this parameter. |
|ModelVersion|String|No|v1.0.0|The version of the TSL.

You can call the [ListThingModelVersion](~~150318~~) operation to view the TSL versions of a product.

If you do not specify this parameter, the last published TSL version is returned. |
|Simple|Boolean|No|true|Specifies whether to retrieve a simplified TSL.

-   true: retrieves a simplified TSL. A simplified TSL only includes the **identifier** and **dataType** attributes of input and output parameters that are defined in properties, services, and events. Simplified TSLs can be used by the device developers for reference.
-   false: retrieves the complete TSL. A complete TSL includes all parameters and values of properties, services, events. Complete TSLs can be used by cloud application developers for reference.

Default value: false. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For information about common request parameters, see [Common parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|Data|Struct| |The returned data. |
|TslStr|String|\{\\"schema\\":\\"https://iotx-tsl.oss-ap-southeast-1.aliyuncs.com/schema.json\\",\\"profile\\":\{\\"productKey\\":\\"a14TeWI\*\*\*\*\\"\},\\"properties\\":\[\{\\"identifier\\":\\"Humidity\\"\}\]\}|The string of the TSL. |
|TslUri|String|https://iotx-pop-dsl.oss-cn-shanghai.aliyuncs.com/thing/a14TeWI\*\*\*\*/model.json?Expires=1581947119&OSSAccessKeyId=LTAIuFOwFSR9\*\*\*\*&Signature=5i389hacjdj3t%2FnrHmQpEUfnxw\*\*\*\*|The URI that is used to store the TSL data in Object Storage Service \(OSS\). The URI is valid for 60 minutes. |
|ErrorMessage|String|A system exception occurred.|The error message returned if the call fails. |
|RequestId|String|E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful.

-   **true**: The call was successful.
-   **false**: The call failed. |

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=GetThingModelTslPublished
&ProductKey=a1BwAGV****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<GetThingModelTslPublishedResponse>
    <Data>
            <TslUri>https://iotx-pop-dsl.oss-cn-shanghai.aliyuncs.com/thing/a14TeWI****/model.json?Expires=1581947119&amp;OSSAccessKeyId=LTAIuFOwFSR9****&amp;Signature=5i389hacjdj3t%2FnrHmQpEUfnx****</TslUri>
            <TslStr>{"schema":"https://iotx-tsl.oss-ap-southeast-1.aliyuncs.com/schema.json","profile":{"productKey":"a14TeWI****"},"properties":[{"identifier":"Humidity","name":"Humidity","accessMode":"rw","required":false,"dataType":{"type":"int","specs":{"min":"55","max":"60","unit":"%","step":"1"}}},{"identifier":"Temperature","name":"Temperature","accessMode":"rw","required":false,"dataType":{"type":"float","specs":{"min":"26","max":"28","unit":"°C","step":"0.01"}}}],"events":[{"identifier":"post","name":"post","type":"info","required":true,"desc":"Submit properties","method":"thing.event.property.post","outputData":[{"identifier":"Humidity","name":"Humidity","dataType":{"type":"int","specs":{"min":"55","max":"60","unit":"%","step":"1"}}},{"identifier":"Temperature","name":"Temperature","dataType":{"type":"float","specs":{"min":"26","max":"28","unit":"°C","step":"0.01"}}}]}],"services":[{"identifier":"set","name":"set","required":true,"callType":"async","desc":"Set properties","method":"thing.service.property.set","inputData":[{"identifier":"Humidity","name":"Humidity","dataType":{"type":"int","specs":{"min":"55","max":"60","unit":"%","step":"1"}}},{"identifier":"Temperature","name":"Temperature","dataType":{"type":"float","specs":{"min":"26","max":"28","unit":"°C","step":"0.01"}}}],"outputData":[]},{"identifier":"get","name":"get","required":true,"callType":"async","desc":"Obtain properties","method":"thing.service.property.get","inputData":["Humidity","Temperature"],"outputData":[{"identifier":"Humidity","name":"Humidity","dataType":{"type":"int","specs":{"min":"55","max":"60","unit":"%","step":"1"}}},{"identifier":"Temperature","name":"Temperature","dataType":{"type":"float","specs":{"min":"26","max":"28","unit":"°C","step":"0.01"}}}]}]}</TslStr>
    </Data>
    <RequestId>C4371E68-F6DB-4D7B-8AD0-D38336E1DF94</RequestId>
    <Success>true</Success>
</GetThingModelTslPublishedResponse>
```

`JSON` format

```
{
    "Data": {
        "TslUri": "https://iotx-pop-dsl.oss-cn-shanghai.aliyuncs.com/thing/a14TeWI****/model.json?Expires=1581947119&OSSAccessKeyId=LTAIuFOwFSR9****&Signature=5i389hacjdj3t%2FnrHmQpEUfnx****",
        "TslStr": "{\"schema\":\"https://iotx-tsl.oss-ap-southeast-1.aliyuncs.com/schema.json \",\" profile\":[\"productKey\":\"a14TeWI %***** \",\" properties\":[{\"identifier\":\"Humidity\",\"name\":\"Humidity\",\" accessMode\":\"rw\",\"required\":false,\"dataType\":{\"type\":\"int\",\"specs\":{\"min\":\"55\",\"max\":\"60\",\"unit\":\"%\",\"step\":\" 1\"****** \"identifier\":\"Temperature\",\"name\":\"Temperature\",\"accessMode\":\"rw\",\"required\":false,\"dataType\":[\"type\": \"float\",\"specs\":{\"min\":\"26\",\"max\":\"28\",\"unit\":\"°C\",\"step\":\"0.01\"}}}],\"events\": [{\"identifier\":\"post\",\"name\":\"post\",\"type\":\"info\",\"required\":true,\"desc\":\"Submit properties reporting \",\"method\":\"thing.event.property.post\", \"outputData\":[{\"identifier\":\"Humidity\",\"name\":\"Humidity\",\"dataType\" :%\ "type\":\"int\",\"specs\" :%\ "min":\"55\", \"max\":\"60\",\"unit\":\"%\",\"step\":\"1\" ****** \"identifier\":\"Temperature\",\"name\":\"Temperature\",\"dataType\": {\"type\":\"float\",\"specs\":{\"min\":\"26\",\"max\":\"28\",\"unit\":\"°C\",\"step\":\"0.01\"}}}] }],\"services\":[{\"identifier\":\"set\",\"name\":\"set\",\"required\":true,\"callType\":\"async\",\"desc\":\"Set properties \",\" method\":\"thing.service.property.set\",\" inputData\":[{\"identifier\":\"Humidity\",\"name\":\" Humidity\",\"dataType\":\"type\":\"int\",\" specs\":\" min\":\"55\",\"max\":\"60\",\"unit\":\"%\",\"step\":\"1\"}}},{\"identifier\":\"Temperature\",\"name\":\" Temperature \",\"dataType\":[\"type\":\"float\",\" specs\":[\"min\":\"26\",\" max\":\"28\",\"unit\":\"°C\",\"step\": \"0.01\"}}}],\"outputData\":[]},{\"identifier\":\"get\",\"name\":\"get\",\"required\":true,\"callType\":\"async\",\"desc\":\"Obtain properties\",\"method\":\"thing.service.property.get\",\"inputData\":[\"Humidity\",\"Temperature\"],\"outputData\":[{\"identifier\":\"Humidity\",\"name\":\"Humidity \", \"dataType\":{\"type\":\"int\",\"specs\":{\"min\":\"55\",\"max\":\"60\",\"unit\":\"%\",\"step\":\" 1\"****** \"identifier\":\"Temperature\",\"name\":\"Temperature\",\"dataType\":\\"type\":\"float\",\"specs\":[\"min ":\"26\", \"max\":\"28\",\"unit\":\"°C\",\"step\":\"0.01\"}}}]}]}"
    },
    "RequestId": "C4371E68-F6DB-4D7B-8AD0-D38336E1DF94",
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

