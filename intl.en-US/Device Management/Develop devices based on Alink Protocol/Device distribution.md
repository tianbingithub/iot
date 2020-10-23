# Device distribution

IoT Platform allows you to distribute devices across regions, instances, or accounts. IoT Platform sends a notification to devices after you configure a device distribution in the IoT Platform console.

For more information about the procedure of device distribution, see [Distribute devices](/intl.en-US/Device Management/Resource allocation/Distribute devices.md).

## Device distribution notifications

The following topics are used when IoT Platform sends downstream notifications and devices send upstream responses:

-   Request topic: `/sys/${productKey}/${deviceName}/thing/bootstrap/notify`
-   Response topic: `/sys/${productKey}/${deviceName}/thing/bootstrap/notify_reply`

Alink request format:

```
{
    "id": "123",
    "version": "1.0",
    "method": "thing.bootstrap.notify", 
    "params": {
      "cmd": 0
    }
}
```

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The ID of the message. Valid values: 0 to 4294967295. Each message ID must be unique for the current device.|
|version|String|The version of the protocol. Set this parameter to 1.0.|
|method|String|The request method. Set the value to thing.bootstrap.notify.|
|params|List|The list of request parameters.|
|cmd|Integer|Set the value to 0. The value 0 indicates that a device distribution occurs and the distributed devices send a request to obtain a new Bootstrap endpoint.|

Alink response format:

```
{
    "id": "456",
    "code":200,
    "data" : {}
}
```

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The ID of the message. Valid values: 0 to 4294967295. Each message ID must be unique for the current device.|
|code|Integer|The HTTP status code in the response. The value 200 indicates that the request was successful. Other values indicate the request failed. For more information, see [Common codes returned by devices](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Common codes on devices.md).|
|data|Object|The data returned by the device. This response parameter is empty.|

