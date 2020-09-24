---
keyword: [sub-device, gateway, Alink protocol, Internet of Things, connect sub-devices in batches, IoT, IoT Platform, topological relationship, connect, disconnect, disconnect sub-devices in batches, response topic, request topic, message structure, data structure]
---

# Connect or disconnect sub-devices

You can connect or disconnect sub-devices in sequence or in batches. Before you connect a sub-device, you must register the sub-device in IoT Platform to establish a topology between the sub-device and the gateway. When the sub-device is connected, IoT Platform verifies the identity of the sub-device based on the topology to determine whether the sub-device can access the gateway.

**Note:**

-   The message delivery between sub-devices and IoT Platform supports QoS 0 rather than QoS 1.
-   The number of sub-devices that are online at the same time cannot exceed 1,500 for a gateway. If the number of online sub-devices exceeds 1,500, IoT Platform rejects all new connection requests.
-   When you connect or disconnect sub-devices in batches, the number of sub-devices in one batch cannot exceed 5.
-   When you connect or disconnect sub-devices in batches, the result is either all sub-devices succeed or all sub-devices fail. If all sub-devices fail, the returned data contain detailed failure information.

## Connect sub-devices

Upstream data:

-   Request topic: `/ext/session/${productKey}/${deviceName}/combine/login`
-   Response topic: `/ext/session/{gw_productKey}/{gw_deviceName}/combine/login_reply`

**Note:** Sub-devices use the gateway to communicate with IoT Platform. The preceding topics belong to the gateway. Replace the $\{productKey\} and $\{deviceName\} variables in the topic with the information of the gateway.

Sample request in the Alink JSON format:

```
{
  "id": "123",
  "params": {
    "productKey":"al12345****",
    "deviceName": "device1234",
    "clientId": "al12345****&device1234",
    "timestamp": "1581417203000",
    "signMethod": "hmacmd5",
    "sign": "9B9C732412A4F84B981E1AB97CAB****",
    "cleanSession": "true"
  }
}
```

**Note:** Replace the productKey and deviceName parameters in the message body with the information of the sub-device.

|Parameter|Type|Description|
|---------|----|-----------|
|id|String|The message ID. Valid values: 0 to 4294967295. Each message ID must be unique for the device.|
|params|Object|The request parameters. For more information, see the following params table.|

|Parameter|Type|Description|
|:--------|:---|:----------|
|deviceName|String|The DeviceName of the sub-device.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|
|sign|String|The signature of the sub-device. The signature procedure of the sub-device is the same as that of a directly connected device.

Procedure:

1.  Sort all parameters that are submitted to the server \(excluding the sign, signMethod, and cleanSession parameters\) in alphabetical order, and splice the parameters and values in sequence. No splicing symbol is required to separate these parameters and values.
2.  Use the algorithm that is specified by the signMethod parameter and the value of the DeviceSecret parameter to calculate the signature.

Use the calculation result as the value of the sign parameter.


Example that shows how to calculate the value of the sign parameter:

```
hmac_md5(deviceSecret, clientIdal12345****&device1234deviceNamedevice1234productKeyal12345****timestamp1581417203000)
``` |
|signMethod|String|The signature algorithm. The supported algorithms are HMACSHA1, HMACSHA256, HMACMD5, and SHA256.|
|timestamp|String|The timestamp. Unit: milliseconds.|
|clientId|String|The ID of the device. You can set this parameter to a value that combines the product key and device name based on the following syntax: `productKey&deviceName`.|
|cleanSession|String|-   If this parameter is set to true, all QoS 1 messages that are not received when the sub-device was offline are cleared.
-   If this parameter is set to false, all messages that are sent when the sub-device was offline are retained. |

Sample response in the Alink JSON format:

```
{
  "id":"123",
  "code": 200,
  "message":"success"
  "data":{
      "deviceName": "device1234",
      "productKey":"al12345****",
    }
}
```

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. Valid values: 0 to 4294967295. Each message ID must be unique for the device.|
|code|Integer|The HTTP status code. The value 200 indicates a successful request.|
|message|String|The result.|
|data|Object|The returned sub-device information in the case of a successful request. For more information about the parameters, see the following data table.|

|Parameter|Type|Description|
|---------|----|-----------|
|deviceName|String|The DeivceName of the sub-device.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|

Error messages:

|HTTP status code|Error message|Description|
|:---------------|:------------|:----------|
|460|request parameter error|The error message returned because one or more request parameters are invalid.|
|429|rate limit, too many subDeviceOnline msg in one minute|The error message returned because the system cannot process this number of authentication requests from the device. The device is throttled.|
|428|too many subdevices under gateway|The error message returned because the number of sub-devices that are online at the same time exceeds the limit.|
|6401|topo relation not exist|The error message returned because the topology between the gateway and the sub-device does not exist.|
|6100|device not found|The error message returned because the sub-device does not exist.|
|521|device deleted|The error message returned because the sub-device is deleted.|
|522|device forbidden|The error message returned because the sub-device is disabled.|
|6287|invalid sign|The error message returned because the password or signature of the sub-device is invalid.|

## Connect sub-devices in batches

Upstream data:

-   Request topic: `/ext/session/${productKey}/${deviceName}/combine/login`
-   Response topic: `/ext/session/{gw_productKey}/{gw_deviceName}/combine/login_reply`

**Note:** Sub-devices use the gateway to communicate with IoT Platform. The preceding topics belong to the gateway. Replace the $\{productKey\} and $\{deviceName\} variables in the topic with the information of the gateway.

Sample request in the Alink JSON format:

```
{
  "id": "123",
  "params":{ 
     "deviceList": [{
        "productKey": "al12345****", 
        "deviceName": "device1234",
        "clientId": "al12345****&device1234",
        "timestamp": "1581417203000", 
        "cleanSession": "false",
        "signMethod": "hmacmd5",
        "sign": "9B9C732412A4F84B981E1AB97CAB****",
     }, {
        "productKey": "al12345****", 
        "deviceName": "device4321",
        "clientId": "al12345****&device4321",
        "timestamp": "1581417203000", 
        "cleanSession": "true"
        "signMethod": "hmacmd5",
        "sign": "9B9C732412A4F84B981E1AB97CAB****",
     }]
  }
}
```

**Note:** Replace the productKey and deviceName parameters in the message body with the information of the sub-device.

|Parameter|Type|Description|
|---------|----|-----------|
|id|String|The message ID. Valid values: 0 to 4294967295. Each message ID must be unique for the device.|
|params|Object|The request parameters. The deviceList parameter includes the required parameters to authenticate sub-devices that you want to connect. For more information, see the following deviceList table.|

|Parameter|Type|Description|
|:--------|:---|:----------|
|deviceName|String|The DeviceName of the sub-device.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|
|sign|String|The signature of the sub-device. The signature procedure of the sub-device is the same as that of a directly connected device.

Procedure:

1.  Sort all parameters that are submitted to the server \(excluding the sign, signMethod, and cleanSession parameters\) in alphabetical order, and splice the parameters and values in sequence. No splicing symbol is required to separate these parameters and values.
2.  Use the algorithm that is specified by the signMethod parameter and the value of the DeviceSecret parameter to calculate the signature.

Use the calculation result as the value of the sign parameter.


Example that shows how to calculate the value of the sign parameter:

```
hmac_md5(deviceSecret, clientIdal12345****&device1234deviceNamedevice1234productKeyal12345****timestamp1581417203000)
``` |
|signMethod|String|The signature algorithm. The supported algorithms are HMACSHA1, HMACSHA256, HMACMD5, and SHA256.|
|timestamp|String|The timestamp. Unit: milliseconds.|
|clientId|String|The ID of the device. You can set this parameter to a value that combines the product key and device name based on the following syntax: `productKey&deviceName`.|
|cleanSession|String|-   If this parameter is set to true, all QoS 1 messages that are not received when the sub-device was offline are cleared.
-   If this parameter is set to false, all messages that are sent when the sub-device was offline are retained. |

Sample response in the Alink JSON format:

```
{
  "id":"123",
  "code":"200",
  "message":"success",
  "data": [{
      "productKey":"al12345****",
      "deviceName": "device1234"
    },{
      "deviceName": "device4321",
      "productKey": "al12345****"
    }]
}
```

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. Valid values: 0 to 4294967295. Each message ID must be unique for the device.|
|code|Integer|The HTTP status code. The value 200 indicates a successful request.|
|message|String|The result.|
|data|Object|The returned sub-device information in the case of a successful request. For more information about the parameters, see the following data table.|

|Parameter|Type|Description|
|---------|----|-----------|
|deviceName|String|The DeivceName of the sub-device.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|

Error messages:

|HTTP status code|Error message|Description|
|:---------------|:------------|:----------|
|460|request parameter error|The error message returned because one or more request parameters are invalid.|
|429|rate limit, too many subDeviceOnline msg in one minute|The error message returned because the system cannot process this number of authentication requests from the device. The device is throttled.|
|428|too many subdevices under gateway|The error message returned because the number of sub-devices that are online at the same time exceeds the limit.|
|6401|topo relation not exist|The error message returned because the topology between the gateway and the sub-device does not exist.|
|6100|device not found|The error message returned because the sub-device does not exist.|
|521|device deleted|The error message returned because the sub-device is deleted.|
|522|device forbidden|The error message returned because the sub-device is disabled.|
|6287|invalid sign|The error message returned because the password or signature of the sub-device is invalid.|

## Disconnect sub-devices

Upstream data:

-   Request topic: `/ext/session/${productKey}/${deviceName}/combine/login`
-   Response topic: `/ext/session/{gw_productKey}/{gw_deviceName}/combine/logout_reply`

**Note:** Sub-devices use the gateway to communicate with IoT Platform. The preceding topics belong to the gateway. Replace the $\{productKey\} and $\{deviceName\} variables in the topic with the information of the gateway.

Sample request in the Alink JSON format:

```
{
  "id": 123,
  "params": {
    "productKey": "al12345****",
    "deviceName": "device1234"
  }
}
```

**Note:** Replace the productKey and deviceName parameters in the message body with the information of the sub-device.

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. Valid values: 0 to 4294967295. Each message ID must be unique for the device.|
|params|Object|The request parameters. These parameters specify the information of the sub-device to disconnect.|

|Parameter|Type|Description|
|---------|----|-----------|
|deviceName|String|The DeviceName of the sub-device.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|

Sample response in the Alink JSON format:

```
{
  "id": "123",
  "code": 200,
  "message": "success",
  "data": {
      "deviceName": "device1234",
      "productKey": "al12345****"
    }
}
```

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. Valid values: 0 to 4294967295. Each message ID must be unique for the device.|
|code|Integer|The HTTP status code. The value 200 indicates a successful request.|
|message|String|The result.|
|data|Object|The returned sub-device information in the case of a successful request. For more information, see the following data table.|

|Parameter|Type|Description|
|---------|----|-----------|
|deviceName|String|The DeivceName of the sub-device.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|

Error messages:

|HTTP status code|Error message|Description|
|:---------------|:------------|:----------|
|460|request parameter error|The error message returned because the request parameters are invalid.|
|520|device no session|The error message returned the sub-device session does not exist.|

## Disconnect sub-devices in batches

Upstream data:

-   Request topic: `/ext/session/${productKey}/${deviceName}/combine/login`
-   Response topic: `/ext/session/{gw_productKey}/{gw_deviceName}/combine/logout_reply`

**Note:** Sub-devices use the gateway to communicate with IoT Platform. The preceding topics belong to the gateway. Replace the $\{productKey\} and $\{deviceName\} variables in the topic with the information of the gateway.

Sample request in the Alink JSON format:

```
{
  "id": 123,
  "params":[{
            "productKey": "al12345****",
            "deviceName": "device1234"
          },{
            "productKey": "al12345****",
            "deviceName": "device4321"
      }]
}
```

**Note:** Replace the productKey and deviceName parameters in the message body with the information of the sub-device.

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. Valid values: 0 to 4294967295. Each message ID must be unique for the device.|
|params|Object|The request parameters. These parameters specify the information of the sub-device to disconnect.|

|Parameter|Type|Description|
|---------|----|-----------|
|deviceName|String|The DeivceName of the sub-device.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|

Sample response in the Alink JSON format:

```
{
  "id":"123",
  "code":"200",
  "message":"success",
  "data": [{
      "productKey": "al12345****"
      "deviceName": "device1234"
    },{
      "deviceName": "device4321",
      "productKey": "al12345****"
    }]
}
```

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. Valid values: 0 to 4294967295. Each message ID must be unique for the device.|
|code|Integer|The HTTP status code. The value 200 indicates a successful request.|
|message|String|The result.|
|data|Object|The returned sub-device information in the case of a successful request. For more information, see the following data table.|

|Parameter|Type|Description|
|---------|----|-----------|
|deviceName|String|The DeivceName of the sub-device.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|

Error messages:

|HTTP status code|Error message|Description|
|:---------------|:------------|:----------|
|460|request parameter error|The error message returned because one or more request parameters are invalid.|
|520|device no session|The error message returned because the sub-device session does not exist.|

## References

For more information about how to connect sub-devices to IoT Platform, see [Device identity registration](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device identity registration.md).

For more information about error codes and solutions, see [Error codes for device SDKs](/intl.en-US/Device Management/Error codes for device SDKs.md).

