# Distribute devices

IoT Platform allows you to distribute devices across regions, instances, or accounts. After you distribute the devices, IoT Platform sends new endpoints to the devices. After the endpoint information is written to the devices, the devices can connect to IoT Platform by using the new endpoints. This eliminates the need of re-burning the device information.

## Scenarios

You can distribute devices in the following scenarios:

-   Device information burning: You do not need to hardcode the endpoints of instances that are deployed in different regions before devices leave the factory. A globally unique endpoint that does not contain the region information is burned onto all devices. After the devices leave the factory, you can distribute the devices to different regions by using the IoT Platform console. This way, the devices can connect to IoT Platform from different regions around the world.
-   Business migration: You can distribute devices across regions, instances, or accounts to meet the requirements of business migration.

Two distribution policies are supported:

-   Specified region: Devices are distributed to the specified region and instance. If you can confirm the destination region and instance to which the devices will be distributed, we recommend that you use this policy. This policy is more efficient.
-   Nearest access: Devices are distributed to different regions around the world. When you distribute the devices, you can select one or more regions and an instance from each region. Then, devices are connected to the nearest region based on their IP addresses.

**Note:**

-   You can distribute devices across all regions to which IoT Platform is accessible.
-   The nearest access policy is not applicable to device distribution cross accounts.
-   You can distribute devices multiple times.
-   After device distribution begins, you cannot cancel the operation.
-   If a device of a product is authorized or transferred, all devices of the product cannot be distributed.
-   After devices are distributed across regions and instances, you must switch to the destination region and instance in the IoT Platform console to configure firmware update. After devices are distributed across accounts, recipients must obtain firmware files from distributors to configure firmware update. For more information about firmware update, see [Push firmware to devices](/intl.en-US/Maintenance/Firmware update/Push firmware to devices.md).

## Device distribution items

You can distribute devices across instances, regions, and accounts.

After a device is distributed, portions of device data and product data are also distributed. The following table lists the device distribution items.

|Distribution item|Description|
|-----------------|-----------|
|Device|You can distribute a maximum of 1,000 devices at a time.|
|Device data|The device certificate is distributed and used to authenticate the connection between the device and a new instance. The certificate includes the ProductKey, DeviceName, and DeviceSecret. The following device data is not distributed:

-   Runtime data. Runtime data is generated when a device is running. The data includes device status, Thing Specification Language \(TSL\) data, device shadows, files, and logs. If a device is distributed, the device becomes inactive. After the device is re-connected to IoT Platform, the device is activated and can go online or offline. The runtime data of the device is retained in the original region, instance, and account. If the device is re-distributed to its original location, the data can still be used.
-   Data about topological relationships, tags, and groups. If a device is distributed, such data is cleared. |
|Product data|The product information, topic categories, TSL features, and data parsing scripts are distributed. However, recipients can only view the data of distributed products. Recipients cannot edit or delete the data on the Product Information, Topic Categories, Define Feature, or Data Parsing tabs of the Product Details page. In addition, recipients cannot create devices for the distributed products by using the IoT Platform console.|

## Procedure

1.  Create a product and a device in the product, and then obtain the device certificate in the IoT Platform console. For more information, see [Create a product](/intl.en-US/Device Access/Create a product.md), [Create a device](/intl.en-US/Device Access/Create devices/Create a device.md), and [Create multiple devices at a time](/intl.en-US/Device Access/Create devices/Create multiple devices at a time.md).

2.  Use the [SDK 4.x for C]() to develop the physical device. For more information about how to develop the Bootstrap features, see [Device provisioning service]().

    After you distribute the device by using the console, the device connects to IoT Platform based on the following process.

    1.  After the device is distributed, the device receives an MQTT message.

        The following topics are used when IoT Platform sends downstream MQTT requests and the device sends responses to IoT Platform:

        Request topic: `/sys/${productKey}/${deviceName}/thing/bootstrap/notify`.

        Sample request in the Alink JSON format:

        ```
        {
            "id": "XXX",
            "version": "1.0", 
            "method": "thing.bootstrap.notify", 
            "params": {
              "cmd": 0
            }
        }
        ```

        Response topic: `/sys/${productKey}/${deviceName}/thing/bootstrap/notify_reply`.

        Sample response in the Alink JSON format:

        ```
        {
            "id": "XXX",
            "code":200,
            "data" : {}
        }
        ```

    2.  The device calls the Bootstrap method that is encapsulated in the SDK and carries the ProductKey and DeviceName parameters to send a request to the globally unique endpoint: `https://iot-auth-global.aliyuncs.com`.
    3.  IoT Platform returns an endpoint based on the destination region and instance. If you set the Distribution Policy parameter to Nearest Access, IoT Platform determines the destination region and instance based on the IP address of the device.
    4.  The device uses the device certificate to calculate a signature and establishes a connection with IoT Platform by sending a Connect request over MQTT to the endpoint.
3.  After you distribute a device, perform one of the following operations based on the actual conditions.

    -   If the device is an existing device that cannot be distributed, you must upgrade its firmware to the version that is developed in the previous step. For more information about firmware update, see [Push firmware to devices](/intl.en-US/Maintenance/Firmware update/Push firmware to devices.md).

        **Note:**

        For existing devices that cannot be distributed, you must upgrade the firmware of the devices to enable distribution. Otherwise, the devices cannot connect to IoT Platform after distribution.

    -   If the device is a new device, you must burn the device certificate onto the device. For more information, see [Burn device certificate](/intl.en-US/Device Access/Devices retrieve certificates/Overview.md).
4.  Distribute the device to a specified region, instance, and account by using the IoT Platform console based on your business requirements.

    1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com/).

    2.  In the top navigation bar, select China \(Shanghai\) from the region name drop-down list.

        **Note:** This step is performed to use the device distribution feature, but not to specify the source or destination region for device distribution.

    3.  In the left-side navigation pane, choose **Resource Allocation** \> **Device Distribution**.

    4.  On the Device Distribution page, click **Device Distribution**.

    5.  Specify the parameters.

        The following table lists the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Distribution Method|The method that is used to distribute devices. Valid values:         -   **This Account**: Distribute the devices to another instance of the current account in another region.
        -   **Cross-account**: Distribute the devices to another account.

**Note:** If you set Distribution Method to Cross-account, you cannot set Distribution Policy to Nearest Access. |
        |Target Account|The recipient of device distribution. This parameter is required if you set Distribution Method to **Cross-account**. You can specify the target account by using the account name or account ID.

        -   **Target Account Name**: Enter the name of the Alibaba Cloud account.
        -   **Target Account ID**: Enter the ID of the Alibaba Cloud account.
**Note:** To view the ID of the Alibaba Cloud account, log on to the [IoT Platform console](https://iot.console.aliyun.com/), and click the profile picture in the upper-right corner to go to the Security Settings page. |
        |Verification Code|After you specify the recipient account, click **Send Verification Code**. IoT Platform sends a verification code to the mobile phone that is associated with the account. The verification code is valid for 5 minutes. Contact the recipient to obtain the verification code.|
        |Source Region|The region in which the devices reside.|
        |Source Instance|The instance to which the devices belong.|
        |Distribution Policy|The policy that is used to distribute devices. Valid values:        -   **Region**: Distribute devices to the specified region.
        -   **Nearest Access**: Distribute devices to the nearest region based on the IP address of the devices. You can select one or more regions. |
        |Destination Region|This parameter is required if you set Distribution Policy to **Region**.You must select the region to which the devices are distributed. |
        |Destination Instance|This parameter is required if you set Distribution Policy to **Region**.The instance to which the devices are distributed.

        -   If you set Distribution Method to **This Account**, select an instance.
        -   If you set Distribution Method to **Cross-account**, enter an instance ID. If you do not specify this parameter, the public instance is used by default. |
        |Region|This parameter is required if you set Distribution Policy to **Nearest Access**.You can select one or more regions. If you need to distribute the devices to the source region, select the source region.

If multiple instances reside in the destination region, you can select an instance from the drop-down list. |
        |Devices|You can select the devices by using one of the following methods:         -   **Select**: Select a product from the product list and then select one or more devices from the device list. The devices remain selected if you turn the pages of the device list.
        -   **Choose File**: Upload a file that contains a device list. You can upload up to 1,000 entries in a CSV file. You can click **Download Template** to obtain the template. |

    6.  Click **OK**.

    IoT Platform immediately performs the distribution task:

    -   If the Distribution Policy parameter is set to Region, the product data and device data are distributed to the specified region.
    -   If the Distribution Policy parameter is set to Nearest Access, the product data is distributed to the selected regions. When a device is connected to IoT Platform, IoT Platform dynamically distributes data to the nearest region based on the IP address of the device.
    The following table lists the possible states that are displayed on the Batch management tab after you create a distribution task.

    |State|Description|
    |-----|-----------|
    |Initializing|The distribution task is being initialized.|
    |Distributing|The distribution task is being implemented. If this state is displayed for a long time, you can click Retry in the Actions column. |
    |Distributed|The distribution task is completed. The distributed state does not indicate that all devices are distributed. To check whether all devices are distributed, click **Download Record** in the Actions column to view the result. If a specified device does not exist, it fails to be distributed. |
    |Failed|The distribution task has failed due to network jitter. You can click Retry in the Actions column. If the failure persists, contact technical support or submit a ticket.|

    After the distribution task is completed and a device is connected to IoT Platform, the device establishes a connection with the target region and instance by following the process in Step 2.


