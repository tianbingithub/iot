# UpdateThingModel

Updates a Thing Specification Language \(TSL\) feature of a specified product.

## Limits

-   You can call this operation to update only one feature. TSL features include properties, services, and events.
-   Each Alibaba Cloud account can run a maximum of 5 queries per second \(QPS\).

**Note:** RAM users of an Alibaba Cloud account share the quota of the account.


## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Iot&api=UpdateThingModel&type=RPC&version=2018-01-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpdateThingModel|The operation that you want to perform. Set the value to UpdateThingModel. |
|Identifier|String|Yes|Temperature|The identifier of the feature.

To update a feature, you must specify the identifier of the feature. You can call the [GetThingModelTsl](~~150319~~) operation and view the identifier in the the TslStr response parameter. |
|ProductKey|String|Yes|a1BwAGV\*\*\*\*|The ProductKey of the product. |
|ThingModelJson|String|Yes|\{"properties":\[ \{ "identifier": "SimCardType", "extendConfig":"\{...\}", "dataSpecs": \{ "max": "1", "dataType": "INT", "unit": "mmHg", "min": "0", "step": "1" \}, "std": false, "custom": true, "dataType": "INT", "rwFlag": "READ\_ONLY", "productKey": "a1Jw4i \*\*\* \*", "required": false, "customFlag": true, "name": "SIM card type" \} \] \}|The updated details of the feature.

**Note:** You can specify only one feature.

For more information about how to specify this parameter, see [Data structure of ThingModelJson](~~150457~~). |
|IotInstanceId|String|No|iot-cn-0pp1n8t\*\*\*\*|The ID of the instance. The ID of the instance. This parameter is not required for public instances. However, the parameter is required for the instances that you have purchased. |

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
https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateThingModel
&ProductKey=a1Jw4id****
&ThingModelJson={"properties":[{"identifier": "SimCardType","dataSpecs": {"max": "1", "dataType": "INT","unit": "mmHg","min": "0","step": "1"},"std": false,"custom": true,"dataType": "INT","rwFlag": "READ_ONLY","productKey": "a1Jw4id****","required": false,"customFlag": true, "name": "SIM card type"}]}
&Identifier=SimCardType
&<Common request parameters>
```

Sample success responses

`XML` format

```
<UpdateThingModelResponse>
  <RequestId>5573D217-8E3E-47AD-9331-2083B88E64B2</RequestId>
  <Success>true</Success>
</UpdateThingModelResponse>
```

`JSON` format

```
{
  "RequestId": "5573D217-8E3E-47AD-9331-2083B88E64B2",
  "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Iot).

