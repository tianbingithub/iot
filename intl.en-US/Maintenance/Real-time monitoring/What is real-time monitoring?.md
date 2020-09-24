---
keyword: [Internet of Things, IoT, IoT Platform, monitoring]
---

# What is real-time monitoring?

IoT Platform provides real-time monitoring on metrics such as the number of online devices, number of upstream and downstream messages, number of messages forwarded by the rules engine, and device network status. In addition, you can monitor the IoT Platform data by setting alert rules in Cloud Monitor.

## Use IoT Platform to monitor resource usage

IoT Platform monitors device data and network status under your Alibaba Cloud account in real time. The monitoring data is displayed on the Real-time Monitoring page.

-   Device data metrics

    The following metrics are displayed in the console: Online Devices, Messages Sent to IoT Platform, Messages Sent from IoT Platform, and Messages Forwarded Through Rule Engine.

    For more information, see [View data metrics](/intl.en-US/Maintenance/Real-time monitoring/View data metrics.md).

-   Device network status

    You can enable the devices whose network connection method is Wi-Fi to submit the network status data. After a device submits data, you can view the network status and network error messages of the device on the Device Network status tab of the Real-time Monitoring page. You can specify a device name and time range for a query.

    The displayed network status data of devices includes the signal collection time, received signal strength \(RRS\), signal-to-noise ratio \(SNR\), and packet loss rate. For more information, see [View device network status](/intl.en-US/Maintenance/Real-time monitoring/View device network status.md).

    For more information about topics that are used to submit network status data, format of network status data, and network errors, see [Devices submit network status data](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device network status.md).

    **Note:** Only devices whose network connection method is Wi-Fi can submit network status data.


## Use Cloud Monitor to monitor resource usage

IoT Platform is integrated with Cloud Monitor. You can use the Event Alarm and Threshold Value Alarm features of Cloud Monitor to monitor the IoT Platform data. The metrics include OnlineDeviceCount, MessageCountSentFromIoT, MessageCountSentToIoT, MessageCountForwardedThroughRuleEngine, MessageCountPerMinute, DeviceEventReportError, DevicePropertyReportError, and DeviceServiceCallError.

On theReal-time Monitoring page, click **Alarm Settings** to go to the CloudMonitor console. Configure threshold-triggered and event-triggered alert rules as required.

For more information about alert rules and alert messages, see [Use CloudMonitor to monitor IoT resources](/intl.en-US/Maintenance/Real-time monitoring/Use CloudMonitor to monitor resource usage/Use CloudMonitor to monitor IoT resources.md) and [Alerts and notifications](/intl.en-US/Maintenance/Real-time monitoring/Use CloudMonitor to monitor resource usage/Alerts and notifications.md).

## Authorize RAM users to use real-time monitoring

To authorize RAM users to use the real-time monitoring feature, you must log on to the [RAM console](https://ram.console.aliyun.com/) and grant the `cms:QueryMetricList` permission to the RAM users. For more information about how to customize a RAM permission policy, see [Customize permissions](/intl.en-US/User Accounts/RAM authorization/Resource Access Management (RAM)/Custom permissions.md).

The following script shows the cms:QueryMetricList permission policy.

```
{
    "Statement": [
        {
            "Action": [
                "cms:QueryMetricList"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ],
    "Version": "1"
}
```

