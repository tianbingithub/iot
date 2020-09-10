---
keyword: [HTTPS, HTTP, Internet of Things, IoT, IoT Platform, device connection, communication]
---

# Establish connections over HTTP

You can connect a device to IoT Platform over HTTP. Only HTTPS is supported. This article describes how to connect a device to IoT Platform over HTTP.

## Limits

-   The device connection over HTTP feature is available only in the China \(Shanghai\) region.
-   Only HTTPS is supported.
-   The device connection over HTTP feature is used only for devices to report data. A device can report a maximum of 128 KB of data at a time.
-   The standard for HTTP topics and [MQTT topics](/intl.en-US/Device Access/Protocols for connecting devices/Use MQTT protocol/MQTT standard.md) is the same. If you connect a device to IoT Platform over HTTP, you can use MQTT topics for message communication between the device and IoT Platform. When you connect a device to IoT Platform over HTTP, the device reports data to a topic in the `${endpoint}/topic/${topic}` format. Request parameters cannot be specified after a question mark `(?).`
-   Only the POST request method is supported.
-   A device token will expire after seven days. Make sure that you have solutions to token expiration issues.

## Procedure

The communication process includes performing device authentication to obtain a device token and using the obtained token for data reporting.

1.  Authenticate the device to obtain the device token.

    Endpoint:

    -   The endpoint of public instances that reside in the China \(Shanghai\) region is `https://iot-as-http.cn-shanghai.aliyuncs.com`.
    -   You can purchase instances in the China \(Shanghai\) and regions. To view the endpoint of the instance that you purchased, perform the following steps: Log on to the IoT Platform console. In the left-side navigation pane, click **Instances**. On the page that appears, click **View** in the Actions column of the instance. On the Instance Details page, you can view the endpoint.
    Sample authentication request:

    ```
    POST /auth HTTP/1.1
    Host: ${YourEndpoint}
    Content-Type: application/json
    body: {"version":"default","clientId":"mylight1000002","signmethod":"hmacsha1","sign":"4870141D4067227128CBB4377906C3731CAC221C","productKey":"ZG1EvTE****","deviceName":"NlwaSPXsCpTQuh8FxBGH","timestamp":"1501668289957"}
    ```

    |Parameter|Description|
    |:--------|:----------|
    |Method|The request method. Valid value: POST.|
    |URL|The URL. Only HTTPS is supported. Valid value: /auth.|
    |Host|The endpoint.|
    |Content-Type|The MIME type of upstream data that the device reports to IoT Platform. Valid value: application/json. If another MIME type is specified, an error is returned.|
    |body|The device authentication information in the JSON format. For more information, see [Table 2](#table_p10_cdu_knj).|

    |Parameter|Required|Description|
    |:--------|:-------|:----------|
    |productKey|Yes|The product key. You can obtain the product key from the Device Details page of the IoT Platform console.|
    |deviceName|Yes|The device name. You can obtain the device name from the Device Details page of the IoT Platform console.|
    |clientId|Yes|The client ID. The client ID must be 1 to 64 characters in length. We recommend that you use the MAC address or SN of the device as the value of the clientId parameter.|
    |timestamp|No|The timestamp. A request is valid within 15 minutes after the timestamp is created. The timestamp is in the numeric format. This value is a UNIX timestamp representing the number of milliseconds that have elapsed since the epoch time January 1, 1970, 00:00:00 UTC.|
    |sign|Required|The signature. The signature is calculated by a function in the `hmacmd5(deviceSecret,content)` format.

The value of content is a string that contains all the parameters to be reported to IoT Platform, except for the version, sign, and signmethod parameters. These parameters are sorted in alphabetical order and spliced without a splicing symbol.

Example:

If the clientId parameter is set to 127.0.0.1, the deviceName parameter is set to http\_test, the productKey parameter is set to a1FHTWxQ\*\*\*\*, the timestamp parameter is set to 1567003778853, the signmethod parameter is set to hmacmd5, and the deviceSecret parameter is set to 89VTJylyMRFuy2T3sywQGbm5Hmk1\*\*\*\*, use the following function to calculate the signature:

`hmacmd5("89VTJylyMRFuy2T3sywQGbm5Hmk1****","clientId127.0.0.1deviceNamehttp_testproductKeya1FHTWxQ****timestamp1567003778853").toHexString();`

The `toHexString()` method is used to convert a set of four-bit binary digits that form the binary result into a hexadecimal string. The hexadecimal string is case-insensitive. For example, an array that includes the result in the decimal format is \[60 68 -67 -7 -17 99 30 69 117 -54 -58 -58 103 -23 113 71\]. After the array is converted into a hexadecimal string, it is 3C44BDF9EF631E4575CAC6C667E97147. |
    |signmethod|No|The signature algorithm. Valid values: hmacmd5 and hmacsha1. Default value: hmacmd5. |
    |version|No|The version number. Default value: default.|

    Sample response:

    ```
    body:
    {
      "code": 0,
      "message": "success",
      "info": {
        "token":  "6944e5bfb92e4d4ea3918d1eda39****"
      }
    }
    ```

    **Note:**

    -   Cache the returned token on the device.
    -   The token is required each time the device reports data to IoT Platform. If the token expires, you must re-authenticate the device to obtain another token.
    |code|message|Description|
    |:---|:------|:----------|
    |10000|common error|The error message returned because an unknown error occurred.|
    |10001|param error|The error message returned because one or more request parameters are invalid.|
    |20000|auth check error|The error message returned because the device failed to pass the authentication.|
    |20004|update session error|The error message returned because the device failed to be updated.|
    |40000|request too many|The error message returned because the system cannot process this number of requests. The throttling policy is applied.|

2.  Report data.

    Report data to a topic.

    To report data to a custom topic, perform the following steps: Log on to the IoT Platform console, go to the Product Details page of the product to which the device belongs. Click the Topic Categories tab, click the Custom Topics tag, and then click Edit Topic Category. In the dialog box that appears, set the required parameters to create a custom topic. For more information, see [Create a custom topic](/intl.en-US/Device Access/Topics/Create a topic category.md).

    For example, report data to a topic in the `/${YourProductKey}/${YourDeviceName}/user/pub` format. If a device name is device123 and product key is a1GFjLP\*\*\*\*, the device reports data to the `https://iot-as-http.cn-shanghai.aliyuncs.com/topic/a1GFjLP****/device123/pub` topic.

    Sample request for data reporting:

    ```
    POST /topic/${topic} HTTP/1.1
    Host: ${YourEndpoint}
    password:${token}
    Content-Type: application/octet-stream
    body: ${your_data}
    ```

    |Parameter|Description|
    |:--------|:----------|
    |Method|The request method. Valid value: POST.|
    |URL|`/topic/${topic}`. Replace the `${topic}` variable with the topic to which data is sent. Only HTTPS is supported.|
    |Host|The endpoint.|
    |password|This parameter is included in the request header. Set this parameter to the token that is returned after you call the auth service to authenticate the device.|
    |Content-Type|The MIME type of upstream data that the device reports to IoT Platform. Valid value: application/octet-stream. If another MIME type is specified, an error is returned.|
    |body|The data that is sent to the specified topic.|

    Sample response:

    ```
    body:
    {
      "code": 0,
      "message": "success",
      "info": {
        "messageId": 892687627916247040,
      }
    }
    ```

    |code|message|Description|
    |:---|:------|:----------|
    |10000|common error|The error message returned because an unknown error occurred.|
    |10001|param error|The error message returned because one or more request parameters are invalid.|
    |20001|token is expired|The error message returned because the token has expired. You must call the auth service to re-authenticate the device and obtain another token.|
    |20002|token is null|The error message returned because no token is specified in the request header.|
    |20003|check token error|The error message returned because the system failed to obtain the device identity information based on the token. You must call the auth service to re-authenticate the device and obtain another token.|
    |30001|publish message error|The error message returned because the device failed to report data.|
    |40000|request too many|The error message returned because the system cannot process this number of requests. The throttling policy is applied.|


