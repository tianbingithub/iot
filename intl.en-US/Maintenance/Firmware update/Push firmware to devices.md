---
keyword: [Internet of Things, IoT Platform, IoT, device, firmware update, firmware version, OTA, firmware management]
---

# Push firmware to devices

IoT Platform provides the firmware update and management feature. To update firmware, you must ensure that your devices support over-the-air \(OTA\) updates. Then, you can upload firmware files to the IoT Platform console and push update notifications to devices. This topic describes how to use the IoT Platform console to add, verify, and push firmware files to devices.

Before you use the firmware update feature, ensure that your devices support OTA updates.

-   If you use device SDKs provided by Alibaba Cloud, see [Update devices by using OTA](/intl.en-US/Maintenance/Firmware update/Update devices by using OTA.md).
-   If your devices are installed with AliOS Things chips, visit [OTA tutorial for AliOS Things](https://github.com/alibaba/AliOS-Things/wiki/OTA-Tutorial).

The limits of firmware updates are described as follows:

-   Each Alibaba Cloud account can have a maximum of 500 firmware files.
-   The maximum size of a firmware file is 1,000 MB. The file format can only be .bin, .tar, .gz, .tar.gz, .zip, or .gzip.
-   The limits of update batches are described as follows:

    Update batches: IoT Platform displays created update tasks as different update batches. You can go to the Firmware Details page and view update batches of the firmware on the Batch Management tab.

    -   You can use a firmware file to create multiple update batches at the same time.
    -   One device can have only one ongoing update batch. \(If a device has an ongoing update batch, the device is in the To Be Pushed, Pushed, or In Upgrade state.\)
    -   You can use a firmware file to create only one dynamic update batch for a firmware version to be updated.
    -   You can use different firmware files to create multiple dynamic update batches for a firmware version to be updated. If a device is included in multiple dynamic update policies at the same time, the system only implements the latest update policy.
-   Only devices that are connected to IoT Platform by using the MQTT protocol support the firmware update feature.
-   If devices are online, they can immediately receive update notifications. If devices are offline, the system pushes update notifications when devices go online.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Maintenance** \> **Firmware Update**.

    **Note:** To provide better services, IoT Platform improves the firmware update feature and allows you to manage firmware versions by product. When you use the new firmware update feature in the console for the first time, you must associate the uploaded firmware files with products. You can associate a firmware file with only one product. For more information about how to associate firmware files with products, see the instructions in the console.

3.  Optional. If your devices are installed with AliOS Things chips, you can activate the Secure Update feature.

    This feature is used to ensure integrity and confidentiality of firmware. We recommend that you activate this feature. If you use the Secure Update feature, you must validate the firmware and firmware signatures. For more information, visit [OTA Tutorial for AliOS Things](https://github.com/alibaba/AliOS-Things/wiki/OTA-Tutorial).

    1.  Go to the Firmware Update page, and click **Secure Update**.

    2.  In the dialog box that appears, switch the status of Secure Update to **Activated** for the product that is pending for update.

        When Secure Update is in the **Activated** state, click **Copy** in the Public Key column to copy the public key. You can use the public key to validate the firmware signature.

4.  On the Firmware Update page, click **Add Firmware**.

5.  In the Add Firmware dialog box that appears, enter firmware information, and upload a firmware file.

    |Parameter|Description|
    |:--------|:----------|
    |Type|    -   Full: If you select Full, you must upload a complete firmware file. The system pushes the complete firmware file to devices for update.
    -   Differential: If you select Differential, you must upload a package that contains only the differences between the target firmware version and the current firmware version. The system pushes the differences to devices for update. You must generate a differential update package.

Differential updates optimize the usage of device resources and minimize the traffic that is consumed to firmware files. |
    |Firmware Name|The name of the firmware. The name can contain letters, digits, and underscores \(\_\) and cannot start with an underscore \(\_\). The name must be 4 to 32 characters in length.|
    |Product|The product to which the specified firmware belongs.|
    |Firmware Module|The module of the firmware. Firmware updates are based on different modules of devices under a product.     -   Select: Select one or more existing firmware modules.
    -   Add: Customize a new module. The module name can contain lowercase letters, digits, hyphens \(-\), and underscores \(\_\). The module name must be 1 to 64 characters in length. Default value: default. |
    |Firmware Version|The version for the firmware. The version number can contain lowercase letters, digits, hyphens \(-\), and underscores \(\_\). The version number must be 1 to 64 characters in length. You must specify this parameter if you set the Type parameter to **Full**. |
    |Version number to be upgraded|The firmware versions to be updated. The drop-down list shows the firmware versions of all devices under the current product. You can select one or more firmware versions from the drop-down list. You must specify this parameter if you set the Type parameter to **Differential**. |
    |Post-upgrade version number|The target firmware version. You must set this parameter when you set the Type parameter to **Differential**. |
    |Signature Algorithm|The signature algorithm. Only MD5 and SHA256 are supported.|
    |Select Firmware|Uploads a firmware file. The maximum size of a firmware file is 1,000 MB. The file format can only be .bin, .tar, .gz, .tar.gz, .zip, or .gzip.|
    |Description|The description of the firmware features. The description can be up to 100 characters in length.|

6.  In the firmware list, find the required firmware and click **Validate Firmware** in the Actions column to validate the firmware on one or more devices.

    **Note:** After you upload the firmware file to IoT Platform, you must validate the firmware on certain devices to ensure that these devices can be updated. Then, you can use the firmware file to create batch update tasks.

    |Parameter|Description|
    |:--------|:----------|
    |Version number to be upgraded|The drop-down list shows the firmware versions of all devices under the specified product. You can select one or more firmware versions from the drop-down list. After you specify firmware versions, the devices that use these firmware versions are displayed in the **Device to be verified** field. |
    |Device to be verified|Select one or more devices to be validated.|
    |Device upgrade time-out \(minutes\)|The timeout period of the firmware update. If the specified device has not been updated within this period, the update times out. The period starts from the first time the specified device submits the update progress. Valid values: 1 to 1440. Unit: minutes.|

7.  After the firmware is validated, click **Batch Update** in the Actions column. In the dialog box that appears, set the parameters. The system pushes the update notifications to the specified devices.

    |Parameter|Description|
    |:--------|:----------|
    |Version number to be upgraded|The drop-down list shows the firmware versions of all devices under the specified product. You can select one or more firmware versions to be updated from the drop-down list. You must set this parameter if you set the Type parameter to Full. |
    |Update Policy|    -   Static Update: updates the activated devices that meet the specified conditions.
    -   Dynamic Update: maintains the devices to be updated, including the devices that have submitted the current versions and the newly activated devices.

**Note:** You can use a firmware file to create only one dynamic update batch. If you have created a dynamic update batch for the firmware file, you must cancel this dynamic update batch before creating another one. |
    |Apply Update to|    -   All Devices: updates all devices under the specified product.
    -   Selected Devices: If you select Selected Devices, you must specify a range for the devices to be updated. In the dialog box that appears, select the devices to be updated. The system only updates the specified devices.

**Note:** You can select multiple firmware versions if you select Selected Devices. By default, the values of the Version number to be upgraded parameter are selected.

    -   Phased Update: updates the specified devices. This option appears if you select Static Update from the Update Policy drop-down list.

After you select Phased Update, specify a range for devices in the Range \(%\) field that appears. IoT Platform calculates the number of devices to be updated based on the specified range. The calculation result is rounded down. You must specify at least one device for a phased update. |
    |Update Time|The time when the device firmware is updated.     -   Update: immediately updates the firmware.
    -   Scheduled Update: updates the firmware at specified time. The specified time ranges from 5 minutes to 7 days.

**Note:** This update is applicable if you select Static Update from the Update Policy drop-down list. |
    |Recipients per Minute|The number of devices to which you want to push the download URLs for the firmware files per minute. Valid values: 10 to 1000.|
    |Retry Interval|The interval at which the firmware update is retried if the specified update fails. Valid values:     -   Do Not Retry
    -   Retry Immediately
    -   Retry in 10 Minutes
    -   Retry in 30 Minutes
    -   Retry in 1 hour
    -   Retry in 24 hours |
    |Maximum Retries|The maximum number of retries after the specified update fails. Valid values:     -   1
    -   2
    -   5 |
    |Timeout \(Minutes\)|The timeout period of the firmware update. If the specified device has not been updated within this period, the update times out. The period starts from the first time the specified device submits the update progress. Valid values: 1 to 1440. Unit: minutes.|


After you submit a batch update request, IoT Platform pushes an update notification to your devices based on your settings.

Find the required firmware and click **View** in the Actions column. On the Batch Management tab, you can perform the following operations:

-   View the status of an update batch.
-   Cancel all update tasks of an update batch.

**View the status of an update batch**

On the Batch Management tab, find the required update batch and click **View** in the Actions column. On the Batch Details page, you can view devices in different update states on the Device List tab. The

|State|Description|
|-----|-----------|
|To Be Pushed|The firmware update notification has not been pushed to the device. This state may appear due to the following three causes: 1. The device is offline. 2. The notification is scheduled to be pushed. 3. The pushing rate exceeds the limit. The states that correspond to these three causes are shown as follows:

-   **To Be Pushed \(Device Offline\)**
-   **To Be Pushed \(Timing: 2020/XX/XX XX:XX:XX\)**
-   **To Be Pushed** |
|Pushed|The device has received the firmware update notification but has not submitted the progress.|
|In Upgrade|The device has received the firmware update notification and submitted the update progress.|
|Updated|The device has submitted the update progress 100% and the correct version number after the update.|
|Update Failed|Firmware updates may fail due to the following causes: -   If a device receives a new update notification before the previous update is complete, the new update will fail. In this case, you can update the device after the previous update is complete.
-   During the updating process, a firmware file may fail to be downloaded, extracted, or validated. If a failure occurs, you can initiate a new update.

If you configure a batch update and specify automatic update retries, the system retries updating after an update fails due to one of the following causes:

-   The device in the **To Be Pushed** or **Pushed** state submits a version that is not the current version or the target version.
-   The device in the **In Upgrade** state submits a version that is not the target version.
-   The device submits one of the following values when using a specified topic to submit the update progress to IoT Platform: -1, -2, -3, and -4.

During automatic retries, the update status of a device on the cloud remains unchanged. For example, if a retry occurs on a device that is in the **Pushed** state, the device status is still displayed as **Pushed** on the cloud. If a retry occurs on a device that is in the **In Upgrade** state, the device status is still displayed as **In Upgrade** on the cloud.

**Note:**

IoT Platform does not trigger update retries if an update fails due to one of the following causes:

-   The update fails due to a `timeout`.
-   The update fails because you cancel the update. |

**Cancel all update tasks of an update batch**

On the Batch Management tab, find the required update batch and click **Cancel** in the Actions column.

-   For a static update batch, only scheduled update tasks are canceled by default. You can cancel all ongoing update tasks, including those in the To Be Pushed, Pushed, and In Upgrade states.
-   For a dynamic update batch, the dynamic update policy is canceled by default. You can cancel all ongoing update tasks, including those in the To Be Pushed, Pushed, and In Upgrade states.

