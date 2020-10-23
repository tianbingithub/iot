# CreateThingModel

Adds a TSL feature to a specified product.

## Limits

-   You can call this operation to add a maximum of 10 TSL features. TSL features include properties, services, and events.
-   Each Alibaba Cloud account can run a maximum of 5 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.


## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=CreateThingModel&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateThingModel|The operation that you want to perform. Set the value to CreateThingModel. |
|ProductKey|String|Yes|a1BwAGV\*\*\*\*|The ProductKey of the product.

You can view the ProductKey on the Product Details page of the IoT Platform console. You can also obtain the ProductKey by calling the [QueryProductList](~~69271~~) operation. |
|ThingModelJson|String|Yes|\{ "properties":\[ \{ "identifier": "SimCardType", "extendConfig":"\{...\}", "dataSpecs": \{ "max": "1", "dataType": "INT", "unit": "mmHg", "min": "0", "step": "1" \}, "std": false, "custom": true, "dataType": "INT", "rwFlag": "READ\_ONLY", "productKey": "a1bPo9p\*\*\*\*", "required": false, "customFlag": true, "name": "SIM card type" \} \]， "services":\[...\], "events":\[...\] \}|The details of the feature.

**Note:** You can specify a maximum of 10 features.

For more information about how to specify this parameter, see Data structure of ThingModelJson. |
|IotInstanceId|String|No|iot-cn-0pp1n8t\*\*\*\*|The ID of the instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |

In addition to the preceding operation-specific request parameters, you must specify common request parameters when you call this API operation. For more information about common request parameters, see [Common parameters](~~30561~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|iot.system.SystemException|The error code returned if the call fails. For more information about error codes, see [Error codes](~~87387~~). |
|ErrorMessage|String|A system exception occurred.|The error message returned if the call fails. |
|RequestId|String|E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565|The ID of the request. |
|Success|Boolean|true|Indicates whether the call was successful.

-   **true**: The call was successful.
-   **false**: The call failed. |

## Examples

Sample requests

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateThingModel
&ProductKey=a1bPo9p****
&ThingModelJson={"properties":[{"identifier": "SimCardType","dataSpecs": {"max": "1", "dataType": "INT","unit": "mmHg","min": "0","step": "1"},"std": false,"custom": true,"dataType": "INT","rwFlag": "READ_ONLY","productKey": "a1bPo9p****","required": false,"customFlag": true, "name": "SIM card type"}]}
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreateThingModelResponse>
  <RequestId>9E76053E-26ED-4AB4-AE58-8AFC3F1E7E8E</RequestId>
  <Success>true</Success>
</CreateThingModelResponse>
```

`JSON` format

```
{
  "RequestId": "9E76053E-26ED-4AB4-AE58-8AFC3F1E7E8E",
  "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

