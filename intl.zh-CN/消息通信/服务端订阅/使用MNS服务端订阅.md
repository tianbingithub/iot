---
keyword: [物联网, 物联网平台, IoT, 服务器, 监听, 设备消息, 服务端订阅, 设备上报消息, 设备状态变化, 网关发现子设备, 设备生命周期变更, 设备拓扑关系变更, 消息服务（MNS）]
---

# 使用MNS服务端订阅

物联网平台服务端订阅支持将设备消息发送至消息服务（MNS），云端应用通过监听MNS队列，获取设备消息。下面讲解使用MNS订阅设备消息的配置方法。

如果使用子账号，子账号需拥有`AliyunIOTAccessingMNSRole`角色权限。

1.  在物联网平台控制台上，为产品配置服务端订阅，实现物联网平台将消息自动转发至MNS。

    1.  登录[物联网平台控制台](http://iot.console.aliyun.com/)。

    2.  在左侧导航栏，选择**规则引擎** \> **服务端订阅**。

    3.  在服务端订阅页的订阅列表页签下，单击**创建订阅**。

    4.  在创建订阅对话框中，完成配置，单击**确认**。

        |参数|说明|
        |--|--|
        |产品|选择订阅消息源设备所属的产品。|
        |订阅类型|选择为**MNS**。|
        |推送消息类型|服务端要订阅的消息类型。目前，服务端可订阅的设备消息类型包括：         -   **设备上报消息**：产品下所有设备Topic列表中，**操作权限**为**发布**的Topic中的消息。

设备上报消息，包括设备上报的自定义数据和物模型数据（属性上报、事件上报、属性设置响应和服务调用响应）。推送到服务端的物模型数据是经物联网平台系统处理过后的数据，数据格式请参见[数据格式](/intl.zh-CN/消息通信/数据格式.md)。

例如，一个产品有3个Topic类，分别是：

            -   `/${YourProductKey}/${YourDeviceName}/user/get`，具有订阅权限。
            -   `/${YourProductKey}/${YourDeviceName}/user/update`，具有发布权限。
            -   `/${YourProductKey}/${YourDeviceName}/thing/event/property/post`，具有发布权限。
那么，服务端订阅会推送具有发布权限的Topic类中的消息，即`/${YourProductKey}/${YourDeviceName}/user/update`和`/${YourProductKey}/${YourDeviceName}/thing/event/property/post`中的消息。

        -   **设备状态变化通知**：该产品下的设备上下线状态变化时通知的消息。
        -   **网关子设备发现上报**：网关将发现的子设备信息上报给物联网平台。需要网关上的应用程序支持。网关产品特有消息类型。
        -   **设备拓扑关系变更**：子设备和网关之间的拓扑关系建立和解除消息。网关产品特有消息类型。
        -   **设备生命周期变更**：设备创建、删除、禁用、启用等消息。
        -   **物模型历史数据上报**：设备上报的属性和事件历史数据。
        -   **OTA升级状态通知**：验证升级包和批量升级时，设备升级成功或失败的事件通知。 |

    5.  在弹出的确认对话框中，单击**确认**。

        物联网平台将自动创建MNS消息队列，名称格式为`aliyun-iot-${yourProductKey}`。您在配置监听MNS队列时，需配置为该队列。

        MNS会收取费用，具体计费方式，请参见[MNS计费](https://www.alibabacloud.com/help/zh/doc-detail/71896.htm)。

        **说明：** 如果删除已创建的MNS服务端订阅，对应的MNS队列也会自动删除。

2.  配置MNS客户端，监听MNS队列，以接收设备消息。

    以下示例中，使用MNS Java SDK监听消息。

    下载MNS SDK Demo，请访问[MNS文档](https://www.alibabacloud.com/help/zh/doc-detail/27508.htm)。

    1.  在pom.xml文件中，添加如下依赖安装MNS Java SDK。

        ```
        <dependency>
            <groupId>com.aliyun.mns</groupId>
            <artifactId>aliyun-sdk-mns</artifactId>
            <version>1.1.8</version>
            <classifier>jar-with-dependencies</classifier>
        </dependency>
        ```

    2.  配置接收消息时，需填入以下信息。

        ```
        CloudAccount account = new CloudAccount( $AccessKeyId, $AccessKeySecret, $AccountEndpoint);
        ```

        -   $AccessKeyId和$AccessKeySecret需替换为您的阿里云账号访问API的基本信息。在[物联网平台控制台](http://iot.console.aliyun.com/)，鼠标移动到您的账号头像上，然后单击**AccessKey管理**，创建或查看AccessKey。
        -   $AccountEndpoint需填写实际的Endpoint值。在[MNS控制台](https://mns.console.aliyun.com/)，单击**获取Endpoint**获取。
    3.  填写接收设备消息的逻辑。

        ```
        MNSClient client = account.getMNSClient(); 
        CloudQueue queue = client.getQueueRef("aliyun-iot-a1xxxxxx8o9"); //请输入物联网平台自动创建的队列名称。
        
            while (true) { 
            // 获取消息。 
            Message popMsg = queue.popMessage(10); //长轮询等待时间为10秒。      
            if (popMsg != null) { 
                System.out.println("PopMessage Body: "+ popMsg.getMessageBodyAsRawString()); //获取原始消息。 
                queue.deleteMessage(popMsg.getReceiptHandle()); //从队列中删除消息。 
            } else { 
                System.out.println("Continuing"); } }
                                    
        ```

    4.  运行程序，完成对MNS队列的监听。
3.  启动设备，上报消息。

    设备端SDK开发，请参见[Link SDK文档](https://www.alibabacloud.com/help/doc-detail/96624.htm)。

4.  检查云端应用是否监听到设备消息。若成功监听，将获得如下所示消息代码。

    ```
    {
    "messageid":" ",
    "messagetype":"upload",
    "topic":"/al12345****/device123/user/update",
    "payload":" ", 
    "timestamp": " "
    }
    ```

    |参数|说明|
    |:-|:-|
    |messageid|物联网平台生成的消息ID。|
    |messagetype|消息类型。 取值：    -   upload：设备上报消息
    -   status：设备状态变化通知
    -   topo\_listfound：网关子设备发现上报
    -   topo\_lifecycle：设备拓扑关系变更
    -   device\_lifecycle：设备生命周期变更
    -   thing\_history：物模型历史数据上报
    -   ota\_event：OTA升级状态通知 |
    |topic|服务端监听到的信息来源的物联网平台Topic。|
    |payload|Base64编码的消息数据。 payload数据格式，请参见[数据格式](/intl.zh-CN/消息通信/数据格式.md)。 |
    |timestamp|时间戳，以Epoch时间表示。|


**相关文档**  


[数据格式](/intl.zh-CN/消息通信/数据格式.md)

