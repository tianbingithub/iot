# Disable and delete devices

Gateways can disable and delete their sub-devices.

## Disable devices​

Downstream

-   Request topic: `/sys/{productKey}/{deviceName}/thing/disable`
-   Reply topic: `/sys/{productKey}/{deviceName}/thing/disable_reply`

This topic disables a device connection. IoT Platform publishes messages to this topic asynchronously, and the devices subscribe to this topic. Gateways can subscribe to this topic to disable the corresponding sub-devices.

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.disable"
            
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. IoT Platform generates IDs for downstream messages.|
|version|String|Protocol version. Currently, the value is 1.0.|
|params|Object|Request parameters. Leave empty.|
|method|String|Request method.|
|code|Integer|Results information. For more information, see [Common codes on devices](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Common codes on devices.md)|

## Enable devices​

Downstream

-   Request Topic: `/sys/{productKey}/{deviceName}/thing/enable`
-   Reply topic: `/sys/{productKey}/{deviceName}/thing/enable_reply`

This topic enables a device connection. IoT Platform publishes messages to this topic asynchronously, and the devices subscribe to this topic. Gateways can subscribe to this topic to enable the corresponding sub-devices.

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.enable"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. IoT Platform generates IDs for downstream messages.|
|version|String|Protocol version. Currently, the value is 1.0.|
|params|Object|Request parameters. Leave empty.|
|method|String|Request method.|
|code|Integer|Result code. For more information, see the common codes.|

## Delete devices

Downstream

-   Request topic: `/sys/{productKey}/{deviceName}/thing/delete`
-   Reply topic: `/sys/{productKey}/{deviceName}/thing/delete_reply`

This topic deletes a device connection. IoT Platform publishes messages to this topic asynchronously, and the devices subscribe to this topic. Gateways can subscribe to this topic to delete the corresponding sub-devices.

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.delete"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. IoT Platform generates IDs for downstream messages.|
|version|String|Protocol version. Currently, the value is 1.0.|
|params|Object|Request parameters. Leave empty.|
|method|String|Request method.|
|code|String|Result code. For more information, see the common codes.|

