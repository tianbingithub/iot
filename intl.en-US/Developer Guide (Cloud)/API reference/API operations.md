---
keyword: [Internet of Things, IoT, IoT Platform, cloud service, API]
---

# API operations

The following tables list the API operations that are available for use in IoT Platform.

## API operations for product management

|API|Description|
|:--|:----------|
|[CreateProduct](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/CreateProduct.md)|Creates a product.|
|[UpdateProduct](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/UpdateProduct.md)|Modifies the information of a product.|
|[QueryProductList](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/QueryProductList.md)|Queries all products of an account.|
|[QueryProduct](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/QueryProduct.md)|Queries the details of a product.|
|[DeleteProduct](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/DeleteProduct.md)|Deletes a product.|
|[t86955.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/CreateProductTags.md)|Creates tags for a product.|
|[t87228.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/UpdateProductTags.md)|Modifies the tags of a product.|
|[t87234.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/DeleteProductTags.md)|Deletes the tags of a product.|
|[t87242.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/ListProductTags.md)|Queries all tags of a product.|
|[t87248.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/ListProductByTags.md)|Queries products of an account by tags.|
|[t1849773.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage products/UpdateProductFilterConfig.md)|Modifies the deduplication rule for property messages that are reported by devices of a product.|

## API operations for device management

|API|Description|
|:--|:----------|
|[RegisterDevice](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/RegisterDevice.md)|Registers a device.|
|[QueryDeviceDetail](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceDetail.md)|Queries the details of a device.|
|[t698589.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/BatchQueryDeviceDetail.md)|Queries the details of multiple devices.|
|[QueryDevice](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDevice.md)|Queries all devices of a product.|
|[DeleteDevice](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/DeleteDevice.md)|Deletes a device.|
|[GetDeviceStatus](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/GetDeviceStatus.md)|Retrieves the status of a device.|
|[BatchGetDeviceState](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/BatchGetDeviceState.md)|Retrieves the status of multiple devices.|
|[DisableThing](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/DisableThing.md)|Disables a device.|
|[EnableThing](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/EnableThing.md)|Enables a device.|
|[ResetThing](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/ResetThing.md)|Resets a device.|
|[BatchCheckDeviceNames](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/BatchCheckDeviceNames.md)|Checks whether the names of the devices that you want to register are valid.|
|[BatchRegisterDeviceWithApplyId](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/BatchRegisterDeviceWithApplyId.md)|Registers multiple devices by using an application ID \(ApplyId\).|
|[BatchRegisterDevice](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/BatchRegisterDevice.md)|Registers multiple devices.|
|[QueryBatchRegisterDeviceStatus](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryBatchRegisterDeviceStatus.md)|Queries the status of the application to register multiple devices.|
|[QueryPageByApplyId](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryPageByApplyId.md)|Queries the information of devices that you have registered at a time.|
|[SaveDeviceProp](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/SaveDeviceProp.md)|Creates tags for a device.|
|[QueryDeviceProp](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceProp.md)|Queries the tags of a device.|
|[DeleteDeviceProp](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/DeleteDeviceProp.md)|Deletes the tags of a device.|
|[GetThingTopo](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/GetThingTopo.md)|Queries the topological relationship of a device.|
|[NotifyAddThingTopo](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/NotifyAddThingTopo.md)|Notifies a gateway to build a topological relationship with sub-devices.|
|[t1846893.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/BatchAddTingTopo.md)|Builds a topological relationship with multiple devices.|
|[RemoveThingTopo](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/RemoveThingTopo.md)|Removes the topological relationships of a device.|
|[QueryDeviceStatistics](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceStatistics.md)|Queries the device statistics of a product.|
|[GetGatewayBySubDevice](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/GetGatewayBySubDevice.md)|Queries the information of a gateway based on sub-device information.|
|[QueryDeviceByTags](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceByTags.md)|Queries devices by tag.|
|[t149025.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceFileList.md)|Queries the information of all files that are uploaded to IoT Platform from a device.|
|[t149026.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceFile.md)|Queries the information of a file that is uploaded to IoT Platform from a device.|
|[t149027.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/DeleteDeviceFile.md)|Deletes a file that is uploaded to IoT Platform from a device.|
|[t149631.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/BatchUpdateDeviceNickname.md)|Modifies the aliases of multiple devices.|
|[t1847845.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceByStatus.md)|Queries devices by status.|

## API operations for device group management

|API|Description|
|:--|:----------|
|[t23495.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/CreateDeviceGroup.md)|Creates a device group.|
|[t23498.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/DeleteDeviceGroup.md)|Deletes a device group.|
|[t23508.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/UpdateDeviceGroup.md)|Modifies the information of a device group.|
|[t23500.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/QueryDeviceGroupInfo.md)|Queries the details of a device group.|
|[t23501.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/QueryDeviceGroupList.md)|Queries all device groups.|
|[t23502.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/BatchAddDeviceGroupRelations.md)|Adds devices to a device group.|
|[t23651.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/BatchDeleteDeviceGroupRelations.md)|Removes a device from a device group.|
|[t23504.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/SetDeviceGroupTags.md)|Creates tags for or updates the tags of a device group.|
|[t23503.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/QueryDeviceGroupTagList.md)|Queries the tags of a device group.|
|[QueryDeviceGroupByDevice](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/QueryDeviceGroupByDevice.md)|Queries the information of the device group to which a specified device belongs.|
|[t77504.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/QuerySuperDeviceGroup.md)|Queries the information of a parent group by sub-group ID.|
|[t78503.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/QueryDeviceListByDeviceGroup.md)|Queries all devices in a device group.|
|[t91986.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage device groups/QueryDeviceGroupByTags.md)|Queries the information of device groups by tag.|

## API operations for Thing Specification Language \(TSL\) management

|API|Description|
|---|-----------|
|[t1857516.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/CreateThingModel.md)|Adds TSL features for a product, or adds the extended information of the TSL.|
|[t1860295.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/UpdateThingModel.md)|Modifies a TSL feature of a product, or updates the extended information of the TSL.|
|[t1857514.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/QueryThingModel.md)|Queries the feature details of the TSL of a product.|
|[t1857515.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/CopyThingModel.md)|Copies the TSL of a product to a destination product.|
|[t1857519.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/PublishThingModel.md)|Publishes the TSL of a product.|
|[t1857517.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/DeleteThingModel.md)|Removes a TSL feature from a product.|
|[t1857508.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/ListThingTemplates.md)|Retrieves all product categories that are predefined in IoT Platform.|
|[GetThingTemplate](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/GetThingTemplate.md)|Queries the standard TSL information of a category.|
|[t1857510.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/ListThingModelVersion.md)|Retrieves a list of the TSL versions of a product.|
|[t1857511.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/GetThingModelTsl.md)|Queries the TSL of a product.|
|[t1857513.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Manage TSL models/ImportThingModelTsl.md)|Imports a TSL for a product. Extended TSL information cannot be imported.|
|[t1936242.md\#]()|Queries the feature details of the published TSL of a product.|
|[t1936243.md\#]()|Queries the information of the published TSL of a product.|
|[QueryThingModelExtendConfig]()|Queries the extended information of the TSL of a product.|

## API operations for TSL usage

|API|Description|
|:--|:----------|
|[SetDeviceProperty](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/SetDeviceProperty.md)|Sets properties for a device.|
|[SetDevicesProperty](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/SetDevicesProperty.md)|Sets properties for multiple devices.|
|[InvokeThingService](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/InvokeThingService.md)|Invokes a service on a device.|
|[InvokeThingsService](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/InvokeThingsService.md)|Invokes a service on multiple devices.|
|[QueryDevicePropertyData](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/QueryDevicePropertyData.md)|Queries the property records of a device.|
|[t77565.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/QueryDevicePropertiesData.md)|Queries the statistics of multiple properties of a device.|
|[QueryDeviceEventData](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/QueryDeviceEventData.md)|Queries the event records of a device.|
|[QueryDeviceServiceData](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/QueryDeviceServiceData.md)|Queries the service records of a device.|
|[SetDeviceDesiredProperty](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/SetDeviceDesiredProperty.md)|Sets expected property values for a device.|
|[t124801.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/QueryDeviceDesiredProperty.md)|Queries the expected property values of a device.|
|[t7599.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/QueryDevicePropertyStatus.md)|Queries the property snapshots of a device.|

## API operations for data forwarding

|API|Description|
|:--|:----------|
|[ListRule](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/ListRule.md)|Queries all rules.|
|[CreateRule](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/CreateRule.md)|Creates a rule.|
|[GetRule](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/GetRule.md)|Queries the details of a rule.|
|[UpdateRule](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/UpdateRule.md)|Modifies a rule.|
|[DeleteRule](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/DeleteRule.md)|Deletes a rule.|
|[ListRuleActions](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/ListRuleActions.md)|Queries all actions of a rule.|
|[GetRuleAction](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/GetRuleAction.md)|Queries the details of a rule action.|
|[CreateRuleAction](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/CreateRuleAction.md)|Creates a rule action.|
|[UpdateRuleAction](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/UpdateRuleAction.md)|Modifies a rule action.|
|[DeleteRuleAction](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/DeleteRuleAction.md)|Deletes a rule action.|
|[StartRule](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/StartRule.md)|Enables a rule.|
|[StopRule](/intl.en-US/Developer Guide (Cloud)/API reference/Data forwarding/StopRule.md)|Disables a rule.|

## API operations for topic management

|API|Description|
|:--|:----------|
|[QueryProductTopic](/intl.en-US/Developer Guide (Cloud)/API reference/Manage Topics/QueryProductTopic.md)|Queries information about all topic categories of a product.|
|[CreateProductTopic](/intl.en-US/Developer Guide (Cloud)/API reference/Manage Topics/CreateProductTopic.md)|Creates a topic category for a product.|
|[UpdateProductTopic](/intl.en-US/Developer Guide (Cloud)/API reference/Manage Topics/UpdateProductTopic.md)|Modifies a topic category.|
|[DeleteProductTopic](/intl.en-US/Developer Guide (Cloud)/API reference/Manage Topics/DeleteProductTopic.md)|Deletes a topic category.|
|[CreateTopicRouteTable](/intl.en-US/Developer Guide (Cloud)/API reference/Manage Topics/CreateTopicRouteTable.md)|Creates message routing relationships between topics.|
|[QueryTopicRouteTable](/intl.en-US/Developer Guide (Cloud)/API reference/Manage Topics/QueryTopicRouteTable.md)|Queries the destination topics that subscribe to a source topic.|
|[QueryTopicReverseRouteTable](/intl.en-US/Developer Guide (Cloud)/API reference/Manage Topics/QueryTopicReverseRouteTable.md)|Queries the source topics to which a destination topic subscribes.|
|[DeleteTopicRouteTable](/intl.en-US/Developer Guide (Cloud)/API reference/Manage Topics/DeleteTopicRouteTable.md)|Deletes message routing relationships between topics.|

## API operations for server-side subscription

|API|Description|
|:--|:----------|
|[CreateSubscribeRelation](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/CreateSubscribeRelation.md)|Creates a Message Service \(MNS\) or AMQP server-side subscription.|
|[UpdateSubscribeRelation](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/UpdateSubscribeRelation.md)|Modifies an MNS or AMQP server-side subscription.|
|[QuerySubscribeRelation](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/QuerySubscribeRelation.md)|Queries an MNS or AMQP server-side subscription.|
|[DeleteSubscribeRelation](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/DeleteSubscribeRelation.md)|Deletes an MNS or AMQP server-side subscription.|
|[CreateConsumerGroup](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/CreateConsumerGroup.md)|Creates a consumer group that is required for an AMQP server-side subscription.|
|[UpdateConsumerGroup](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/UpdateConsumerGroup.md)|Modifies the name of a consumer group.|
|[QueryConsumerGroupByGroupId](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/QueryConsumerGroupByGroupId.md)|Queries the details of a consumer group by consumer group ID.|
|[QueryConsumerGroupList](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/QueryConsumerGroupList.md)|Queries all consumer groups of the account, or performs a fuzzy search by consumer group name.|
|[QueryConsumerGroupStatus](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/QueryConsumerGroupStatus.md)|Queries the status of a consumer group when AMQP server-side subscription is enabled. The status information includes the online client information, message consumption rate, number of accumulated messages, and latest message consumption time.|
|[ResetConsumerGroupPosition](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/ResetConsumerGroupPosition.md)|Clears the accumulated messages of a consumer group when AMQP server-side subscription is enabled.|
|[DeleteConsumerGroup](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/DeleteConsumerGroup.md)|Deletes a consumer group.|
|[CreateConsumerGroupSubscribeRelation](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/CreateConsumerGroupSubscribeRelation.md)|Adds a consumer group to an AMQP subscription.|
|[DeleteConsumerGroupSubscribeRelation](/intl.en-US/Developer Guide (Cloud)/API reference/Server-side subscription/DeleteConsumerGroupSubscribeRelation.md)|Removes a consumer group from multiple consumer groups of an AMQP subscription.|

## API operations for communications

|API|Description|
|:--|:----------|
|[Pub](/intl.en-US/Developer Guide (Cloud)/API reference/Communications/Pub.md)|Publishes messages to topics.|
|[RRpc](/intl.en-US/Developer Guide (Cloud)/API reference/Communications/RRpc.md)|Sends a request to a device and receives a response from the device at the same time.|
|[PubBroadcast](/intl.en-US/Developer Guide (Cloud)/API reference/Communications/PubBroadcast.md)|Broadcasts a message.|

## API operations for device shadows

|API|Description|
|:--|:----------|
|[GetDeviceShadow](/intl.en-US/Developer Guide (Cloud)/API reference/Device shadows/GetDeviceShadow.md)|Queries the shadow information of a device.|
|[UpdateDeviceShadow](/intl.en-US/Developer Guide (Cloud)/API reference/Device shadows/UpdateDeviceShadow.md)|Modifies the shadow information of a device.|

## API operations for firmware update

|API|Description|
|---|-----------|
|[t1850163.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/GenerateOTAUploadURL.md)|Generates the information that is used to upload firmware file to Object Storage Service \(OSS\).|
|[CreateOTAFirmware](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/CreateOTAFirmware.md)|Creates a firmware component.|
|[t1850202.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/DeleteOTAFirmware.md)|Deletes a firmware component.|
|[ListOTAFirmware](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/ListOTAFirmware.md)|Queries all firmware components.|
|[QueryOTAFirmware](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/QueryOTAFirmware.md)|Queries the details of a firmware component.|
|[t1850206.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/CreateOTAVerifyJob.md)|Creates a firmware verification task.|
|[CreateOTAStaticUpgradeJob](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/CreateOTAStaticUpgradeJob.md)|Creates a static update task.|
|[t1850208.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/CreateOTADynamicUpgradeJob.md)|Creates a dynamic update task.|
|[t1850210.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/ListOTAJobByFirmware.md)|Queries the update tasks of a firmware component.|
|[ListOTAJobByDevice](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/ListOTAJobByDevice.md)|Queries all firmware update tasks of a device.|
|[QueryOTAJob](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/QueryOTAJob.md)|Queries the details of an update task.|
|[t1850212.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/CancelOTAStrategyByJob.md)|Cancels an update policy that is related to a dynamic update task.|
|[t1850217.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/CancelOTATaskByDevice.md)|Cancels the pending device update tasks of a firmware component.|
|[t1850216.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/CancelOTATaskByJob.md)|Cancels an update task.|
|[t1850213.md\#](/intl.en-US/Developer Guide (Cloud)/API reference/Firmware update/ListOTATaskByJob.md)|Queries an update task.|

