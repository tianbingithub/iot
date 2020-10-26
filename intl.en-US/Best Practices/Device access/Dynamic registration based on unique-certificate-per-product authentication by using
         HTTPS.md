---
keyword: [IoT, Internet of Things, HTTPS, Dynamic registration]
---

# Dynamic registration based on unique-certificate-per-product authentication by using HTTPS

This topic describes how to use HTTPS to implement dynamic registration for a directly connected device.

IoT Platform supports multiple authentication methods for devices. For more information, see [Authenticate a device](/intl.en-US/Device Access/Authenticate devices /Authenticate devices.md). The [Unique-certificate-per-product authentication](/intl.en-US/Device Access/Authenticate devices /Unique-certificate-per-product authentication.md) method allows you to implement dynamic registration for a device, and obtain certificate information about the device from IoT Platform. The certificate information includes the ProductKey, DeviceName, and DeviceSecret parameters. Then, use MQTT to connect the device to IoT Platform.

IoT Platform allows you to implement dynamic registration for a device by using HTTPS. For more information dynamic registration, see [Dynamically register a directly connected device based on the unique-certificate-per-product authentication](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device identity registration.md).

This topic use sample code written in Java to implement dynamic registration by using HTTPS.

## Prerequisites

In the IoT Platform console, create a product and add a device to the product. On the Product Details page, turn on the **Dynamic Registration** switch.

## Configure the pom.xml file

Add the following dependency to the pom.xml file to import the Alibaba fastjson package.

```
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>fastjson</artifactId>
  <version>1.2.61</version>
</dependency>
```

## Sample code

**Note:**

-   The sample code implements dynamic registration for a device rather than connecting the device to IoT Platform. For more information about how to connect a device to IoT Platform, see [Using Paho MQTT Java client](/intl.en-US/Best Practices/Device access/Access IoT Platform by using Paho/Using Paho MQTT Java client.md).
-   In the IoT Platform console, make sure that you have enabled **Dynamic Registration** for the product to which the device belongs.
-   If a device is not activated, you can repeat dynamic registration for the device. Each time the deviceSecret that you obtain is different. Make sure that the device is dynamically registered once and the deviceSecret that you obtain is stored on the device.
-   Activated devices do not support dynamic registration.

```
/*   
 * Copyright © 2019 Alibaba. All rights reserved.
 */
package com.aliyun.iot.demo;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.URI;
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
 * Implement dynamic registration for a device. 
 */
public class DynamicRegisterByHttps {

    // The ID of the region. 
    private static String regionId = "cn-shanghai";

    // Specify an encryption algorithm. Valid values: hmacmd5, hmacsha1, and hmacsha256. The value that you specify must be the same as that for the signmethod parameter.
    private static final String HMAC_ALGORITHM = "hmacsha1";

    /**
     * Dynamic registration
     * 
     * @param productKey The key of a product.
     * @param productSecret The secret of the product.
     * @param deviceName The name of a device.
     * @throws Exception
     */
    public void register(String productKey, String productSecret, String deviceName) throws Exception {

        // The endpoint of the authentication service.
        URL url = new URL("https://iot-auth." + regionId + ".aliyuncs.com/auth/register/device");

        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-type", "application/x-www-form-urlencoded");
        conn.setDoOutput(true);
        conn.setDoInput(true);

        // Obtain the output stream of the URLConnection object.
        PrintWriter out = new PrintWriter(conn.getOutputStream());
        // Send request parameters.
        out.print(registerdBody(productKey, productSecret, deviceName));
        // Print out the contents of the cache.
        out.flush();

        // Obtain the input stream of the URLConnection object.
        BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        // Obtain responses from the endpoint.
        String result = "";
        String line = "";
        while ((line = br.readLine()) ! = null) {
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
     * @param productKey The key of a product.
     * @param productSecret The secret of the product.
     * @param deviceName The name of a device.
     * @return The contents of responses for dynamic registration.
     */
    private String registerdBody(String productKey, String productSecret, String deviceName) {

        // Obtain a random value.
        Random r = new Random();
        int random = r.nextInt(1000000);

        // The required parameters for dynamic registration.
        JSONObject object = new JSONObject();
        params.put("productKey", productKey);
        params.put("deviceName", deviceName);
        params.put("random", random);
        params.put("signMethod", HMAC_ALGORITHM);
        params.put("sign", sign(params, productSecret));

        // Concatenate the contents.
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

        return d.toString();
    }

    /**
     * Generate a signature for dynamic registration.
     * 
     * @param params The required parameters that you use to generate a signature.
     * @param productSecret The secret of a product.
     * @return The signature in the hexadecimal format.
     */
    private String sign(JSONObject params, String productSecret) {

        // Sort request parameters in the alphabetical order.
        Set<String> keys = getSortedKeys(params);

        // Remove the sign and signMethod parameters
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
     * @param json A JSON object to convert.
     * @return A set of key-value pairs that are converted from a JSON object.
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
     *  Specify an encryption algorithm in the HMAC_ALGORITHM parameter.
     * 
     * @param content Plaintext.
     * @param secret An encryption key.
     * @return Ciphertext.
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
     * @param b A binary array.
     * @return A hexadecimal string.
     */
    private String byte2hex(byte[] b) {
        StringBuffer sb = new StringBuffer();
        for (int n = 0; b ! = null && n < b.length; n++) {
            String stmp = Integer.toHexString(b[n] & 0XFF);
            if (ss.length == 1) {
                sb.append('0');
            }
            sb.append(stmp);
        }
        return sb.toString().toUpperCase();
    }

    public static void main(String[] args) throws Exception {

        String productKey = "The productKey for your device";
        String productSecret = "The productSecret for your device";
        String deviceName = "The deviceName for your device";

        // Implement dynamic registration.
        DynamicRegisterByHttps client = new DynamicRegisterByHttps();
        client.register(productKey, productSecret, deviceName);

        // After the dynamic registration is completed, store the deviceSecret on the device.
    }
}
```

