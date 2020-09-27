# QueryThingModelExtendConfig

Queries the extended TSL information of a specified product.

Each Alibaba Cloud account can run a maximum of 20 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=QueryThingModelExtendConfig&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|QueryThingModelExtendConfig|The operation that you want to perform. Set the value to QueryThingModelExtendConfig. |
|ProductKey|String|Yes|a1T27vz\*\*\*\*|The ProductKey of the product. |
|IotInstanceId|String|No|iot-cn-0pp1n8t\*\*\*\*|The ID of the instance. The ID of the instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |
|ResourceGroupId|String|No|rg-acfm4l5tcwd\*\*\*\*|The ID of the resource group.

**Note:** You cannot specify this parameter. |
|ModelVersion|String|No|v1.0.0|The version of the TSL.

You can call the [ListThingModelVersion](~~150318~~) operation to view the TSL version of a product.

If you do not specify this parameter, the last published TSL version is returned. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For more information, see [Common request parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code returned if the call fails. For more information, see [Error codes](~~87387~~). |
|Data|Struct| |The returned data. |
|Configuration|String|\{\\"profile\\":\{\\"productKey\\":\\"a114x\*\*\*\*\*\*\\"\},\\"properties\\":\[\{\\"originalDataType\\":\{\\"specs\\":\{\\"registerCount\\":1,\\"reverseRegister\\":0,\\"swap16\\":0\},\\"type\\":\\"bool\\"\},\\"identifier\\":\\"WakeUpData\\",\\"registerAddress\\":\\"0x04\\",\\"scaling\\":1,\\"writeFunctionCode\\":0,\\"operateType\\":\\"inputStatus\\",\\"pollingTime\\":1000,\\"trigger\\":1\}\]\}|The information of the extended TSL parameters. For more information about the definition of extended parameters, see [CreateThingModel](~~150323~~). |
|ErrorMessage|String|System exception|The error message returned if the call fails. |
|RequestId|String|E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful.

-   **true**: The call was successful.
-   **false**: The call failed. |

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryThingModelExtendConfig
&ProductKey=a1T27vz****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<QueryThingModelExtendConfigResponse>
    <Data>
        <Configuration>{"profile":{"productKey":"a114x******"},"properties":[{"originalDataType":{"specs":{"registerCount":1,"reverseRegister":0,"swap16":0},"type":"bool"},"identifier":"WakeUpData","registerAddress":"0x04","scaling":1,"writeFunctionCode":0,"operateType":"inputStatus","pollingTime":1000,"trigger":1}]}</Configuration>
    </Data>
    <RequestId>6DDF9D04-24C3-40D8-B490-2A528E59EA67</RequestId>
    <Success>true</Success>
</QueryThingModelExtendConfigResponse>
```

`JSON` format

```
{
   "Data": {
      "Configuration": "{\"profile\":{\"productKey\":\"a114x******\"},\"properties\":[{\"originalDataType\":{\"specs\":{\"registerCount\":1,\"reverseRegister\":0,\"swap16\":0},\"type\":\"bool\"},\"identifier\":\"WakeUpData\",\"registerAddress\":\"0x04\",\"scaling\":1,\"writeFunctionCode\":0,\"operateType\":\"inputStatus\",\"pollingTime\":1000,\"trigger\":1}]}"
   },
   "RequestId": "6DDF9D04-24C3-40D8-B490-2A528E59EA67",
   "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

