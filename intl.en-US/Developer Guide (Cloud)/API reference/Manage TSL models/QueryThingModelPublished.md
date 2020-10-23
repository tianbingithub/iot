# QueryThingModelPublished

Queries the published TSL of a specified product.

Each Alibaba Cloud account can run a maximum of 10 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=QueryThingModelPublished&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|QueryThingModelPublished|The operation that you want to perform. Set the value to QueryThingModelPublished. |
|ProductKey|String|Yes|a1BwAGV\*\*\*\*|The ProductKey of the product. |
|IotInstanceId|String|No|iot-cn-0pp1n8t\*\*\*\*|The ID of the instance. The ID of the instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |
|ResourceGroupId|String|No|rg-acfm4l5tcwd\*\*\*\*|The ID of the resource group.

**Note:** You cannot specify this parameter. |
|ModelVersion|String|No|v1.0.0|The version of the TSL.

You can call the [ListThingModelVersion](~~150318~~) operation to view the TSL versions of a product.

If you do not specify this parameter, the last published TSL version is returned. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For information about common request parameters, see [Common parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|Data|Struct| |The returned data. |
|ThingModelJson|String|\{\\"\_ppk\\":\{\\"version\\":\\"1594253010934\\"\},\\"events\\":\[\],\\"productKey\\":\\"a1BwAGV\*\*\*\*\\",\\"properties\\":\[\{\\"configCode\\":\\"8C03F0EEC63D4897BF2637F89AE36B011594227294067\\",\\"custom\\":true,\\"customFlag\\":true,\\"dataSpecs\\":\{\\"custom\\":true,\\"dataType\\":\\"INT\\",\\"max\\":\\"1\\",\\"min\\":\\"0\\",\\"step\\":\\"1\\",\\"unit\\":\\"ppb\\"\},\\"dataType\\":\\"INT\\",\\"description\\":\\"1\\",\\"extendConfig\\":\\"\{\\\\\\"originalDataType\\\\\\":\{\\\\\\"specs\\\\\\":\{\\\\\\"registerCount\\\\\\":1,\\\\\\"reverseRegister\\\\\\":0,\\\\\\"swap16\\\\\\":0\},\\\\\\"type\\\\\\":\\\\\\"bool\\\\\\"\},\\\\\\"identifier\\\\\\":\\\\\\"WakeUpData\\\\\\",\\\\\\"registerAddress\\\\\\":\\\\\\"0x04\\\\\\",\\\\\\"scaling\\\\\\":1,\\\\\\"writeFunctionCode\\\\\\":0,\\\\\\"operateType\\\\\\":\\\\\\"inputStatus\\\\\\",\\\\\\"pollingTime\\\\\\":1000,\\\\\\"trigger\\\\\\":1\}\\",\\"identifier\\":\\"WakeUpData\\",\\"name\\":\\"WakeUpData\\",\\"productKey\\":\\"a114xeJGj2p\\",\\"required\\":false,\\"rwFlag\\":\\"READ\_ONLY\\",\\"std\\":false\}\],\\"services\\":\[\]\}|The TSL features. For more information about the data format of the ThingModelJson parameter, see [Data structure of ThingModelJson](~~150457~~). |
|ErrorMessage|String|A system exception occurred.|The error message returned if the call fails. |
|ProductKey|String|a1BwAGV\*\*\*\*|The ProductKey of the product. |
|RequestId|String|E4C0FF92-2A86-41DB-92D3-73B60310D25E|The ID of the request. |
|Success|Boolean|true|Indicates whether the call is successful.

-   **true**: The call was successful.
-   **false**: The call failed. |

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryThingModelPublished
&ProductKey=a1BwAGV****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<QueryThingModelPublishedResponse>
    <RequestId>1F9041A2-ED5B-4A5A-9C44-598E28C0B434</RequestId>
    <Data>
        <ThingModelJson>{"_ppk":{"version":"1594253010934"},"events":[],"productKey":"a1BwAGV****","properties":[{"configCode":"8C03F0EEC63D4897BF2637F89AE36B011594227294067","custom":true,"customFlag":true,"dataSpecs":{"custom":true,"dataType":"INT","max":"1","min":"0","step":"1","unit":"ppb"},"dataType":"INT","description":"1","extendConfig":"{\"originalDataType\":{\"specs\":{\"registerCount\":1,\"reverseRegister\":0,\"swap16\":0},\"type\":\"bool\"},\"identifier\":\"WakeUpData\",\"registerAddress\":\"0x04\",\"scaling\":1,\"writeFunctionCode\":0,\"operateType\":\"inputStatus\",\"pollingTime\":1000,\"trigger\":1}","identifier":"WakeUpData","name":"WakeUpData","productKey":"a114xeJ****","required":false,"rwFlag":"READ_ONLY","std":false}],"services":[]}</ThingModelJson>
    </Data>
    <Success>true</Success>
</QueryThingModelPublishedResponse>
```

`JSON` format

```
{
   "RequestId": "1F9041A2-ED5B-4A5A-9C44-598E28C0B434",
   "Data": {
      "ThingModelJson": "{\"_ppk\":{\"version\":\"1594253010934\"},\"events\":[],\"productKey\":\"a1BwAGV****\",\"properties\":[{\"configCode\":\"8C03F0EEC63D4897BF2637F89AE36B011594227294067\",\"custom\":true,\"customFlag\":true,\"dataSpecs\":{\"custom\":true,\"dataType\":\"INT\",\"max\":\"1\",\"min\":\"0\",\"step\":\"1\",\"unit\":\"ppb\"},\"dataType\":\"INT\",\"description\":\"1\",\"extendConfig\":\"{\\\"originalDataType\\\":{\\\"specs\\\":{\\\"registerCount\\\":1,\\\"reverseRegister\\\":0,\\\"swap16\\\":0},\\\"type\\\":\\\"bool\\\"},\\\"identifier\\\":\\\"WakeUpData\\\",\\\"registerAddress\\\":\\\"0x04\\\",\\\"scaling\\\":1,\\\"writeFunctionCode\\\":0,\\\"operateType\\\":\\\"inputStatus\\\",\\\"pollingTime\\\":1000,\\\"trigger\\\":1}\",\"identifier\":\"WakeUpData\",\"name\":\"WakeUpData\",\"productKey\":\"a114xeJGj2p\",\"required\":false,\"rwFlag\":\"READ_ONLY\",\"std\":false}],\"services\":[]}"
   },
   "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

