# QueryThingModel

Queries the TSL features of a specified product.

## Description

TSL features include properties, services, and events.

## Limits

Each Alibaba Cloud account can run a maximum of 10 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=QueryThingModel&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|QueryThingModel|The operation that you want to perform. Set the value to QueryThingModel. |
|ProductKey|String|Yes|a1BwAGV\*\*\*\*|The ProductKey of the product. |
|IotInstanceId|String|No|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|The ID of the instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |
|ResourceGroupId|String|No|rg-acfm4l5tcwd\*\*\*\*|The ID of the resource group.

**Note:** You cannot specify this parameter. |
|ModelVersion|String|No|v1.0.0|The TSL version to be queried.

You can call the [ListThingModelVersion](~~150318~~) operation to view the TSL versions of a product.

If you do not specify this parameter, the TSL that is in the draft status is queried. If you specify this parameter, the TSL of the specified version is queried. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For more information about common request parameters, see [Common parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|Data|Struct| |The data returned if the call is successful. |
|ThingModelJson|String|\{"productKey":"a1Jw4id\*\*\*", "\_ppk":\{ "version":"1.1", "description":"xxx" \}, "properties":\[ \{ "identifier": "SimCardType", "dataSpecs": \{ "max": "1", "dataType": "INT", "unit": "mmHg", "min": "0", "step": "1"\} , "std": false, "custom": true, "dataType": "INT", "rwFlag": "READ\_ONLY", "productKey": "a1Jw4idFWHX", "required": false, "customFlag": true, "name": "SIM card type" \} \], "services":\[\], "events":\[\] \}|The features that are defined in the TSL model. For more information about the data format of the ThingModelJson parameter, see [Data structure of ThingModelJson](~~150457~~). |
|ErrorMessage|String|A system exception occurred.|The error message returned if the call fails. |
|ProductKey|String|a1BwAGV\*\*\*\*|The ProductKey of the product. |
|RequestId|String|E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565|The ID of the request. |
|Success|Boolean|true|Indicates whether the call was successful.

-   **true**: The call was successful.
-   **false**: The call failed. |

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryThingModel
&ProductKey=a1bPo9p****
&ModelVersion=v1.0.0
&<Common request parameters>
```

Sample success response

`XML` format

```
<QueryThingModelResponse>
    <RequestId>1F9041A2-ED5B-4A5A-9C44-598E28C0B434</RequestId>
    <Data>
        <ThingModelJson>{"_ppk":{"version":"1594253010934"},"events":[],"productKey":"a114xeJ****","properties":[{"configCode":"8C03F0EEC63D4897BF2637F89AE36B011594227294067","custom":true,"customFlag":true,"dataSpecs":{"custom":true,"dataType":"INT","max":"1","min":"0","step":"1","unit":"ppb"},"dataType":"INT","description":"1","extendConfig":"{\"originalDataType\":{\"specs\":{\"registerCount\":1,\"reverseRegister\":0,\"swap16\":0},\"type\":\"bool\"},\"identifier\":\"WakeUpData\",\"registerAddress\":\"0x04\",\"scaling\":1,\"writeFunctionCode\":0,\"operateType\":\"inputStatus\",\"pollingTime\":1000,\"trigger\":1}","identifier":"WakeUpData","name":"WakeUpData,"productKey":"a114xeJGj2p","required":false,"rwFlag":"READ_ONLY","std":false}],"services":[]}</ThingModelJson>
    </Data>
    <Success>true</Success>
</QueryThingModelResponse>
```

`JSON` format

```
{
   "RequestId": "1F9041A2-ED5B-4A5A-9C44-598E28C0B434",
   "Data": {
      "ThingModelJson": "{\"_ppk\":{\"version\":\"1594253010934\"},\"events\":[],\"productKey\":\"a114xeJ****\",\"properties\":[{\"configCode\":\"8C03F0EEC63D4897BF2637F89AE36B011594227294067\",\"custom\":true,\"customFlag\":true,\"dataSpecs\":{\"custom\":true,\"dataType\":\"INT\",\"max\":\"1\",\"min\":\"0\",\"step\":\"1\",\"unit\":\"ppb\"},\"dataType\":\"INT\",\"description\":\"1\",\"extendConfig\":\"{\\\"originalDataType\\\":{\\\"specs\\\":{\\\"registerCount\\\":1,\\\"reverseRegister\\\":0,\\\"swap16\\\":0},\\\"type\\\":\\\"bool\\\"},\\\"identifier\\\":\\\"WakeUpData\\\",\\\"registerAddress\\\":\\\"0x04\\\",\\\"scaling\\\":1,\\\"writeFunctionCode\\\":0,\\\"operateType\\\":\\\"inputStatus\\\",\\\"pollingTime\\\":1000,\\\"trigger\\\":1}\",\"identifier\":\"WakeUpData\",\"name\":\"WakeUpData\",\"productKey\":\"a114xeJGj2p\",\"required\":false,\"rwFlag\":\"READ_ONLY\",\"std\":false}],\"services\":[]}"
   },
   "Code": "",
   "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

