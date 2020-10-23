---
keyword: [Internet of Things, IoT Platform, IoT, forward data, send data, system topics, data format, data structure]
---

# Data formats

If you want to process data by using the data forwarding feature, you must write SQL statements based on topics. Data is also transferred based on topics when you configure AMQP and MNS server-side subscriptions. If you use custom topics, you can define the data formats of the topics. IoT Platform does not parse payloads in custom topics. The data formats of basic communication topics and Thing Specification Language \(TSL\)-based communication topics are defined by IoT Platform. In this case, you must parse payloads based on the defined data formats. This article describes the data formats of basic communication topics and TSL-based communication topics.

## Submit device status

Topic: `/as/mqtt/status/${productKey}/${deviceName}`

You can use this topic to retrieve the online or offline status of devices.

Data format:

```
{
    "status":"online|offline",
    "productKey":"al12345****",
    "deviceName":"deviceName1234",
    "time":"2018-08-31 15:32:28.205",
    "utcTime":"2018-08-31T07:32:28.205Z",
    "lastTime":"2018-08-31 15:32:28.195",
    "utcLastTime":"2018-08-31T07:32:28.195Z",
    "clientIp":"123.123.123. ***"
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|status|String|The status of the device. -   online: The device is online.
-   offline: The device is offline. |
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|time|String|The time when the status notification was sent.|
|utcTime|String|The UTC time when the status notification was sent.|
|lastTime|String|The time when the last communication occurred before the status changed. **Note:** To prevent message timing disorders, we recommend that you maintain the final device status based on the lastTime parameter. |
|utcLastTime|String|The UTC time when the last communication occurred before the status changed.|
|clientIp|String|The public IP address of the device.|

## Submit device properties

Topic: `/${productKey}/${deviceName}/thing/event/property/post`

You can use this topic to retrieve the properties that are submitted by devices.

Data format:

```
{
    "iotId":"4z819VQHk6VSLmmBJfrf00107e****",
    "productKey":"al12345****",
    "deviceName":"deviceName1234",
    "gmtCreate":1510799670074,
    "deviceType":"Ammeter",
    "items":{
        "Power":{
            "value":"on",
            "time":1510799670074
        },
        "Position":{
            "time":1510292697470,
            "value":{
                "latitude":39.9,
                "longitude":116.38
            }
        }
    }
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|iotId|String|The ID of the device in IoT Platform.|
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|deviceType|String|The type of the device.|
|items|Object|The data that is submitted by the device.|
|Power|String|The name of the property. For more information about the properties, see the TSL of the product.|
|Position|String|The name of the property. For more information about the properties, see the TSL of the product.|
|value|Defined based on TSL|The value of the property.|
|time|Long|The time when the property was generated. If the device does not submit the timestamp, the timestamp generated when IoT Platform receives the message is used by default.|
|gmtCreate|Long|The time when the message was generated.|

## Submit device events

Topic: `/${productKey}/${deviceName}/thing/event/${tsl.event.identifier}/post`

You can use this topic to retrieve the events that are submitted by devices.

Data format:

```
{
    "identifier":"BrokenInfo",
    "name":"Damage rate report",
    "type":"info",
    "iotId":"4z819VQHk6VSLmmBJfrf00107e****",
    "productKey":"X5eCzh6****",
    "deviceName":"5gJtxDVeGAkaEztpisjX",
    "gmtCreate":1510799670074,
    "value":{
        "Power":"on",
        "Position":{
            "latitude":39.9,
            "longitude":116.38
        }
    },
    "time":1510799670074
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|iotId|String|The ID of the device in IoT Platform.|
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|type|String|The type of the event. For more information about the event types, see the TSL of the product.|
|value|Object|The parameters of the event.|
|Power|String|The name of the event parameter.|
|Position|String|The name of the event parameter.|
|time|Long|The time when the event was generated. If the device does not submit the timestamp, the timestamp generated when IoT Platform receives the message is used by default.|
|gmtCreate|Long|The time when the message was generated.|

## Submit lifecycle changes

Topic: `/${productKey}/${deviceName}/thing/lifecycle`

You can use this topic to retrieve notifications when devices are created, deleted, enabled, or disabled.

Data format:

```
{
    "action" : "create|delete|enable|disable",
    "iotId" : "4z819VQHk6VSLmmBJfrf00107e****",
    "productKey" : "al5eCzh****",
    "deviceName" : "5gJtxDVeGAkaEztpisjX",
    "deviceSecret" : "", 
    "messageCreateTime": 1510292739881 
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|action|String|-   create: creates a device
-   delete: deletes a device
-   enable: enables a device
-   disable: disables a device |
|iotId|String|The ID of the device in IoT Platform.|
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|deviceSecret|String|The DeviceSecret of the device. This parameter is included only if the action parameter is set to create.|
|messageCreateTime|Integer|The timestamp when the message was generated. Unit: milliseconds.|

## Submit topology changes

Topic: `/${productKey}/${deviceName}/thing/topo/lifecycle`

You can use this topic to retrieve notifications when topological relationships between sub-devices and gateways are created or deleted.

Data format:

```
{
    "action" : "create|delete|enable|disable",
    "gwIotId": "dfaejVQHk6VSLmmBJfrf00107e****",
    "gwProductKey": "al5eCzh****",
    "gwDeviceName": "deviceName1234",
    "devices": [ 
        {
          "iotId": "4z819VQHk6VSLmmBJfrf00107e****",
          "productKey": "ala4Czh****",
          "deviceName": "deviceName1234"
       }
    ],

    "messageCreateTime": 1510292739881 
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|action|String|-   create: creates a topology
-   delete: deletes a topology
-   enable: enables a topology
-   disable: disables a topology |
|gwIotId|String|The ID of the gateway in IoT Platform.|
|gwProductKey|String|The ProductKey of the product to which the gateway belongs.|
|gwDeviceName|String|The name of the gateway.|
|devices|Object|The list of sub-devices whose topologies changed.|
|iotId|String|The ID of the sub-device in IoT Platform.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|
|deviceName|String|The name of the sub-device.|
|messageCreateTime|Integer|The timestamp when the message was generated. Unit: milliseconds.|

## Submit information of detected sub-devices

Topic: `/${productKey}/${deviceName}/thing/list/found`

In some scenarios, gateways can detect sub-devices and submit sub-device information. You can use this topic to retrieve the submitted information.

Data format:

```
{
    "gwIotId":"dfaew9VQHk6VSLmmBJfrf00107e****",
    "gwProductKey":"al12345****",
    "gwDeviceName":"deviceName1234",
    "devices":[
        {
            "iotId":"4z819VQHk6VSLmmBJfrf00107e****",
            "productKey":"alr56g9****",
            "deviceName":"deviceName1234"
        }
    ]
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|gwIotId|String|The ID of the gateway in IoT Platform.|
|gwProductKey|String|The ProductKey of the product to which the gateway belongs.|
|gwDeviceName|String|The name of the gateway.|
|devices|Object|The list of sub-devices that are detected by the gateway.|
|iotId|String|The ID of the sub-device in IoT Platform.|
|productKey|String|The ProductKey of the product to which the sub-device belongs.|
|deviceName|String|The name of the sub-device.|

## Submit responses to downstream requests

Topic: `/${productKey}/${deviceName}/thing/downlink/reply/message`

You can use this topic to retrieve the results that are returned after devices process downstream requests. IoT Platform sends the downstream requests to devices in an asynchronous manner. If errors occur when IoT Platform sends the downstream requests, you can also use this topic to retrieve error messages.

Data format:

```
{
    "gmtCreate":1510292739881,
    "iotId":"4z819VQHk6VSLmmBJfrf00107e****",
    "productKey":"al12355****",
    "deviceName":"deviceName1234",
    "requestId":1234,
    "code":200,
    "message":"success",
    "topic":"/sys/al12355****/deviceName1234/thing/service/property/set",
    "data":{

    }
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|gmtCreate|Long|The timestamp. The time is displayed in UTC.|
|iotId|String|The ID of the device in IoT Platform.|
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|requestId|Long|The ID of the request.|
|code|Integer|The HTTP status code in the response. For more information, see [Table 1](#table_p23_n5l_b2b).|
|message|String|The response message. For more information, see [Table 1](#table_p23_n5l_b2b).|
|data|Object|The data that is returned by the device. If the device returns Alink data, you do not need to parse the data. If the device returns pass-through data, you must parse the data by using a data parsing script.|

|code|message|Description|
|:---|:------|:----------|
|200|success|The message returned because the request is successful.|
|400|request error|The error message returned because an internal error has occurred during request processing.|
|460|request parameter error|The error message returned because the request parameter is invalid and the device has failed to verify the parameter.|
|429|too many requests|The error message returned because the maximum number of requests in a specified period has been reached.|
|9200|device not actived|The error message returned because the device is not activated.|
|9201|device offline|The error message returned because the device is disconnected from IoT Platform.|
|403|request forbidden|The error message returned because the request is forbidden due to overdue payments.|

For more information about the solutions to the errors, see [Error codes for device SDKs](/intl.en-US/Device Management/Error codes for device SDKs.md).

## Submit historical properties

Topic: `/${productKey}/${deviceName}/thing/event/property/history/post`

You can use this topic to retrieve historical properties that are submitted by devices.

Data format:

```
{
    "iotId":"4z819VQHk6VSLmmBJfrf00107e****",
    "productKey":"12345****",
    "deviceName":"deviceName1234",
    "gmtCreate":1510799670074,
    "deviceType":"Ammeter",
    "items":{
        "Power":{
            "value":"on",
            "time":1510799670074
        },
        "Position":{
            "time":1510292697470,
            "value":{
                "latitude":39.9,
                "longitude":116.38
            }
        }
    }
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|iotId|String|The ID of the device in IoT Platform.|
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|gmtCreate|Long|The time when the message was generated.|
|deviceType|String|The type of the TSL feature. For more information, see the TSL of the product.|
|items|Object|The data that is submitted by the device.|
|Power|String|The name of the property. For more information about the properties, see the TSL of the product.|
|Position|String|The name of the property. For more information about the properties, see the TSL of the product.|
|value|Defined based on TSL|The value of the property.|
|time|Long|The time when the property was generated. If the device does not submit the timestamp, the timestamp generated when IoT Platform receives the message is used by default.|

## Submit historical events

Topic: `/${productKey}/${deviceName}/thing/event/${tsl.event.identifier}/history/post`

You can use this topic to retrieve historical events that are submitted by devices.

Data format:

```
{
    "identifier":"BrokenInfo",
    "name":"Damage rate report",
    "type":"info",
    "iotId":"4z819VQHk6VSLmmBJfrf00107e****",
    "productKey":"X5eCzh6****",
    "deviceName":"5gJtxDVeGAkaEztpisjX",
    "gmtCreate":1510799670074,
    "value":{
        "Power":"on",
        "Position":{
            "latitude":39.9,
            "longitude":116.38
        }
    },
    "time":1510799670074
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|identifier|String|The identifier of the event.|
|name|String|The name of the event.|
|type|String|The type of the event. For more information about the event types, see the TSL of the product.|
|iotId|String|The ID of the device in IoT Platform.|
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|gmtCreate|Long|The time when the message was generated.|
|value|Object|The parameters of the event.|
|Power|String|The name of the event parameter.|
|Position|String|The name of the event parameter.|
|time|Long|The time when the event was generated. If the device does not submit the timestamp, the timestamp generated when IoT Platform receives the message is used by default.|

## Submit firmware update status

Topic: `/${productKey}/${deviceName}/ota/upgrade`

You can use this topic to retrieve notifications when firmware updates succeed or fail.

**Note:** If a device has a pending update task and you initiate another batch update task on the device, the latter update task fails. In this case, a notification generated when the latter update task fails is not submitted to IoT Platform.

Data format:

```
{
    "iotId": "4z819VQHk6VSLmmBJfrf00107e****",
    "productKey": "X5eCzh6****",
    "deviceName": "deviceName1234",
    "status": "SUCCEEDED|FAILED",
    "messageCreateTime": 1571323748000,
    "srcVersion": "1.0.1",
    "destVersion": "1.0.2",
    "desc": "success",
    "jobId": "wahVIzGkCMuAUE2gDERM02****",
    "taskId": "y3tOmCDNgpR8F9jnVEzC01****"
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|---------|----|-----------|
|iotId|String|The ID of the device.|
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|status|String|The status of the update. -   SUCCEEDED: The update is successful.
-   FAILED: The update failed. |
|messageCreateTime|Long|The timestamp when the message was generated. Unit: milliseconds.|
|srcVersion|String|The firmware version before the update.|
|destVersion|String|The destination firmware version of the update.|
|desc|String|The description of the update.|
|jobId|String|The ID of the update batch.|
|taskId|String|The ID of the device update record.|

## Submit OTA module versions

Topic: `/${productKey}/${deviceName}/ota/version/post`

You can use this topic to retrieve OTA module versions that are submitted by devices. If the submitted versions are different from the previous versions, the messages that are sent to the topic are forwarded.

Data format:

```
{
    "iotId": "4z819VQHk6VSLmmBJfrf00107e****",
    "deviceName": "deviceName1234",
    "productKey": "X5eCzh6****",
    "moduleName": "BarcodeScanner",
    "moduleVersion": "1.0.3",
    "messageCreateTime": 1571323748000
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|---------|----|-----------|
|iotId|String|The ID of the device.|
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|moduleName|String|The name of the firmware module.|
|moduleVersion|String|The version of the firmware module.|
|messageCreateTime|Long|The timestamp when the message was generated. Unit: milliseconds.|

## Submit status of OTA update batches

Topic: `/${productKey}/${packageId}/${jobId}/ota/job/status`

You can use this topic to retrieve the status of OTA update batches.

Data format:

```
{
    "productKey": "X5eCzh6****",
    "moduleName": "BarcodeScanner",
    "packageId": "wahVIzGkCMuAUE2xxxx",
    "jobId": "wahVIzGkCMuAUE2gDERM02****",
    "status": "IN_PROGRESS",
    "messageCreateTime": 1571323748000
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|---------|----|-----------|
|productKey|String|The ProductKey of the product to which the device belongs.|
|moduleName|String|The name of the firmware module.|
|packageId|String|The ID of the update package. The value of this parameter is the same as the value of the FirmwareId parameter that is returned when you call the [CreateOTAFirmware](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/CreateOTAFirmware.md) operation to create a firmware update package.|
|jobId|String|The ID of the update batch.|
|status|String|The status of the update batch. Valid values:-   PLANNED: The update is not started.
-   IN\_PROGRESS: The update is in progress.
-   COMPLETED: The update is complete.
-   CANCELED: The update is canceled. |
|messageCreateTime|Long|The timestamp when the message was generated. Unit: milliseconds.|

## Submit device tag changes

Topic: `/${productKey}/${deviceName}/thing/deviceinfo/update`

You can use this topic to retrieve new device tags that are submitted by devices.

Data format:

```
{
  "iotId": "4z819VQHk6VSLmmBJfrf00107e****",
  "productKey": "X5eCzh6****",
  "deviceName": "deviceName1234",
  "value": [
    {
      "attrKey": "tagKey",
      "attrValue": "tagValue"
    }
  ],
  "messageCreateTime": 1510799670074
}
```

The following table lists the parameters.

|Parameter|Type|Description|
|---------|----|-----------|
|iotId|String|The ID of the device.|
|productKey|String|The ProductKey of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|value|List|The data of the tag.|
|attrKey|String|The key of the tag.|
|attrValue|String|The value of the tag.|
|messageCreateTime|Long|The timestamp when the message was generated. Unit: milliseconds.|

