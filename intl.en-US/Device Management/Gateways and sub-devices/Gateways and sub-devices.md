---
keyword: [Internet of Things,, IoT, Gateway, Sub-device, IoT Hub, Topological relationships, Sub-device connection IoT Platform]
---

# Gateways and sub-devices

Devices can directly connect to IoT Platform, or connect to IoT Platform through a gateway.

## Scenarios

If devices cannot directly connect to IoT Platform or if topology management is required, you must use Wi-Fi gateways, Bluetooth gateways, ZigBee gateways, or other gateways to connect the devices to IoT Platform.

## Benefits

IoT Platform allows you to manage the sub-devices of gateways and the topologies between the sub-devices and the gateways. It also monitors and maintains the sub-devices. Your business system can transfer messages to and from sub-devices without the need to consider the topologies.

## Gateways and devices

When you create products and devices, you must select a node type. Currently, IoT platform supports the following three node types:

-   Directly connected devices: Devices that directly connect to IoT Platform, but cannot manage sub-devices.
-   Sub-devices: a sub-device connected to the gateway through the gateway.
-   Gateway devices: Devices that directly connect to IoT platform, and can manage sub-devices, maintain their topological relationships with sub-devices, and synchronize these topological relationships to the cloud.

The following figure shows the topology between a gateway and its sub-devices.

![Gateways and sub-devices](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2257711061/p170568.jpg)

## Connect gateways and sub-devices to IoT Platform

After a gateway is connected to IoT Platform, the gateway synchronizes the information of topologies between the gateway and its sub-devices to IoT Platform. The gateway authenticates the sub-devices. After authentication, the gateway uploads messages from the sub-devices to IoT Platform, transfers commands from IoT Platform to the sub-devices, and supports other communications between the sub-devices and IoT Platform.

-   You can connect the gateway to IoT Platform by using the same method as a common device. For more information, see [Link SDK documentation](https://www.alibabacloud.com/help/product/93051.htm).
-   You can connect sub-devices to IoT Platform by using one of the following methods:
    -   [Unique-certificate-per-device authentication](/intl.en-US/Device Access/Authenticate devices /Unique-certificate-per-device authentication.md). After the gateway retrieves a certificate of the sub-device, it sends the certificate to IoT Platform. The certificate includes the ProductKey, DeviceName, and DeviceSecret.
    -   [Dynamic sub-device registration](/intl.en-US/Device Access/Authenticate devices /Authenticate devices.md). You must enable dynamic sub-device registration in the IoT Platform console. After the gateway retrieves the ProductKey and DeviceName from a sub-device, the gateway registers the sub-device in IoT Platform to verify the identity of the sub-device. After the sub-device passes the authentication, a DeviceSecret is assigned to the device. Then, the sub-device uses its certificate to connect to IoT Platform. The certificate includes the ProductKey, DeviceName, and DeviceSecret.

