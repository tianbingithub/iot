---
keyword: [Internet of Things, IoT Platform, IoT, device log, Log Service, log report, alert, log analysis]
---

# Dump logs

The device log feature of IoT Platform allows you to retain logs of the last seven days by default. This feature also allows you to export IoT Platform operational logs to the Logstores of Log Service for persistent storage. After you enable the log dump feature, you can search and analyze logs in the IoT Platform console. This feature provides multiple functions, such as log reports, dashboard subscription, and alerts.

Log Service is activated. For more information, see [Activate Log Service](/intl.en-US/Data Collection/Preparation/Preparations.md).

## Enable the log dump feature

**Note:** The log dump feature is available only for Alibaba Cloud accounts.

You must enable the log dump feature for products in sequence.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Maintenance** \> **Device Log**.

3.  Select a product and click the **Log Dump** tab.

4.  Click **Enable**.

5.  Read the instructions in the Log Configurations dialog box and click **OK**.

    **Note:** If Log Service is not activated, you are redirected to the activation page of Log Service after you click **Enable**.

    After you enable the log dump feature for a product, IoT Platform creates a log storage location and service linked role that is used to export logs.

    -   Log storage location:

        -   Project: iot-log-$\{uid\}-$\{regionId\}. Replace the $\{uid\} variable with your Alibaba Cloud account ID. Replace the $\{regionId\} variable with the ID of your region where IoT Platform resides.
        -   Logstore: iot-logs.
        All products share the log storage location. You can find the logs of a product based on the specified product key. For more information, see [IoT Platform logs](/intl.en-US/Maintenance/Log Service/IoT Platform logs.md).

    -   Service linked role: to export logs. For more information, see [Roles](/intl.en-US/User Accounts/RAM authorization/Resource Access Management (RAM)/AliyunServiceRoleForIoTLogExport service linked role.md).
6.  Set the retention period of logs. If exported logs reach the end of the specified retention period, these logs are deleted. The default retention period is seven days. The retention period can be 1 to 3,000 days or permanent.

    After you click **Set Log Save Time**, you are redirected to the Logstore Attributes page. You can click **Modify**, set the required parameters, and then click **Save**.


## Use the log search and analysis feature

After you enable the log dump feature, you can log on to the [IoT Platfrom console](http://iot.console.aliyun.com/), choose **Maintenance** \> **Device Log** \> **Log Dump**, and then perform the following operations:

-   Search and analyze logs: On the **Raw Logs** tab, you can use SQL statements to search logs of a specified period of time. Then, you can create statistical charts based on analytical results. For more information about the SQL syntax, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md) and [Analysis syntax](/intl.en-US/Index and query/Real-time log analysis.md). For more information about statistical charts, see [Analytical charts](/intl.en-US/Query and visualization/Analysis graph/Overview.md).
-   Quick analysis: After you search logs on the **Raw Logs** tab, you can view the distribution of a field in a specified period of time based on the search results. For more information, see [Quick analysis](/intl.en-US/Index and query/Query/Quick analysis.md).
-   View reports: On the **Log Report** tab, you can view log reports in a specified period of time. Log reports reflect the statuses and exceptions of devices. The following table describes the reports.

    |Report|Description|
    |------|-----------|
    |Number of Times Device Online & Offline|The line chart shows the number of times that devices connect to IoT Platform, and number of times that devices disconnect from IoT Platform in a specified period of time.|
    |Device Uplink & Downlink Messages|The line chart shows the number of upstream messages and number of downstream messages in a specified period of time.|
    |Top 20 Devices by Uplink&Downlink Message|The list shows the top 20 devices that report or receive the most upstream or downstream messages, and the number of messages of each device.|
    |Error Distribution for TSL Validation|The pie chart shows the distribution of data parsing errors in a specified period of time.You can search logs by error code and view the details of data parsing errors in logs. This allows you to improve a data parsing script in an efficient manner. |
    |Top 10 Devices by Data Parsing Script Error|The list shows the top 10 devices on which the most data parsing errors occur, and the number of errors that occur on each device.You can search logs by device name and view the details of data parsing errors in logs. This allows you to improve a data parsing script in an efficient manner. |
    |Error Distribution for TSL Validation|The pie chart shows the distribution of Thing Specification Language \(TSL\) validation errors in a specified period of time.You can search logs by error code, view the details of TSL validation errors in logs, and troubleshoot issues. |
    |Top 10 Devices by TSL Validation Error|The list shows the top 10 devices on which the most TSL validation errors occur, and the number of errors that occur on each device in a specified period of time.You can search logs by device name, view the details of TSL validation errors in logs, and troubleshoot issues. |
    |Server-side Subscription Messages Forwarded|The line chart shows the total number of messages that are forwarded by an Advanced Message Queuing Protocol \(AMQP\) or Message Service \(MNS\) server-side subscription in a specified period of time.|
    |Last 20 Device Anomaly Messages|The pie chart shows the last 20 device anomaly messages.You can search logs by device name and error code, view the details of device anomaly messages in logs, and troubleshoot issues. |
    |Cloud Service Messages Forwarded|The line chart shows the total number of messages that are forward by the data forwarding feature in a specified period of time.|
    |Last 20 Data Forwarding Errors|The line chart shows the last 20 data forwarding errors of the rules engine in a specified period of time.You can search logs by device name and error code, view the details of data forwarding errors for the rules engine in logs, and troubleshoot issues. |
    |Error Distribution for Cloud API Calls|The pie chart shows the distribution of API invocation errors in a specified period of time.You can search logs by API operation name and troubleshoot issues by using log details and error codes. |

    Note:

    -   Reports show data of the last one hour \(time frame\). The time interval for line charts is one minute.
    -   To set a time range, you can select the required value from the Time drop-down list in the upper-right corner of the Log Dump tab or choose ![Button](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5976101061/p170268.jpg)\>**Select Time Range** in the upper-right corner of a report card.
    -   To set a time interval for a line chart, you must write an SQL statement on the **Raw Logs** tab. For more information about the SQL syntax, see [Date and time functions](/intl.en-US/Index and query/Analysis grammar/Date and time functions.md).

        For example, you can use the `bizCode:device | SELECT date_format(date_trunc('hour',__time__), '%m-%d %H:%i') AS Time, count(1) AS count , operation GROUP BY Time, operation ORDER BY Time limit 1440` query statement to set the time interval to one hour.

-   Subscribe to a dashboard: IoT Platform converts dashboards into images on a regular basis and sends these images to the required customers by using emails or DingTalk chatbots. For more information about how to subscribe to a dashboard, see [Subscribe to a dashboard](/intl.en-US/Query and visualization/Dashboard/Subscribe to a dashboard.md).
-   Set alerts: If one of the alert conditions is met, IoT Platform sends alerts by using multiple notification methods. These methods include the text message, phone call, email, and DingTalk chatbot. For more information about how to set alerts, see [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md).

## Disable the log dump feature

You can disable the log dump feature for a product at any time. This allows you to save storage space. Procedure:

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Maintenance** \> **Device Log**.

3.  Select a product and click the **Log Dump** tab.

4.  Select the destination product, click **Stop Dump**, and click **OK**.

    New logs are no longer exported to the Logstore. Previous logs will not be deleted until these logs reach the end of the retention period.


