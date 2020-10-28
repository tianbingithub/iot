---
keyword: [物联网, IoT, 物联网平台, 云端, API]
---

# API列表

以下是物联网平台API列表。

## 产品管理相关API

|API|描述|
|:--|:-|
|[CreateProduct](/cn.zh-CN/云端开发指南/云端API参考/产品管理/CreateProduct.md)|创建产品。|
|[UpdateProduct](/cn.zh-CN/云端开发指南/云端API参考/产品管理/UpdateProduct.md)|修改产品信息。|
|[QueryProductList](/cn.zh-CN/云端开发指南/云端API参考/产品管理/QueryProductList.md)|查询产品列表。|
|[QueryProduct](/cn.zh-CN/云端开发指南/云端API参考/产品管理/QueryProduct.md)|查询产品详细信息。|
|[DeleteProduct](/cn.zh-CN/云端开发指南/云端API参考/产品管理/DeleteProduct.md)|删除指定产品。|
|[CreateProductTags](/cn.zh-CN/云端开发指南/云端API参考/产品管理/CreateProductTags.md)|创建产品标签。|
|[UpdateProductTags](/cn.zh-CN/云端开发指南/云端API参考/产品管理/UpdateProductTags.md)|更新产品标签。|
|[DeleteProductTags](/cn.zh-CN/云端开发指南/云端API参考/产品管理/DeleteProductTags.md)|删除产品标签。|
|[ListProductTags](/cn.zh-CN/云端开发指南/云端API参考/产品管理/ListProductTags.md)|查询产品的所有标签。|
|[ListProductByTags](/cn.zh-CN/云端开发指南/云端API参考/产品管理/ListProductByTags.md)|根据标签查询产品。|
|[UpdateProductFilterConfig](/cn.zh-CN/云端开发指南/云端API参考/产品管理/UpdateProductFilterConfig.md)|更新产品下设备上报的属性去重规则。|
|[QueryProductCertInfo](/cn.zh-CN/云端开发指南/云端API参考/产品管理/QueryProductCertInfo.md)|获取产品的X.509证书信息。|
|[SetProductCertInfo](/cn.zh-CN/云端开发指南/云端API参考/产品管理/SetProductCertInfo.md)|设置产品的X.509证书信息。|

## 设备管理相关API

|API|描述|
|:--|:-|
|[RegisterDevice](/cn.zh-CN/云端开发指南/云端API参考/设备管理/RegisterDevice.md)|注册设备。|
|[QueryDeviceDetail](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceDetail.md)|查询设备详情。|
|[BatchQueryDeviceDetail](/cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchQueryDeviceDetail.md)|批量查询设备详情。|
|[QueryDevice](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevice.md)|查询产品的设备列表。|
|[DeleteDevice](/cn.zh-CN/云端开发指南/云端API参考/设备管理/DeleteDevice.md)|删除设备。|
|[GetDeviceStatus](/cn.zh-CN/云端开发指南/云端API参考/设备管理/GetDeviceStatus.md)|获取设备的运行状态。|
|[BatchGetDeviceState](/cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchGetDeviceState.md)|批量获取设备状态。|
|[DisableThing](/cn.zh-CN/云端开发指南/云端API参考/设备管理/DisableThing.md)|禁用设备。|
|[EnableThing](/cn.zh-CN/云端开发指南/云端API参考/设备管理/EnableThing.md)|解禁设备。|
|[ResetThing](/cn.zh-CN/云端开发指南/云端API参考/设备管理/ResetThing.md)|重置设备。|
|[BatchCheckDeviceNames](/cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchCheckDeviceNames.md)|批量自定义设备名称，物联网平台将检查名称的合法性。|
|[BatchRegisterDeviceWithApplyId](/cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchRegisterDeviceWithApplyId.md)|根据ApplyId批量申请设备。|
|[BatchRegisterDevice](/cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchRegisterDevice.md)|批次申请特定数量设备。|
|[QueryBatchRegisterDeviceStatus](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryBatchRegisterDeviceStatus.md)|查询批量注册设备状态。|
|[QueryPageByApplyId](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryPageByApplyId.md)|查询批次设备列表。|
|[SaveDeviceProp](/cn.zh-CN/云端开发指南/云端API参考/设备管理/SaveDeviceProp.md)|设置设备标签。|
|[QueryDeviceProp](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceProp.md)|查询设备标签列表。|
|[DeleteDeviceProp](/cn.zh-CN/云端开发指南/云端API参考/设备管理/DeleteDeviceProp.md)|删除设备标签。|
|[GetThingTopo](/cn.zh-CN/云端开发指南/云端API参考/设备管理/GetThingTopo.md)|查询网关设备或子设备所具有的拓扑关系。|
|[NotifyAddThingTopo](/cn.zh-CN/云端开发指南/云端API参考/设备管理/NotifyAddThingTopo.md)|通知网关增加设备拓扑关系。|
|[BatchAddThingTopo](/cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchAddThingTopo.md)|批量添加设备拓扑关系。|
|[RemoveThingTopo](/cn.zh-CN/云端开发指南/云端API参考/设备管理/RemoveThingTopo.md)|移除网关设备或子设备所具有的拓扑关系。|
|[QueryDeviceStatistics](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceStatistics.md)|获取设备的统计数量。|
|[GetGatewayBySubDevice](/cn.zh-CN/云端开发指南/云端API参考/设备管理/GetGatewayBySubDevice.md)|根据挂载的子设备信息查询对应的网关设备信息。|
|[QueryDeviceByTags](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceByTags.md)|根据标签查询设备。|
|[QueryDeviceFileList](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceFileList.md)|查询指定设备上传到物联网平台的所有文件。|
|[QueryDeviceFile](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceFile.md)|查询指定设备上传到物联网平台的指定文件信息。|
|[DeleteDeviceFile](/cn.zh-CN/云端开发指南/云端API参考/设备管理/DeleteDeviceFile.md)|删除指定设备上传到物联网平台的指定文件。|
|[BatchUpdateDeviceNickname](/cn.zh-CN/云端开发指南/云端API参考/设备管理/BatchUpdateDeviceNickname.md)|批量更新设备备注名称。|
|[QueryLoRaJoinPermissions](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryLoRaJoinPermissions.md)|查询账号下的LoRaWAN入网凭证列表。|
|[CreateLoRaNodesTask](/cn.zh-CN/云端开发指南/云端API参考/设备管理/CreateLoRaNodesTask.md)|创建批量注册LoRaWAN设备的任务。|
|[GetLoraNodesTask](/cn.zh-CN/云端开发指南/云端API参考/设备管理/GetLoraNodesTask.md)|查询批量注册LoRaWAN设备任务的状态。|
|[QueryDeviceCert](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceCert.md)|查询单个设备的X.509证书。|
|[QueryCertUrlByApplyId](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryCertUrlByApplyId.md)|查询批量注册设备的X.509证书下载链接。|
|[QueryDeviceByStatus](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceByStatus.md)|根据设备状态查询设备列表。|
|[QueryDeviceBySQL](/cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceBySQL.md)|通过SQL语句快速搜索满足指定条件的设备。|

## 分组管理相关API

|API|描述|
|:--|:-|
|[CreateDeviceGroup](/cn.zh-CN/云端开发指南/云端API参考/分组管理/CreateDeviceGroup.md)|创建分组。|
|[DeleteDeviceGroup](/cn.zh-CN/云端开发指南/云端API参考/分组管理/DeleteDeviceGroup.md)|删除分组。|
|[UpdateDeviceGroup](/cn.zh-CN/云端开发指南/云端API参考/分组管理/UpdateDeviceGroup.md)|修改分组信息。|
|[QueryDeviceGroupInfo](/cn.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupInfo.md)|查询分组详情。|
|[QueryDeviceGroupList](/cn.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupList.md)|分页查询分组列表。|
|[BatchAddDeviceGroupRelations](/cn.zh-CN/云端开发指南/云端API参考/分组管理/BatchAddDeviceGroupRelations.md)|添加设备到分组。|
|[BatchDeleteDeviceGroupRelations](/cn.zh-CN/云端开发指南/云端API参考/分组管理/BatchDeleteDeviceGroupRelations.md)|删除分组中已添加的指定设备。|
|[SetDeviceGroupTags](/cn.zh-CN/云端开发指南/云端API参考/分组管理/SetDeviceGroupTags.md)|添加或更新分组标签。|
|[QueryDeviceGroupTagList](/cn.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupTagList.md)|查询分组标签列表。|
|[QueryDeviceGroupByDevice](/cn.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupByDevice.md)|查询指定设备所在的分组列表。|
|[QuerySuperDeviceGroup](/cn.zh-CN/云端开发指南/云端API参考/分组管理/QuerySuperDeviceGroup.md)|根据子分组ID查询父分组信息。|
|[QueryDeviceListByDeviceGroup](/cn.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceListByDeviceGroup.md)|查询分组中的设备列表。|
|[QueryDeviceGroupByTags](/cn.zh-CN/云端开发指南/云端API参考/分组管理/QueryDeviceGroupByTags.md)|根据标签查询设备分组。|

## 物模型管理相关API

|API|描述|
|---|--|
|[CreateThingModel](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/CreateThingModel.md)|为指定产品的物模型新增功能，支持定义物模型扩展描述。|
|[UpdateThingModel](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/UpdateThingModel.md)|更新指定产品物模型中的单个功能，支持更新物模型扩展描述。|
|[QueryThingModel](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/QueryThingModel.md)|查看指定产品的物模型中的功能定义详情。|
|[CopyThingModel](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/CopyThingModel.md)|复制指定产品的物模型到目标产品。|
|[PublishThingModel](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/PublishThingModel.md)|发布指定产品的物模型。|
|[DeleteThingModel](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/DeleteThingModel.md)|删除指定产品物模型中的指定功能。|
|[ListThingTemplates](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/ListThingTemplates.md)|获取物联网平台预定义的产品品类列表。|
|[GetThingTemplate](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/GetThingTemplate.md)|查询指定品类的标准物模型信息。|
|[ListThingModelVersion](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/ListThingModelVersion.md)|获取指定产品的物模型历史版本列表。|
|[GetThingModelTsl](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/GetThingModelTsl.md)|查询指定产品的物模型。|
|[ImportThingModelTsl](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/ImportThingModelTsl.md)|为指定产品导入物模型TSL，暂不支持扩展描述配置。|
|[QueryThingModelPublished](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/QueryThingModelPublished.md)|查看指定产品的已发布物模型中的功能定义详情。|
|[GetThingModelTslPublished](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/GetThingModelTslPublished.md)|查询指定产品的已发布物模型TSL。|
|[QueryThingModelExtendConfig](/cn.zh-CN/云端开发指南/云端API参考/物模型管理/QueryThingModelExtendConfig.md)|导出指定产品的物模型扩展描述配置。|

## 物模型使用相关API

|API|描述|
|:--|:-|
|[SetDeviceProperty](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/SetDeviceProperty.md)|设置设备的属性。|
|[SetDevicesProperty](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/SetDevicesProperty.md)|批量设置设备属性。|
|[InvokeThingService](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/InvokeThingService.md)|调用设备的服务。|
|[InvokeThingsService](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/InvokeThingsService.md)|批量调用设备的服务。|
|[QueryDevicePropertyData](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/QueryDevicePropertyData.md)|查询设备的属性历史数据。|
|[QueryDevicePropertiesData](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/QueryDevicePropertiesData.md)|批量查询指定设备的多个属性的历史数据。|
|[QueryDeviceEventData](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/QueryDeviceEventData.md)|查询设备的事件历史数据。|
|[QueryDeviceServiceData](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/QueryDeviceServiceData.md)|获取设备的服务记录历史数据。|
|[SetDeviceDesiredProperty](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/SetDeviceDesiredProperty.md)|为指定设备批量设置期望属性值。|
|[QueryDeviceDesiredProperty](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/QueryDeviceDesiredProperty.md)|查询指定设备的期望属性值。|
|[QueryDevicePropertyStatus](/cn.zh-CN/云端开发指南/云端API参考/物模型使用/QueryDevicePropertyStatus.md)|查询指定设备的属性快照。|

## 云产品流转相关API

|API|描述|
|:--|:-|
|[ListRule](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/ListRule.md)|查询规则列表。|
|[CreateRule](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/CreateRule.md)|创建规则。|
|[GetRule](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/GetRule.md)|查询规则信息。|
|[UpdateRule](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/UpdateRule.md)|修改规则。|
|[DeleteRule](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/DeleteRule.md)|删除规则。|
|[ListRuleActions](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/ListRuleActions.md)|查询规则动作列表。|
|[GetRuleAction](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/GetRuleAction.md)|查询规则动作信息。|
|[CreateRuleAction](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/CreateRuleAction.md)|创建规则动作。|
|[UpdateRuleAction](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/UpdateRuleAction.md)|更新规则动作。|
|[DeleteRuleAction](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/DeleteRuleAction.md)|删除规则动作。|
|[StartRule](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/StartRule.md)|启动规则。|
|[StopRule](/cn.zh-CN/云端开发指南/云端API参考/云产品流转/StopRule.md)|停止规则。|

## Topic管理相关API

|API|描述|
|:--|:-|
|[QueryProductTopic](/cn.zh-CN/云端开发指南/云端API参考/Topic管理/QueryProductTopic.md)|查询产品Topic类。|
|[CreateProductTopic](/cn.zh-CN/云端开发指南/云端API参考/Topic管理/CreateProductTopic.md)|创建产品Topic类。|
|[UpdateProductTopic](/cn.zh-CN/云端开发指南/云端API参考/Topic管理/UpdateProductTopic.md)|修改产品Topic类。|
|[DeleteProductTopic](/cn.zh-CN/云端开发指南/云端API参考/Topic管理/DeleteProductTopic.md)|删除产品Topic类。|
|[CreateTopicRouteTable](/cn.zh-CN/云端开发指南/云端API参考/Topic管理/CreateTopicRouteTable.md)|添加Topic路由表。|
|[QueryTopicRouteTable](/cn.zh-CN/云端开发指南/云端API参考/Topic管理/QueryTopicRouteTable.md)|查询Topic路由表。|
|[QueryTopicReverseRouteTable](/cn.zh-CN/云端开发指南/云端API参考/Topic管理/QueryTopicReverseRouteTable.md)|查询Topic反向路由表。|
|[DeleteTopicRouteTable](/cn.zh-CN/云端开发指南/云端API参考/Topic管理/DeleteTopicRouteTable.md)|删除Topic路由表。|

## 服务端订阅相关API

|API|描述|
|:--|:-|
|[CreateSubscribeRelation](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/CreateSubscribeRelation.md)|创建MNS或AMQP服务端订阅。|
|[UpdateSubscribeRelation](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/UpdateSubscribeRelation.md)|修改MNS或AMQP服务端订阅。|
|[QuerySubscribeRelation](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/QuerySubscribeRelation.md)|查询MNS或AMQP服务端订阅。|
|[DeleteSubscribeRelation](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/DeleteSubscribeRelation.md)|删除MNS或AMQP服务端订阅。|
|[CreateConsumerGroup](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/CreateConsumerGroup.md)|创建一个消费组，用于创建AMQP服务端订阅。|
|[UpdateConsumerGroup](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/UpdateConsumerGroup.md)|修改消费组名称。|
|[QueryConsumerGroupByGroupId](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/QueryConsumerGroupByGroupId.md)|根据消费组ID查询消费组详情。|
|[QueryConsumerGroupList](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/QueryConsumerGroupList.md)|查询用户所有消费组列表，或按消费组名称进行模糊查询。|
|[QueryConsumerGroupStatus](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/QueryConsumerGroupStatus.md)|使用AMQP服务端订阅时，查询某个消费组的状态，包括在线客户端信息、消息消费速率、消息堆积数、最近消息消费时间。|
|[ResetConsumerGroupPosition](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/ResetConsumerGroupPosition.md)|使用AMQP服务端订阅时，清空某个消费组的堆积消息。|
|[DeleteConsumerGroup](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/DeleteConsumerGroup.md)|删除消费组。|
|[CreateConsumerGroupSubscribeRelation](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/CreateConsumerGroupSubscribeRelation.md)|在AMQP订阅中添加一个消费组。|
|[DeleteConsumerGroupSubscribeRelation](/cn.zh-CN/云端开发指南/云端API参考/服务端订阅/DeleteConsumerGroupSubscribeRelation.md)|从AMQP订阅中的多个消费组移除指定消费组。|

## 消息通信相关API

|API|描述|
|:--|:-|
|[Pub](/cn.zh-CN/云端开发指南/云端API参考/消息通信/Pub.md)|发布消息到Topic。|
|[RRpc](/cn.zh-CN/云端开发指南/云端API参考/消息通信/RRpc.md)|发消息给设备，并同步返回响应。|
|[PubBroadcast](/cn.zh-CN/云端开发指南/云端API参考/消息通信/PubBroadcast.md)|发布广播消息。|

## 设备影子相关 API

|API|描述|
|:--|:-|
|[GetDeviceShadow](/cn.zh-CN/云端开发指南/云端API参考/设备影子/GetDeviceShadow.md)|查询设备影子。|
|[UpdateDeviceShadow](/cn.zh-CN/云端开发指南/云端API参考/设备影子/UpdateDeviceShadow.md)|更新设备影子。|

## OTA升级相关API

|API|描述|
|---|--|
|[GenerateOTAUploadURL](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/GenerateOTAUploadURL.md)|生成升级包文件上传到OSS的URL及详细信息。|
|[GenerateDeviceNameListURL](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/GenerateDeviceNameListURL.md)|生成设备列表文件上传到OSS的URL及详细信息。在创建静态升级批次时，设备列表文件可用于指定要升级的设备。|
|[CreateOTAFirmware](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/CreateOTAFirmware.md)|添加升级包。|
|[DeleteOTAFirmware](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/DeleteOTAFirmware.md)|删除指定升级包。|
|[ListOTAFirmware](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/ListOTAFirmware.md)|查询升级包列表。|
|[QueryOTAFirmware](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/QueryOTAFirmware.md)|查询指定升级包的详细信息。|
|[CreateOTAVerifyJob](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/CreateOTAVerifyJob.md)|创建升级包验证批次。|
|[CreateOTAStaticUpgradeJob](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/CreateOTAStaticUpgradeJob.md)|创建静态升级批次。|
|[CreateOTADynamicUpgradeJob](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/CreateOTADynamicUpgradeJob.md)|创建动态升级批次。|
|[ListOTAJobByFirmware](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/ListOTAJobByFirmware.md)|获取升级包下的升级批次列表。|
|[ListOTAJobByDevice](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/ListOTAJobByDevice.md)|获取设备所在的升级包升级批次列表。|
|[ListOTATaskByJob](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/ListOTATaskByJob.md)|查询指定升级批次下的设备升级作业列表。|
|[QueryOTAJob](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/QueryOTAJob.md)|查询指定升级批次的详情。|
|[CancelOTAStrategyByJob](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/CancelOTAStrategyByJob.md)|取消动态升级批次所关联的动态升级策略。|
|[CancelOTATaskByDevice](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/CancelOTATaskByDevice.md)|取消指定升级包下状态为待升级的设备升级作业。|
|[CancelOTATaskByJob](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/CancelOTATaskByJob.md)|取消指定批次下的设备升级作业。|
|[CreateOTAModule](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/CreateOTAModule.md)|创建产品的OTA模块。|
|[UpdateOTAModule](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/UpdateOTAModule.md)|修改OTA模块别名、描述。|
|[DeleteOTAModule](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/DeleteOTAModule.md)|删除自定义OTA模块。|
|[ListOTAModuleByProduct](/cn.zh-CN/云端开发指南/云端API参考/OTA升级/ListOTAModuleByProduct.md)|查询产品下的OTA模块列表。|

物联网数据分析服务的云端API调用说明，请访问[数据开发API](https://help.aliyun.com/document_detail/135188.html)。

物联网边缘计算的云端API调用说明，请访问[边缘计算API](https://help.aliyun.com/document_detail/135202.html)。

