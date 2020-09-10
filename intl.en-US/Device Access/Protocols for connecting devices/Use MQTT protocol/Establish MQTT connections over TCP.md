---
keyword: [MQTT, Internet of Things, IoT, IoT Platform, TCP, device connection, communication]
---

# Establish MQTT connections over TCP

This article describes how to establish MQTT connections over TCP by using an MQTT client.

When you configure an MQTT CONNECT packet, note the following issues:

-   If a device certificate \(ProductKey, DeviceName, and DeviceSecret\) or a combination of ProductKey, DeviceName, ClientID, and DeviceToken is used to connect multiple physical devices, clients may frequently go online and offline. This is because when a new device initiates an authentication request to IoT Platform, the original device is forced to go offline. After the device goes offline, it automatically tries to re-establish a connection.
-   In the MQTT connection mode, the [Link SDK](https://www.alibabacloud.com/help/doc-detail/96627.htm) automatically tries to re-establish a connection after the device is disconnected. You can view device behaviors by using Log Service.

## Connect an MQTT client to IoT Platform

1.  Optional. We recommend that you use the TLS protocol for encryption.

    -   The Link SDK integrates the TLS encryption feature, which eliminates your needs of configuration.
    -   If you are developing your own device SDK, you must [download the root certificate](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt). For more information about how to use the root certificate, see [mbed TLS](https://tls.mbed.org/kb/how-to/mbedtls-tutorial).
2.  Connect the MQTT client to the server. For more information about the connection method, see [Open-source MQTT client](https://github.com/mqtt/mqtt.github.io/wiki/libraries).

    For more information about the MQTT protocol, see [MQTT documentation](http://mqtt.org/).

    **Note:** Alibaba Cloud does not provide technical support for third-party code.

3.  Establish an MQTT connection.

    We recommend that you use the Link SDK to connect the device to IoT Platform. If you use your own device SDK for connection, you must specify the following parameters.

    |Endpoint|    -   To view the endpoint of the instance that you purchased, perform the following steps: Log on to the IoT Platform console. In the left-side navigation pane, click **Instances**. On the page that appears, click **View** in the Actions column of the instance. On the Instance Details page, you can view the endpoint.
    -   The endpoint for public instances is `${YourProductKey}.iot-as-mqtt.${YourRegionId}.aliyuncs.com:1883`.
        -   $\{YourProductKey\}: Replace this variable with the ProductKey of the product to which the device belongs. You can obtain the ProductKey on the Device Details page of the IoT Platform console.
        -   $\{YourRegionId\}: Replace this variable with your region ID. For information about region IDs, see [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm). |
    |Variable header: keep-alive|The CONNECT command must include a keep-alive time. Valid values of the keep-alive time: 30 to 1,200 seconds. If no response is received from a device before the keep-alive time expires, IoT Platform rejects the connection request. We recommend that you set a value that is greater than 300 seconds. If a network is intermittent, set the keep-alive time to a value that is close to 1,200 seconds.|
    |Parameters in an MQTT CONNECT message|    -   Unique-certificate-per-device authentication and pre-registration unique-certificate-per-product authentication: Use the device certificate \(ProductKey, DeviceName, and DeviceSecret\) to connect the device to IoT Platform.

        ```
mqttClientId: clientId+"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"
mqttUsername: deviceName+"&"+productKey
mqttPassword: sign_hmac(deviceSecret,content)
        ```

        -   mqttClientId: Extended parameters are placed between vertical bars \(`|`\).
        -   clientId: the ID of the client. We recommend that you use the MAC address or serial number \(SN\) of the device as the client ID. The client ID cannot exceed 64 characters in length.
        -   securemode: the current security mode. Valid values: 2 \(direct TLS connection\) and 3 \(direct TCP connection\).
        -   signmethod: the signature algorithm. Valid values: hmacmd5, hmacsha1, hmacsha256, and sha256. Default value: hmacmd5.
        -   timestamp: the current time, in milliseconds. This parameter is optional.
        -   mqttPassword: the password. Calculation method: Alphabetically sort the parameters that are submitted to the server and encrypt the parameters based on the specified signature algorithm. For more information about the signature calculation example, see [Examples of signing MQTT connections](/intl.en-US/Device Access/Protocols for connecting devices/Use MQTT protocol/Examples of creating signatures for MQTT connections.md).
        -   content: a concatenated string of the parameters that are submitted to the server. These parameters include productKey, deviceName, timestamp, and clientId. The parameters are sorted in alphabetical order and concatenated without delimiters.
Example

Assume that the following values are specified: `clientId=12345, deviceName=device, productKey=pk, timestamp=789, signmethod=hmacsha1, deviceSecret=secret`. The parameters in an MQTT CONNECT message that is sent over TCP is shown in the following code:

        ```
mqttclientId=12345|securemode=3,signmethod=hmacsha1,timestamp=789|
mqttUsername=device&pk
mqttPassword=hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString(); 
        ```

The encrypted password is a hexadecimal string that is converted from a binary string. The result is shown in the following code:

        ```
FAFD82A3D602B37FB0FA8B7892F24A477F85****
        ```

    -   Preregistration-free unique-certificate-per-product authentication: Use ProductKey, DeviceName, ClientID, and DeviceToken to connect the device to IoT Platform.

        ```
mqttClientId: clientId+"|securemode=-2,signmethod=hmacsha1,timestamp=132323232|"
mqttUsername: deviceName+"&"+productKey
mqttPassword: deviceToken
        ```

        -   mqttClientId: Extended parameters are placed between vertical bars \(`|`\).
        -   clientId, deviceToken: the ClientID and DeviceToken that are obtained when the device is dynamically registered. For more information, see [MQTT-based dynamic registration]().
        -   securemode: the current security mode. If you use preregistration-free unique-certificate-per-product authentication, the value is -2.
        -   signmethod: the signature algorithm. Valid values: hmacmd5, hmacsha1, hmacsha256, and sha256. Default value: hmacmd5.
        -   timestamp: the current time, in milliseconds. This parameter is optional. |


## Examples

For information about examples of using open-source MQTT clients to access IoT Platform, see the following topics:

-   [Using Paho MQTT Go client](/intl.en-US/Best Practices/Device access/Access IoT Platform by using Paho/Using Paho MQTT Go client.md)
-   [Using Paho MQTT C\# client](/intl.en-US/Best Practices/Device access/Access IoT Platform by using Paho/Using Paho MQTT C# client.md)
-   [Using Paho MQTT C client](/intl.en-US/Best Practices/Device access/Access IoT Platform by using Paho/Using Paho MQTT C client.md)
-   [Using Paho MQTT Java client](/intl.en-US/Best Practices/Device access/Access IoT Platform by using Paho/Using Paho MQTT Java client.md)
-   [Using Paho MQTT Android client](/intl.en-US/Best Practices/Device access/Access IoT Platform by using Paho/Using Paho MQTT Android client.md)

## MQTT keep-alive

In a keep-alive interval, the device must send at least one message, including ping requests.

If IoT Platform does not receive a message in a keep-alive interval, the device is disconnected from IoT Platform and needs to reconnect to the server.

Valid values of the keep-alive time: 30 to 1,200 seconds. We recommend that you set a value that is greater than 300 seconds.

