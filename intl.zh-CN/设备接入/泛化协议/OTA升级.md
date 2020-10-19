---
keyword: [物联网, 物联网平台, IoT, 通信协议, 泛化协议, 网桥, 桥接]
---

# OTA升级

泛化协议SDK 2.1.3及以上版本支持设备固件升级。下面介绍使用泛化协议SDK调用OTA升级接口，实现设备固件升级。

## 背景信息

**说明：** 仅泛化协议SDK 2.1.3及以上版本支持固件OTA升级。

设备OTA升级流程如下图所示。

![流程](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3049232061/p172100.jpg)

云端推送升级包操作说明，请参见[推送升级包到设备端](/intl.zh-CN/监控运维/固件升级/推送升级包到设备端.md)。

设备OTA升级流程说明，以及使用的Topic和数据格式等信息，请参见[设备端OTA升级](/intl.zh-CN/监控运维/固件升级/设备端OTA升级.md)。

## 调用OTA接口

泛化协议SDK中已封装OTA升级相关接口。您需配置SDK调用以下三个接口实现OTA升级。

-   设备上报固件版本接口

    设备启动时和OTA升级后，上报固件当前版本到Topic：`/ota/device/inform/${YourProductKey}/${YourDeviceName}`。

    泛化协议SDK调用TslUplinkHandler.reportOtaVersion接口，上报版本信息。接口设置如下：

    ```
    /**
     * 设备上报固件版本。
     * @param requestId 请求ID。
     * @param originalIdentity 设备身份原始标识符。
     * @param version 待上报的固件版本。
     * @return 上报成功则返回true。
    */
    boolean reportOtaVersion(String requestId, String originalIdentity, String version)
    ```

    调用示例：

    ```
    TslUplinkHandler tslUplinkHandler = new TslUplinkHandler();
    tslUplinkHandler.doOnline(session, originalIdentity);
    tslUplinkHandler.reportOtaVersion("12345", originalIdentity, "1.0.1");
    ```

-   监听云端下发升级通知接口

    您在物联网平台上添加升级包，并启动批量升级后，物联网平台向设备下发升级通知。设备通过Topic：`/ota/device/upgrade/${YourProductKey}/${YourDeviceName}`获取升级通知。

    泛化协议SDK调用BridgeBootStrap.setOtaUpgradeHandler\(\)接口，并设置一个回调，接收云端推送的OTA升级的信息。接口设置如下：

    ```
    /**
     * 设置回调，接收云端推送的OTA升级的信息。
     * @param otaUpgradeHandler 设置的回调。
    */
    public void setOtaUpgradeHandler(OtaUpgradeHandler otaUpgradeHandler) {
        this.callback.setOtaUpgradeHandler(otaUpgradeHandler);
    }
    
    public interface OtaUpgradeHandler {
    
        /**
         * 云端推送OTA升级消息。
         * @param requestId 云端推送OTA升级消息的ID。
         * @param firmwareInfo OTA升级信息。
         * @param session 当前Session。
         * @return
         */
        boolean onUpgrade(String requestId, OtaFirmwareInfo firmwareInfo, Session session);
    }
    
    public class OtaFirmwareInfo {
    
        /**
         * 升级包大小。
         */
        private long size;
    
        /**
         * 签名方法: MD5、SHA256。
         */
        private String signMethod;
    
        /**
         * 升级包签名。
         */
        private String sign;
    
        /**
         * 升级包版本。
         */
        private String version;
    
        /**
         * 升级包下载URL。
         */
        private String url;
    }
    ```

    调用示例：

    ```
    bridgeBootstrap.setOtaUpgradeHandler(new OtaUpgradeHandler() {
        @Override
        public boolean onUpgrade(String requestId, OtaFirmwareInfo firmwareInfo, Session session) {
            log.info("ota onUpgrade, requestId:{}, firmware:{}, identity:{}",
                requestId, firmwareInfo, session.getOriginalIdentity());
            //处理OTA升级。
            return true;
        }
    });
    ```

-   上报升级进度接口

    设备开始升级，并上报升级进度到Topic：`/ota/device/progress/${YourProductKey}/${YourDeviceName}`。

    泛化协议SDK调用TslUplinkHandler.reportOtaProgress接口，上报升级进度。接口设置如下：

    ```
    /**
     * 上报OTA升级进度。
     * @param requestId 请求消息ID。
     * @param originalIdentity 设备原始身份标识符。
     * @param step 当前进度。
     * @param desc 描述。
     * @return
    */
    boolean reportOtaProgress(String requestId, String originalIdentity, String step, String desc)
    ```

    调用示例：

    ```
    tslUplinkHandler.reportOtaProgress("7979", session.getOriginalIdentity(), "100", "ota success");
    ```


