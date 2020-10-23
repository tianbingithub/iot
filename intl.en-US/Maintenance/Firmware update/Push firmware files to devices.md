---
keyword: [Internet of Things, IoT Platform, IoT, device, firmware update, firmware version, OTA, firmware management]
---

# Push firmware files to devices

IoT Platform provides the firmware update and management feature. To update firmware, you must ensure that your devices support over-the-air \(OTA\) updates. Then, you can upload firmware files to the IoT Platform console and push update notifications to the devices. This article describes how to use the IoT Platform console to add, verify, and push firmware files to devices.

Before you use the firmware update feature, ensure that your devices support OTA updates.

-   For information about how to configure OTA updates by using device SDKs, see [Update devices by using OTA](/intl.en-US/Maintenance/Firmware update/Update devices by using OTA.md).
-   If you use the [Link SDK for Android](https://help.aliyun.com/document_detail/96607.html) on devices and need to perform differential updates, you must integrate the algorithms to authenticate the devices and generate full update packages. For more information, see [Implement a differential firmware update on an Android device]().
-   For information about how to configure OTA updates if your devices are installed with AliOS Things chips, see [OTA tutorial for AliOS Things](https://github.com/alibaba/AliOS-Things/wiki/OTA-Tutorial).

Note the following limits of firmware updates:

-   Each Alibaba Cloud account can have a maximum of 500 firmware files.
-   The maximum size of a firmware file is 1,000 MB. The file format can only be .bin, .tar, .gz, .tar.gz, .zip, or .gzip.
-   Note the following limits of update batches:

    Update batches: IoT Platform displays created update tasks as different update batches. You can go to the Firmware Details page and view update batches of the firmware on the Batch Management tab.

    -   You can use a firmware file to create different update batches for multiple firmware versions to be updated.
    -   You can use a firmware file to create only one dynamic update batch for a firmware version to be updated.
    -   You can use different firmware files to create multiple dynamic update batches for a firmware version to be updated.
    -   Each device can have only one ongoing update batch. A device that has an ongoing update batch is in the To Be Pushed, Pushed, or In Upgrade state.
-   Only devices that are connected to IoT Platform by using the MQTT protocol support the firmware update feature.
-   If devices are online, they can immediately receive update notifications. If devices are offline, IoT Platform pushes update notifications when devices go online.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Maintenance** \> **Firmware Update**.

    **Note:** To provide better services, IoT Platform improves the firmware update feature and allows you to manage firmware versions based on products. When you use the new firmware update feature in the console for the first time, you must associate the uploaded firmware files with products. You can associate a firmware file with only one product. For more information about how to associate firmware files with products, see the instructions in the console.

3.  Optional. If your devices are installed with AliOS Things chips, you can enable the Secure Update feature.

    We recommend that you enable this feature to ensure integrity and confidentiality of firmware. If you use the Secure Update feature, you must validate the firmware and firmware signatures of devices. For more information, see [OTA Tutorial for AliOS Things](https://github.com/alibaba/AliOS-Things/wiki/OTA-Tutorial).

    1.  Go to the Firmware Update page, and click **Secure Update**.

    2.  In the dialog box that appears, turn on the **Secure Update** switch for the product that is pending for update.

        When Secure Update is in the **Activated** state, click **Copy** in the Public Key column to copy the public key. You can use the public key to validate the signature of the device.

4.  On the Firmware Update page, click **Add Firmware**.

5.  In the Add Firmware dialog box, specify the parameters, upload a firmware file, and then click **OK**.

    |Parameter|Description|
    |:--------|:----------|
    |Type|    -   Full: If you select Full, you must upload a complete firmware file. The system pushes the complete firmware file to devices for update.
    -   Differential: If you select Differential, you must upload a file that contains only the differences between the destination firmware version and the original firmware version. IoT Platform pushes the differences to devices for update. Then, the differences are merged into the original firmware packages. Differential updates optimize the usage of device resources and minimize the traffic that is consumed during firmware pushing. |
    |Firmware Name|The name of the firmware. The name can contain Chinese characters, English letters, digits, hyphens \(-\), underscores \(\_\), and parentheses \(\). The name must be 1 to 40 characters in length.|
    |Firmware Version|The version number of the firmware. The version number can contain letters, digits, periods \(.\), hyphens \(-\), and underscores \(\_\). It must be 1 to 64 characters in length. You must specify this parameter if you set the Type parameter to **Full**. |
    |Version number to be upgraded|The firmware versions to be updated. The drop-down list shows the firmware versions of all devices under the current product. You can select one or more firmware versions from the drop-down list. You must specify this parameter if you set the Type parameter to **Differential**. |
    |Post-upgrade version number|The destination firmware version. You must specify this parameter if you set the Type parameter to **Differential**. |
    |Affiliated Products|The product to which the specified firmware belongs.|
    |Firmware Module|The module of the firmware. Firmware updates are based on different modules of devices under a product.     -   Select: Select one or more existing firmware modules.
    -   Add: Customize a new module. The module name can contain letters, digits, periods \(.\), hyphens \(-\), and underscores \(\_\). The module name must be 1 to 64 characters in length. Default value: default. |
    |Signature Algorithm|The signature algorithm. Only MD5 and SHA256 are supported. If you use the Link SDK for Android and set the Type parameter to **Differential**, select the MD5 algorithm. |
    |Select Firmware|Uploads a firmware file. The maximum size of a firmware file is 1,000 MB. The file format can only be .bin, .tar, .gz, .tar.gz, .zip, or .gzip.|
    |Description|The description of the firmware features. The description can be up to 100 characters in length. Each Chinese symbol occupies two characters.|

6.  Validate the firmware on one or more devices. In the firmware list, find the required firmware and click **Validate Firmware** in the Actions column. In the dialog box that appears, specify the parameters, and click **OK**.

    **Note:** After you upload a firmware file to IoT Platform, you must validate the firmware on devices. Then, you can use the firmware file to create batch update tasks.

    |Parameter|Description|
    |:--------|:----------|
    |Version number to be upgraded|The drop-down list shows the firmware versions of all devices under the specified product. You can select one or more firmware versions from the drop-down list. After you specify firmware versions, the devices that use these firmware versions are displayed in the **Device to be verified** field. |
    |Device to be verified|Select one or more devices to be validated.|
    |Device upgrade time-out \(minutes\)|The timeout period of the firmware update. If the specified device has not been updated within this period, the update times out. The period starts from the first time the specified device submits the update progress. Valid values: 1 to 1440. Unit: minutes.|

7.  Push update notifications to the devices. After the firmware is validated, click **Batch Update**. In the dialog box that appears, specify the parameters, and click **OK**.

    |Parameter|Description|
    |:--------|:----------|
    |Version number to be upgraded|The drop-down list shows the firmware versions of all devices under the specified product. You can select one or more firmware versions to be updated from the drop-down list. You must specify this parameter if you set the Type parameter to Full. |
    |Update Policy|    -   Static Update: updates only the current devices that meet the conditions.
    -   Dynamic Update: constantly updates the devices that meet the conditions. Dynamic updates can be applied to the following scenarios:

        -   The devices that are subsequently activated meet the conditions.
        -   The current firmware versions that devices submit do not meet the conditions. However, the devices subsequently submit the firmware versions that meet the conditions.
**Note:** You can use a firmware file to create only one dynamic update batch. If you have created a dynamic update batch by using a firmware file, you must cancel this dynamic update batch before creating another one. |
    |Apply Update to|    -   All Devices: updates all devices under the specified product.
    -   Selected Devices: If you select Selected Devices, you must specify a range for the devices to be updated. In the dialog box that appears, select the devices to be updated. Only the selected devices are updated.

**Note:** You can select multiple firmware versions if you select the Selected Devices option. By default, the firmware versions that are specified by the Version number to be upgraded parameter are selected.

    -   Phased Update: updates the specified devices. This option appears if you select Static Update from the Update Policy drop-down list.

After you select Phased Update, specify a range for devices in the Range \(%\) field that appears. IoT Platform calculates the number of devices to be updated based on the specified range. The calculation result is rounded down. You must specify at least one device for a phased update. |
    |Update Time|The time when the device firmware is updated.     -   Update: immediately updates the firmware.
    -   Scheduled Update: updates the firmware at a specified time range. You can specify a start time and end time. The start time must be 5 minutes to 7 days later than the current time. The end time must be 1 hour to 30 days later than the start time. The end time is optional. If you do not specify an end time, the update is not forcibly stopped.

**Note:** Scheduled updates are available if you select Static Update from the Update Policy drop-down list. |
    |Recipients per Minute|The number of devices to which you want to push the download URL of the firmware file per minute. Valid values: 10 to 1000.|
    |Retry Interval|The interval after which a retry is performed if the update fails. Valid values:     -   Do Not Retry
    -   Retry Immediately
    -   Retry in 10 Minutes
    -   Retry in 30 Minutes
    -   Retry in 1 Hour
    -   Retry in 24 Hours |
    |Maximum Retries|The maximum number of retries after the update fails. Valid values:     -   1
    -   2
    -   5 |
    |Timeout \(Minutes\)|The timeout period of the firmware update. If the specified device has not been updated within this period, the update times out. The period starts from the first time the specified device submits the update progress. Valid values: 1 to 1440. Unit: minutes.|
    |Override Previous Device Update Tasks|Specifies whether to overwrite the previous update task. Each device can be in only one ongoing update task at a time. The To Be Pushed, Pushed, or In Upgrade state is displayed if a device is in an ongoing update task.    -   If you select Yes, only the new update task is performed.
    -   If you select No and the device already has an update task, the previous task is implemented.
The update task that is in progress is not overwritten. |
    |Take Effect for only Devices that Newly Report Versions|This parameter is available if you set the Update Policy parameter to **Dynamic Update**.This parameter specifies whether to update only the devices that submit firmware versions later. Ensure that the submit firmware versions are used as the firmware versions to be updated.

    -   If you select Yes, only the devices that subsequently submit firmware versions are updated.
    -   If you select No, the current devices that meet the conditions are updated. In addition, IoT Platform constantly checks the devices. If the devices that subsequently submit firmware versions meet the conditions, these devices are updated. |


After you submit a batch update request, IoT Platform pushes an update notification to your devices based on your settings.

Find the required firmware and click **View** in the Actions column. On the Batch Management tab, you can perform the following operations:

-   View the status of an update batch.
-   Cancel all update tasks of an update batch.

**View the status of an update batch**

On the Batch Management tab, find the required update batch and click **View** in the Actions column. On the Batch Details page, you can view devices in different update statuses on the Device List tab.

|State|Description|
|-----|-----------|
|To Be Pushed|The firmware update notification has not been pushed to the device. This state may appear due to the following three causes: 1. The device is offline. 2. The notification is scheduled to be pushed. 3. The pushing rate exceeds the limit. The following states that correspond to these three causes are displayed:

-   **To Be Pushed \(Device Offline\)**
-   **To Be Pushed \(Timing: 2020/XX/XX XX:XX:XX\)**
-   **To Be Pushed** |
|Pushed|The device has received the firmware update notification but has not submitted the progress.|
|In Upgrade|The device has received the firmware update notification and submitted the update progress.|
|Updated|The device has submitted the update progress 100% and valid version number after the update.|
|Updated Failed|Firmware updates may fail due to the following causes: -   If a device receives a new update notification before the previous update is complete, the new update fails. In this case, you can update the device after the previous update is complete.
-   During the updating process, a firmware file may fail to be downloaded, extracted, or validated. If a failure occurs, you can initiate a new update.

If you configure a batch update and specify automatic update retries, the retries are performed after the update fails. An update may fail due to one of the following causes:

-   The device in the **To Be Pushed** or **Pushed** state submits a version that is not the current version or the destination version.
-   The device in the **In Upgrade** state submits a version that is not the destination version.
-   The device submits one of the following values when using a specified topic to submit the update progress to IoT Platform: -1, -2, -3, and -4.

During automatic retries, the update status of a device in IoT Platform remains unchanged. For example, if a retry occurs on a device that is in the **Pushed** state, the device status is still displayed as **Pushed** in IoT Platform. If a retry occurs on a device that is in the **In Upgrade** state, the device status is still displayed as **In Upgrade** in IoT Platform.

**Note:**

IoT Platform does not perform update retries if an update fails due to one of the following causes:

-   The update times out``.
-   The update is canceled. |
|Canceled|The update for the device is canceled.|

**Cancel all update tasks of an update batch**

On the Batch Management tab, find the required update batch and click **Cancel** in the Actions column.

-   For a static update batch, only scheduled update tasks are canceled by default. You can cancel all ongoing update tasks, including those in the To Be Pushed, Pushed, and In Upgrade states.
-   For a dynamic update batch, the dynamic update policy is canceled by default. You can cancel all ongoing update tasks, including those in the To Be Pushed, Pushed, and In Upgrade states.

