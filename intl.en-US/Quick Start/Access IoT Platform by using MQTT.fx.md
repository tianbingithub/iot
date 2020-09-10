# Access IoT Platform by using MQTT.fx

This article uses MQTT.fx as an example to describe the method for using a third-party MQTT client to connect to IoT Platform. MQTT.fx is a MQTT client that is written in Java language and based on Eclipse Paho. It supports subscribing to messages and publishing messages through topics.

## Prerequisites

You have created products and devices in the [IoT Platform console](https://iot.console.aliyun.com), and have got the ProductKey, DeviceName, and DeviceSecret of the devices. When you set the connection parameters for MQTT.fx, you will use the values of the ProductKey, DeviceName, and DeviceSecret. See [Create a product](/intl.en-US/Device Access/Create a product.md), [Create a device](/intl.en-US/Device Access/Create devices/Create a device.md), and [Create multiple devices at a time](/intl.en-US/Device Access/Create devices/Create multiple devices at a time.md)for help when creating products and devices.

## Procedure

1.  Download and install the MQTT.fx software.

    Download the MQTT.fx software for Windows from [MQTT.fx website](https://mqttfx.jensd.de/index.php/download).

2.  Open MQTT.fx, and click the settings icon.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7694.png)

3.  Set the connection parameters.

    Currently, two types of connection modes are supported: TCP and TLS. These two modes only differ in settings of Client ID and SSL/TLS.

    The procedure is as follows:

    1.  Enter basic information. See the following table for parameter descriptions.

        You can keep the default parameters for General, or set the values according to your needs.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7698.png)

        |Parameter|Description|
        |:--------|:----------|
        |Profile Name|Enter a custom profile name.|
        |Profile Type|Select **MQTT Broker**.|
        |Broker Address|        -   To view the endpoint of the instance that you purchased, perform the following steps: Log on to the IoT Platform console. In the left-side navigation pane, click **Instances**. On the page that appears, click **View** in the Actions column of the instance. On the Instance Details page, you can view the endpoint.
        -   The endpoint for public instances is `${YourProductKey}.iot-as-mqtt. ${YourRegionId}.aliyuncs.com`.
            -   $\{YourProductKey\}: Replace this variable with the ProductKey of the product to which the device belongs. You can obtain the ProductKey on the Device Details page of the IoT Platform console.
            -   $\{YourRegionId\}: Replace this variable with your region ID. For information about region IDs, see [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm). |
        |Broker Port|Set to 1883.|
        |Client ID|Enter a value in the format of $\{clientId\}\|securemode=3,signmethod=hmacsha1\|. Example: `12345|securemode=3,signmethod=hmacsha1|`. The parameters are described as follows:         -   $\{clientId\} is a custom client ID. It can be any value within 64 characters. We recommend that you use the MAC address or SN code of the device as the value of clientId.
        -   securemode is the security mode of the connection. If you use the TCP mode, set it as securemode=3; if you use the TLS mode, set it as securemode=2.
        -   signmethod is the signature method that you want to use. IoT Platform supports hmacmd5 and hmacsha1.
**Note:** Do not click **Generate** after you enter the Client ID information. |

    2.  Click **User Credentials**, and enter your **User Name** and **Password**.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7699.png)

        |Parameter|Description|
        |:--------|:----------|
        |User Name|It must be the device name directly followed by the character "&" and the product key. Format: $\{YourDeviceName\}&$\{YourProductKey\}. For example, device&fOAt5H5TOWF.|
        |Password|You must enter an encrypted value of the input parameters.         -   IoT Platform provides a [Password Generator](https://files.alicdn.com/tpsservice/88413c66e471bec826257781969d1bc7.zip) for you to generate one easily.

Parameters in the password generator:

            -   productKey: The unique identifier of the product to which the device belongs. You can view this information on the device details page in the console.
            -   deviceName: The name of the device. You can view this information on the device details page in the console.
            -   deviceSecret: The device secret. You can view this information on the device details page in the console.
            -   timestamp: \(Optional\) Timestamp of the current system time.
            -   clientId: The custom client ID, which must be the same as the value of $\{clientId\} in **Client ID**.
            -   method: The signature algorithm, which must be the same as the value of signmethod in **Client ID**.
        -   You can also encrypt a password by yourself.

Generate a password manually:

            1.  Sort and join the parameters.

Sort and join the parameters clientId, deviceName, productKey, and timestamp in a lexicographical order. \(If you have not set a timestamp, do not include timestamp in the string.\) Joint string example: `clientId12345deviceNamedeviceproductKeyfOAt5H5TOWF`

            2.  Encrypt.

Use the deviceSecret of the device as the secret key to encrypt the joint string by the signature algorithm defined in **Client ID**.

Suppose the deviceSecret of the device is abc123, the encryption format is `hmacsha1(abc123,clientId12345deviceNamedeviceproductKeyfOAt5H5TOWF)`. |

    3.  If you use TLS connection mode, you are required to set information for SSL/TLS. SSL/TLS settings are not required when the connection mode is TCP.

        Check the box for **Enable SSL/TLS**, and select **TLSv1** as the protocol.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7734.png)

    4.  Enter all the required information, and then click **OK**.
4.  Click **Connect** to connect to IoT Platform.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7735.png)


## Message communication test

Test whether MQTT.fx and IoT Platform are successfully connected.

1.  In MQTT.fx, click **Subscribe**.
2.  Enter a topic of the device, and then click **Subscribe**.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7736.png)

    After you have successfully subscribed to a topic, it is displayed in the topic list.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7737.png)

3.  In the [IoT Platform console](https://iot.console.aliyun.com), in the **Topic List** of the Device Details page, click the **Publish** button of the topic that you have subscribed to.
4.  Enter message content, and click **OK**.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7738.png)

5.  Go back to MQTT.fx to check if the message has been received.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7739.png)


## View logs

In MQTT.fx, click **Log** to view the operation logs and error logs.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4447749951/p7740.png)

