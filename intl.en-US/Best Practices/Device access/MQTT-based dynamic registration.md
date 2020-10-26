---
keyword: [Internet of Things, IoT, MQTT, dynamic registration]
---

# MQTT-based dynamic registration

This article describes how to perform MQTT-based dynamic registration for directly connected devices.

IoT Platform supports multiple authentication methods for devices. For more information, see [Authenticate a device](/intl.en-US/Device Access/Authenticate devices /Authenticate devices.md).

You can establish MQTT connections to perform [pre-registration unique-certificate-per-product authentication](/intl.en-US/Device Access/Authenticate devices /Unique-certificate-per-product authentication.md) or [preregistration-free unique-certificate-per-product authentication](/intl.en-US/Device Access/Authenticate devices /Unique-certificate-per-product authentication.md). If you use pre-registration unique-certificate-per-product authentication, devices are dynamically registered to obtain DeviceSecrets. The DeviceSecrets are required when you connect the devices with IoT Platform. For more information about the procedure and parameters of MQTT-based dynamic registration, see [MQTT-based dynamic registration]().

This topic provides the Java sample code.

After dynamic registration is successful, a device uses an MQTT client to connect with IoT Platform. For more information about sample code, see [Using Paho MQTT Java client](/intl.en-US/Best Practices/Device access/Access IoT Platform by using Paho/Using Paho MQTT Java client.md).

## Prerequisites

The following steps that are specified in the [Unique-certificate-per-product authentication](/intl.en-US/Device Access/Authenticate devices /Unique-certificate-per-product authentication.md) topic are performed:

1.  Create a product.
2.  Enable dynamic registration.
3.  Add a device.
4.  Burn the device SDK on the production line.

## Configure the pom.xml file

Add the following dependencies to the pom.xml to import SDKs:

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
```

## Sample code

**Note:**

-   If a device is not activated, you can repeat dynamic registration for the device. Each time you obtain a different DeviceSecret. Ensure that the device is dynamically registered once and the DeviceSecret that you obtain is persisted on the device.
-   If an activated device needs to be dynamically registered again, you must reset the device in IoT Platform. You can call the [ResetThing](/intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/ResetThing.md) operation to reset the device.

```
/*   
 * Copyright © 2019 Alibaba. All rights reserved.
 */
package com.aliyun.iot.demo;

import java.nio.charset.StandardCharsets;
import java.util.Random;
import java.util.Set;
import java.util.SortedMap;
import java.util.TreeMap;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;

import org.eclipse.paho.client.mqttv3.IMqttDeliveryToken;
import org.eclipse.paho.client.mqttv3.MqttCallback;
import org.eclipse.paho.client.mqttv3.MqttClient;
import org.eclipse.paho.client.mqttv3.MqttConnectOptions;
import org.eclipse.paho.client.mqttv3.MqttException;
import org.eclipse.paho.client.mqttv3.MqttMessage;
import org.eclipse.paho.client.mqttv3.persist.MemoryPersistence;

import com.alibaba.fastjson.JSONObject;

/**
 * Perform dynamic registration for a device. 
 */
public class DynamicRegisterByMqtt {

    // The ID of the region where your product resides.
    private static String regionId = "cn-shanghai";

    // Specify an encryption algorithm. Valid values: hmacmd5, hmacsha1, and hmacsha256. The value that you specify must be the same as the value of the signmethod parameter.
    private static final String HMAC_ALGORITHM = "hmacsha1";

    // The topic that receives the device certificate from IoT Platform. Use the topic without any changes. You do not need to create or subscribe to the topic.
    private static final String REGISTER_TOPIC = "/ext/register";

    /**
     * Dynamic registration.
     * 
     * @param productKey: the ProductKey of the device.
     * @param productSecret: the ProductSecret of the device.
     * @param deviceName: the DeviceName of the device.
     * @throws Exception
     */
    public void register(String productKey, String productSecret, String deviceName) throws Exception {

        // The endpoint of the authentication service. You must use the Transport Layer Security (TLS) protocol.
        String broker = "ssl://" + productKey + ".iot-as-mqtt." + regionId + ".aliyuncs.com:1883";

        // The ID of the client. We recommend that you use the MAC address or SN of the device. The client ID can be a maximum of 64 characters in length.
        String clientId = productKey + "." + deviceName;

        // Obtain a random value.
        Random r = new Random();
        int random = r.nextInt(1000000);

        // Set the value of the securemode parameter to 2. The value 2 indicates that TLS is applied. The signmethod parameter specifies an encryption algorithm.
        String clientOpts = "|securemode=2,authType=register,signmethod=" + HMAC_ALGORITHM + ",random=" + random + "|";

        // The ID of the MQTT client.
        String mqttClientId = clientId + clientOpts;

        // The usename of the MQTT client.
        String mqttUsername = deviceName + "&" + productKey;

        // The signature that the MQTT client uses to connect to the endpoint.
        JSONObject params = new JSONObject();
        params.put("productKey", productKey);
        params.put("deviceName", deviceName);
        params.put("random", random);
        String mqttPassword = sign(params, productSecret);

        // Send an MQTT CONNECT message for dynamic registration.
        connect(broker, mqttClientId, mqttUsername, mqttPassword);
    }

    /**
     * Send an MQTT CONNECT message for dynamic registration.
     * 
     * @param serverURL: the requested URL.
     * @param clientId: the ID of the client.
     * @param username: the username of the MQTT client.
     * @param password: the password of the MQTT client.
     */
    @SuppressWarnings("resource")
    private void connect(String serverURL, String clientId, String username, String password) {
        try {
            MemoryPersistence persistence = new MemoryPersistence();
            MqttClient sampleClient = new MqttClient(serverURL, clientId, persistence);
            MqttConnectOptions connOpts = new MqttConnectOptions();
            connOpts.setMqttVersion(4);// MQTT 3.1.1
            connOpts.setUserName(username);// The username.
            connOpts.setPassword(password.toCharArray());// The password.
            connOpts.setAutomaticReconnect(false); // Disable the automatic reconnection feature based on MQTT rules that are set for dynamic registration.
            System.out.println("----- register params -----");
            System.out.print("server=" + serverURL + ",clientId=" + clientId);
            System.out.println(",username=" + username + ",password=" + password);
            sampleClient.setCallback(new MqttCallback() {
                @Override
                public void messageArrived(String topic, MqttMessage message) throws Exception {
                    // Print out only the response of dynamic registration.
                    if (REGISTER_TOPIC.equals(topic)) {
                        String payload = new String(message.getPayload(), StandardCharsets.UTF_8);
                        System.out.println("----- register result -----");
                        System.out.println(payload);
                        sampleClient.disconnect();
                    }
                }

                @Override
                public void deliveryComplete(IMqttDeliveryToken token) {
                }

                @Override
                public void connectionLost(Throwable cause) {
                }
            });
            sampleClient.connect(connOpts);
        } catch (MqttException e) {
            System.out.print("register failed: clientId=" + clientId);
            System.out.println(",username=" + username + ",password=" + password);
            System.out.println("reason " + e.getReasonCode());
            System.out.println("msg " + e.getMessage());
            System.out.println("loc " + e.getLocalizedMessage());
            System.out.println("cause " + e.getCause());
            System.out.println("excep " + e);
            e.printStackTrace();
        }
    }

    /**
     * Generate a signature for dynamic registration.
     * 
     * @param params: the required parameters that you can use to generate a signature.
     * @param productSecret: the ProductSecret of the device.
     * @return: a hexadecimal signature string.
     */
    private String sign(JSONObject params, String productSecret) {

        // Sort request parameters in alphabetical order.
        Set<String> keys = getSortedKeys(params);

        // Remove the sign and signMethod parameters.
        keys.remove("sign");
        keys.remove("signMethod");

        // Obtain the plaintext of the signature.
        StringBuffer content = new StringBuffer();
        for (String key : keys) {
            content.append(key);
            content.append(params.getString(key));
        }

        // Generate a signature.
        String sign = encrypt(content.toString(), productSecret);
        System.out.println("sign content=" + content);
        System.out.println("sign result=" + sign);

        return sign;
    }

    /**
     * Convert a JSON object to a set of key-value pairs.
     *
     * @param json: the JSON object to be converted.
     * @return: a set of key-value pairs that are converted from the JSON object.
     */
    private Set<String> getSortedKeys(JSONObject json) {
        SortedMap<String, String> map = new TreeMap<String, String>();
        for (String key : json.keySet()) {
            String vlaue = json.getString(key);
            map.put(key, vlaue);
        }
        return map.keySet();
    }

    /**
     * Specify an encryption algorithm for the HMAC_ALGORITHM parameter.
     * 
     * @param content: the plaintext.
     * @param secret: the encryption key.
     * @return: the ciphertext.
     */
    private String encrypt(String content, String secret) {
        try {
            byte[] text = content.getBytes(StandardCharsets.UTF_8);
            byte[] key = secret.getBytes(StandardCharsets.UTF_8);
            SecretKeySpec secretKey = new SecretKeySpec(key, HMAC_ALGORITHM);
            Mac mac = Mac.getInstance(secretKey.getAlgorithm());
            mac.init(secretKey);
            return byte2hex(mac.doFinal(text));
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * Convert a binary array to a hexadecimal string.
     * 
     * @param b: the binary array.
     * @return: a hexadecimal string.
     */
    private String byte2hex(byte[] b) {
        StringBuffer sb = new StringBuffer();
        for (int n = 0; b ! = null && n < b.length; n++) {
            String stmp = Integer.toHexString(b[n] & 0XFF);
            if (stmp.length() == 1) {
                sb.append('0');
            }
            sb.append(stmp);
        }
        return sb.toString().toUpperCase();
    }

    public static void main(String[] args) throws Exception {

        String productKey = "The ProductKey of your device";
        String productSecret = "The ProductSecret of your device";
        String deviceName = "The DeviceName of your device";

        // Perform dynamic registration.
        DynamicRegisterByMqtt client = new DynamicRegisterByMqtt();
        client.register(productKey, productSecret, deviceName);

        // After dynamic registration is successful, persist the DeviceSecret on premises.
    }
}
```

