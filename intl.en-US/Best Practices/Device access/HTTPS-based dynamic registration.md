---
keyword: [Internet of Things, IoT, HTTPS, dynamic registration]
---

# HTTPS-based dynamic registration

This article describes how to perform HTTPS-based dynamic registration for directly connected devices.

IoT Platform supports multiple authentication methods for devices. For more information, see [Authenticate a device](/intl.en-US/Device Access/Authenticate devices /Authenticate devices.md).

You can establish HTTPS connections to perform [pre-registration unique-certificate-per-product](/intl.en-US/Device Access/Authenticate devices /Unique-certificate-per-product authentication.md) authentication. In this case, devices are dynamically registered to obtain DeviceSecrets that are required when you connect the devices with IoT Platform. For more information about the topics and parameters that are used for HTTPS-based dynamic registration, see [Dynamically register a directly connected device based on the unique-certificate-per-product authentication](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device identity registration.md).

This topic provides the Java sample code.

After dynamic registration is successful, a device uses an MQTT client to connect with IoT Platform. For more information about sample code, see [Using Paho MQTT Java client](/intl.en-US/Best Practices/Device access/Access IoT Platform by using Paho/Using Paho MQTT Java client.md).

## Prerequisites

The following steps that are specified in the [Unique-certificate-per-product authentication](/intl.en-US/Device Access/Authenticate devices /Unique-certificate-per-product authentication.md) topic are performed:

1.  Create a product.
2.  Enable dynamic registration.
3.  Add a device.
4.  Burn the device SDK on the production line.

## Configure the pom.xml file

Add the following dependency to the pom.xml file to import the fastjson package that is provided by Alibaba Cloud:

```
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

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.util.Random;
import java.util.Set;
import java.util.SortedMap;
import java.util.TreeMap;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import javax.net.ssl.HttpsURLConnection;

import com.alibaba.fastjson.JSONObject;

/**
 * Perform dynamic registration for a device.
 */
public class DynamicRegisterByHttps {

    // Specify the ID of the region where your product resides.
    private static String regionId = "cn-shanghai";

    // Specify an encryption algorithm. Valid values: hmacmd5, hmacsha1, and hmacsha256. The value that you specify must be the same as the value of the signmethod parameter.
    private static final String HMAC_ALGORITHM = "hmacsha1";

    /**
     * Dynamic registration.
     * 
     * @param productKey: the ProductKey of the device.
     * @param productSecret: the ProductSecret of the device.
     * @param deviceName: the DeviceName of the device.
     * @throws Exception
     */
    public void register(String productKey, String productSecret, String deviceName) throws Exception {

        // The requested URL.
        URL url = new URL("https://iot-auth." + regionId + ".aliyuncs.com/auth/register/device");

        HttpsURLConnection conn = (HttpsURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-type", "application/x-www-form-urlencoded");
        conn.setDoOutput(true);
        conn.setDoInput(true);

        // Obtain the output stream of the URLConnection object.
        PrintWriter out = new PrintWriter(conn.getOutputStream());
        // Send request parameters.
        out.print(registerdBody(productKey, productSecret, deviceName));
        // Print out the content of the cache.
        out.flush();

        // Obtain the input stream of the URLConnection object.
        BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        // Obtain a response from the endpoint.
        String result = "";
        String line = "";
        while ((line = in.readLine()) ! = null) {
            result += line;
        }
        System.out.println("----- register result -----");
        System.out.println(result);

        // Close input and output streams.
        in.close();
        out.close();
        conn.disconnect();
    }

    /**
     * Generate a request for dynamic registration.
     * 
     * @param productKey: the ProductKey of the device.
     * @param productSecret: the ProductSecret of the device.
     * @param deviceName: the DeviceName of the device.
     * @return: the response payloads.
     */
    private String registerdBody(String productKey, String productSecret, String deviceName) {

        // Obtain a random value.
        Random r = new Random();
        int random = r.nextInt(1000000);

        // The required parameters for dynamic registration.
        JSONObject params = new JSONObject();
        params.put("productKey", productKey);
        params.put("deviceName", deviceName);
        params.put("random", random);
        params.put("signMethod", HMAC_ALGORITHM);
        params.put("sign", sign(params, productSecret));

        // Concatenate the payloads.
        StringBuffer payload = new StringBuffer();
        for (String key : params.keySet()) {
            payload.append(key);
            payload.append("=");
            payload.append(params.getString(key));
            payload.append("&");
        }
        payload.deleteCharAt(payload.length() - 1);

        System.out.println("----- register payload -----");
        System.out.println(payload);

        return payload.toString();
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
        DynamicRegisterByHttps client = new DynamicRegisterByHttps();
        client.register(productKey, productSecret, deviceName);

        // After dynamic registration is successful, persist the DeviceSecret on the device.
    }
}
```

