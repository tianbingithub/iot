---
keyword: [device registration, Alink, Alink protocol, connection, authentication, Internet of Things, IoT, IoT Platform, data structure, message structure, request topic, response topic]
---

# Device identity registration

Before you connect a device to IoT Platform, you must register the device identity to identify the device on IoT Platform.

The following methods are available for identity registration:

-   Unique-certificate-per-device. Create a device in IoT Platform, obtain the device certificate, and then use the certificate information as the unique identifier. The certificate includes the ProductKey, DeviceName, and DeviceSecret. Burn the certificate information onto the firmware. After the device is connected to IoT Platform, the device starts to communicate with IoT Platform.
-   Dynamic registration. You can perform dynamic registration for sub-devices and directly connected devices. Dynamic registration based on the unique-certificate-per-product authentication is used to register the directly connected devices.
    -   To dynamically register a directly connected device based on the unique-certificate-per-product authentication, perform the following steps:
        1.  Create a device in IoT Platform and obtain the product certificate that includes the ProductKey and ProductSecret. When you create the device, set the DeviceName parameter to the serial number or MAC address of the sub-device.
        2.  Enable dynamic registration in the IoT Platform console.
        3.  Burn the product certificate onto the firmware.
        4.  The device initiates an authentication request to IoT Platform. If the authentication succeeds, IoT Platform assigns a DeviceSecret to the device.
        5.  The device uses the device certificate to establish a connection with IoT Platform.
    -   To dynamically register a sub-device, perform the following steps:
        1.  Create a sub-device in IoT Platform and obtain the ProductKey. When you create the sub-device, set the DeviceName parameter to the serial number or MAC address of the sub-device.
        2.  Enable dynamic registration in the console.
        3.  Burn the ProductKey onto the firmware or the gateway.
        4.  The gateway initiates an authentication request to IoT Platform on behalf of the sub-device. If the authentication succeeds, IoT Platform assigns a DeviceSecret to the sub-device.

## Dynamically register a sub-device

The gateway device can perform dynamic registration for the sub-device by sending an upstream request to IoT Platform. If the sub-device registration succeeds, IoT Platform returns the device certificate of the sub-device.

The following topics are used when the device sends requests to IoT Platform and IoT Platform sends responses to the device:

-   request topic: `/sys/{productKey}/{deviceName}/thing/sub/register`
-   response topic: `/sys/{productKey}/{deviceName}/thing/sub/register_reply`

Sample request:

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "deviceName": "deviceName1234",
      "productKey": "a1234******"
    }
  ],
  "method": "thing.sub.register"
}
```

Sample response:

```
{
  "id": "123",
  "code": 200,
  "data": [
    {
      "iotId": "12344",
      "productKey": "a1234******",
      "deviceName": "deviceName1234",
      "deviceSecret": "xxxxxx"
    }
  ]
}
```

The following table describes the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. Valid values: 0 to 4294967295. The message ID must be unique for the device.|
|version|String|The version of the protocol. Valid value: 1.0.|
|params|List|The parameters used for dynamic registration.|
|deviceName|String|The name of the sub-device.|
|productKey|String|The key of the product to which the sub-device belongs.|
|iotId|String|The unique identifier of the sub-device.|
|deviceSecret|String|The secret of the sub-device.|
|method|String|The request method. The value is set to `thing.sub.register`.|
|code|Integer|The result.|

The following table describes the error codes.

|HTTP status code|Error message|Description|
|:---------------|:------------|:----------|
|460|request parameter error|The error message returned because one or more request parameters are invalid.|
|6402|topo relation cannot add by self|The error message returned because a device cannot be added as a sub-device of itself.|
|401|request auth error|The error message returned because the signature verification has failed.|

## Dynamically register a directly connected device based on the unique-certificate-per-product authentication

Directly connected devices send HTTP requests to perform dynamic registeration. Make sure that you have enabled dynamic registration based on unique-certificate-per-product in the IoT Platform console.

-   URL template: `https://iot-auth.cn-shanghai.aliyuncs.com/auth/register/device`
-   HTTP method: POST.

Sample request:

```
POST /auth/register/device  HTTP/1.1
Host: iot-auth.cn-shanghai.aliyuncs.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 123
productKey=a1234******&deviceName=deviceName1234&random=567345&sign=adfv123hdfdh&signMethod=HmacMD5
```

Sample response:

```
{
  "code": 200,
  "data": {
    "productKey": "a1234******",
    "deviceName": "deviceName1234",
    "deviceSecret": "adsfw******"
  },
  "message": "success"
}
```

The following table describes the parameters.

|Parameter|Type|Description|
|:--------|:---|:----------|
|Method|String|POST|
|Host|String|The endpoint: `iot-auth.cn-shanghai.aliyuncs.com`.|
|Content-Type|String|The encoding format of the upstream data that the device sends to IoT Platform.|
|productKey|String|The unique identifier of the product.|
|deviceName|String|The name of the device.|
|random|String|A random number.|
|sign|String|The signature. You can create a signature by using one of the following methods:

1.  Sort all parameters that are submitted to IoT Platform \(excluding the sign and signMethod parameters\) in alphabetical order, and concatenate the parameters and values in sequence without using concatenation operators.
2.  Use the signature method that is specified in the signMethod parameter and set the secret key of the algorithm to ProductSecret.

Example:

```
hmac_sha1(productSecret, deviceNamedeviceName1234productKeya1234******random123)
``` |
|signMethod|String|The signature method. Valid values: hmacmd5, hmacsha1, and hmacsha256.|
|code|Integer|The result.|
|deviceSecret|String|The secret of the device.|

