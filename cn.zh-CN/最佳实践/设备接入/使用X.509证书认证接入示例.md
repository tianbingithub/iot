---
keyword: [IoT, 物联网, CA证书, X.509证书]
---

# 使用X.509证书认证接入示例

本文将提供设备通过MQTT协议，使用X.509证书进行认证接入物联网平台的示例。

目前仅华东2（上海）地域支持X.509证书认证。

本文示例使用物联网平台颁发的X.509证书。

示例代码使用Java语言编写。

## 创建产品和设备

1.  登录[物联网平台控制台](https://iot.console.aliyun.com/)。

2.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

    ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

3.  参见[创建产品](/cn.zh-CN/设备接入/创建产品.md)，创建**认证方式**为**X.509证书**的产品。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3231649951/p75856.png)

4.  产品创建成功后，单击**添加设备**下的**前往添加**，创建设备。

    设备创建成功后，可在**设备详情**页，查看该设备的X.509证书。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3231649951/p75859.png)

5.  单击**X.509**对应的**下载**，下载证书。

    在开发设备端时，需导入证书文件。


## pom.xml 配置

在pom.xml文件中，添加以下依赖。

```
<dependency>
  <groupId>org.eclipse.paho</groupId>
  <artifactId>org.eclipse.paho.client.mqttv3</artifactId>
  <version>1.2.1</version>
</dependency>

<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>fastjson</artifactId>
  <version>1.2.61</version>
</dependency>

<dependency>
  <groupId>commons-codec</groupId>
  <artifactId>commons-codec</artifactId>
  <version>1.13</version>
</dependency>
```

## 导入证书

在Java maven工程的resource目录下，导入证书文件。

-   将从设备详情下载的X.509证书包解压缩，获取证书文件\*.cer和\*.key，并将这两个文件放置到resource目录下。

    **说明：** 物联网平台提供的证书私钥是PKCS\#1格式，而Java原生代码只能使用PKCS\#8格式。可以使用OpenSSL来进行转换，命令如下：

    ```
    openssl pkcs8 -topk8 -in devicex509.key -nocrypt -out devicex509_pkcs8.key
    ```

-   使用TLS方式（securemode=2）将设备接入物联网平台，需使用物联网平台根证书。

    请下载[根证书](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/cert_pub/root.crt?spm=a2c4g.11186623.2.11.527a3f82qk4WXO&file=root.crt)，然后将根证书放置到resource目录下。


## 示例代码

```
/*   
 * Copyright © 2019 Alibaba. All rights reserved.
 */
package com.aliyun.iot.demo;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.security.KeyFactory;
import java.security.KeyStore;
import java.security.PrivateKey;
import java.security.cert.Certificate;
import java.security.cert.CertificateFactory;
import java.security.spec.PKCS8EncodedKeySpec;

import javax.net.ssl.KeyManagerFactory;
import javax.net.ssl.SSLContext;
import javax.net.ssl.SSLSocketFactory;
import javax.net.ssl.TrustManagerFactory;

import org.apache.commons.codec.binary.Base64;
import org.eclipse.paho.client.mqttv3.IMqttDeliveryToken;
import org.eclipse.paho.client.mqttv3.MqttCallback;
import org.eclipse.paho.client.mqttv3.MqttClient;
import org.eclipse.paho.client.mqttv3.MqttConnectOptions;
import org.eclipse.paho.client.mqttv3.MqttException;
import org.eclipse.paho.client.mqttv3.MqttMessage;
import org.eclipse.paho.client.mqttv3.persist.MemoryPersistence;

import com.alibaba.fastjson.JSONObject;

/**
 * MQTT客户端直连阿里云物联网平台，基于Eclipse Paho开发。
 * 基于X.509认证接入文档：https://help.aliyun.com/document_detail/140588.html
 */
public class IotMqttClientWithAuthByX509 {

    // 地域ID
    private static String regionId = "cn-shanghai"; // 目前仅华东2（上海）支持X.509认证。

    // 设备证书
    private String certPath = "";
    // 设备证书私钥
    private String privateKeyPath = "";
    // 密码固定为空
    private String privateKeyPassword = "";
    // X.509认证返回信息的Topic。无需创建，无需订阅，直接使用。
    private static final String AUTH_TOPIC = "/ext/auth/identity/response";
    // 设备productKey，用于接收物联网平台下发的productKey，无需填写。
    private static String productKey = "";
    // 设备deviceName，用于接收物联网平台下发的deviceName，无需填写。
    private static String deviceName = "";

    // MQTT客户端
    private MqttClient sampleClient = null;

    /**
     * 建立MQTT连接
     * 
     * @param certPath 证书路径
     * @param privateKeyPath 私钥路径
     * @param privateKeyPassword 私钥密码，目前固定为空
     */
    public void connect(String certPath, String privateKeyPath, String privateKeyPassword) {

        this.certPath = certPath;
        this.privateKeyPath = privateKeyPath;
        this.privateKeyPassword = privateKeyPassword;

        // 接入域名
        String broker = "ssl://x509.itls." + regionId + ".aliyuncs.com:1883";

        // 表示客户端ID，建议使用设备的MAC地址或SN码，64字符内。
        String clientId = ".";

        // 只支持securemode=2，表示使用TLS。
        String clientOpts = "|securemode=2|";

        // MQTT接入客户端ID
        String mqttClientId = clientId + clientOpts;

        // 建立MQTT连接。使用X.509证书认证，所以不需要username和password。
        connect(broker, mqttClientId, "", "");
    }

    /**
     * 建立MQTT连接
     * 
     * @param serverURL 连接服务器地址
     * @param clientId MQTT接入客户端ID
     * @param username MQTT接入用户名
     * @param password MQTT接入密码
     */
    protected void connect(String serverURL, String clientId, String username, String password) {
        try {
            MemoryPersistence persistence = new MemoryPersistence();
            sampleClient = new MqttClient(serverURL, clientId, persistence);
            MqttConnectOptions connOpts = new MqttConnectOptions();
            connOpts.setMqttVersion(4);// MQTT 3.1.1
            connOpts.setUserName(username);// 用户名
            connOpts.setPassword(password.toCharArray());// 密码
            connOpts.setSocketFactory(createSSLSocket()); // 使用TLS，需要下载根证书root.crt，设置securemode=2。
            connOpts.setCleanSession(false); // 不清理离线消息。qos=1的消息，在设备离线期间会保存在云端。
            connOpts.setAutomaticReconnect(false); // 本demo关闭自动重连。强烈建议生产环境开启自动重连。
            connOpts.setKeepAliveInterval(300); // 设置心跳，建议300秒。
            // 先设置回调。如果是先connect，后设置回调，可能会导致消息到达时回调还没准备好，这样消息可能会丢失。
            sampleClient.setCallback(new MqttCallback() {

                @Override
                public void messageArrived(String topic, MqttMessage message) throws Exception {
                    // 只处理X.509认证返回信息
                    if (AUTH_TOPIC.equals(topic)) {
                        JSONObject json = JSONObject
                                .parseObject(new String(message.getPayload(), StandardCharsets.UTF_8));
                        productKey = json.getString("productKey");
                        deviceName = json.getString("deviceName");
                    } else {
                        // 处理其他下行消息，强烈建议另起线程处理，以免回调堵塞。
                    }
                }

                @Override
                public void deliveryComplete(IMqttDeliveryToken token) {
                }

                @Override
                public void connectionLost(Throwable cause) {
                }
            });
            System.out.println("Connecting to broker: " + serverURL);
            sampleClient.connect(connOpts);
            System.out.print("Connected: clientId=" + clientId);
            System.out.println(",username=" + username + ",password=" + password);
        } catch (MqttException e) {
            System.out.print("connect failed: clientId=" + clientId);
            System.out.println(",username=" + username + ",password=" + password);
            System.out.println("reason " + e.getReasonCode());
            System.out.println("msg " + e.getMessage());
            System.out.println("loc " + e.getLocalizedMessage());
            System.out.println("cause " + e.getCause());
            System.out.println("excep " + e);
            e.printStackTrace();
        } catch (Exception e) {
            System.out.print("connect exception: clientId=" + clientId);
            System.out.println(",username=" + username + ",password=" + password);
            System.out.println("msg " + e.getMessage());
            e.printStackTrace();
        }
    }

    /**
     * 发布消息，默认qos=0
     * 
     * @param topic 发布消息的Topic
     * @param payload 发布的消息内容
     */
    public void publish(String topic, String payload) {
        byte[] content = payload.getBytes(StandardCharsets.UTF_8);
        publish(topic, 0, content);
    }

    /**
     * 发布消息
     * 
     * @param topic 发布消息的Topic
     * @param qos 消息等级，平台支持qos=0和qos=1，不支持qos=2。
     * @param payload 发布的消息内容
     */
    public void publish(String topic, int qos, byte[] payload) {
        MqttMessage message = new MqttMessage(payload);
        message.setQos(qos);
        try {
            sampleClient.publish(topic, message);
            System.out.println("Message published: topic=" + topic + ",qos=" + qos);
        } catch (MqttException e) {
            System.out.println("publish failed: topic=" + topic + ",qos=" + qos);
            System.out.println("reason " + e.getReasonCode());
            System.out.println("msg " + e.getMessage());
            System.out.println("loc " + e.getLocalizedMessage());
            System.out.println("cause " + e.getCause());
            System.out.println("excep " + e);
            e.printStackTrace();
        }
    }

    protected SSLSocketFactory createSSLSocket() throws Exception {

        // 物联网平台根证书，可以从官网文档中下载https://help.aliyun.com/document_detail/73742.html
        // 设备X.509证书，可以从控制台设备信息中下载。

        // CA certificate is used to authenticate server
        InputStream in = IotMqttClientWithAuthByX509.class.getResourceAsStream("/root.crt");
        CertificateFactory cf = CertificateFactory.getInstance("X.509");
        Certificate ca = cf.generateCertificate(in);
        in.close();
        KeyStore keyStore = KeyStore.getInstance(KeyStore.getDefaultType());
        keyStore.load(null, null);
        keyStore.setCertificateEntry("ca", ca);
        TrustManagerFactory tmf = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
        tmf.init(keyStore);

        // client key and certificates are sent to server so it can authenticate us
        InputStream certIn = IotMqttClientWithAuthByX509.class.getResourceAsStream(certPath);
        CertificateFactory certCf = CertificateFactory.getInstance("X.509");
        Certificate certCa = certCf.generateCertificate(certIn);
        certIn.close();
        KeyStore ks = KeyStore.getInstance(KeyStore.getDefaultType());
        ks.load(null, null);
        ks.setCertificateEntry("certificate", certCa);
        PrivateKey privateKey = getPrivateKey(privateKeyPath);
        ks.setKeyEntry("private-key", privateKey, privateKeyPassword.toCharArray(), new Certificate[] { certCa });
        KeyManagerFactory kmf = KeyManagerFactory.getInstance(KeyManagerFactory.getDefaultAlgorithm());
        kmf.init(ks, privateKeyPassword.toCharArray());

        SSLContext context = SSLContext.getInstance("TLSV1.2");
        context.init(kmf.getKeyManagers(), tmf.getTrustManagers(), null);
        SSLSocketFactory socketFactory = context.getSocketFactory();
        return socketFactory;
    }

    private PrivateKey getPrivateKey(String path) throws Exception {
        byte[] buffer = Base64.decodeBase64(getPem(path));
        PKCS8EncodedKeySpec keySpec = new PKCS8EncodedKeySpec(buffer);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        return keyFactory.generatePrivate(keySpec);
    }

    private String getPem(String path) throws Exception {
        InputStream in = IotMqttClientWithAuthByX509.class.getResourceAsStream(path);
        BufferedReader br = new BufferedReader(new InputStreamReader(in));
        String readLine = null;
        StringBuilder sb = new StringBuilder();
        while ((readLine = br.readLine()) != null) {
            if (readLine.charAt(0) == '-') {
                continue;
            } else {
                sb.append(readLine);
                sb.append('\r');
            }
        }
        in.close();
        return sb.toString();
    }

    /**  
     * 连接成功后，物联网平台会下发productKey和deviceName信息到/ext/auth/identity/response，用于组装Topic进行消息收发。 
     * 
     * @param args
     */
    public static void main(String[] args) {

        IotMqttClientWithAuthByX509 client = new IotMqttClientWithAuthByX509();

        // 填写设备证书路径信息
        client.connect("您的设备证书路径", "您的证书私钥路径", "");

        // 连接成功之后，休眠两秒，为保证接收云端下发的productKey和deviceName，不然消息收发Topic的productKey和deviceName字段可能为空。
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 发送消息，验证连接是否成功。
        String updateTopic = "/" + productKey + "/" + deviceName + "/user/update";
        client.publish(updateTopic, "hello mqtt with X.509 auth");
    }
}
```

