---
keyword: [IoT, 物联网, HTTPS, 动态注册]
---

# 直连设备一型一密动态注册（HTTPS通道）

本文提供基于HTTPS通道的设备一型一密预注册示例代码。

物联网平台支持多种设备安全认证方式，具体认证方式，请参见[设备安全认证](/intl.zh-CN/设备接入/设备安全认证/概述.md)。

物联网平台支持基于HTTPS通道实现[一型一密](/intl.zh-CN/设备接入/设备安全认证/一型一密.md)预注册认证，设备通过动态注册获取连接云端所需的DeviceSecret。相关Topic和参数说明，请参见[直连设备使用一型一密动态注册](/intl.zh-CN/设备管理/Alink协议/设备身份注册.md)。

本文提供基于Java语言的示例代码。

动态注册成功后，设备使用MQTT客户端直连接入物联网平台，示例代码请参见[Paho-MQTT Java接入示例](/intl.zh-CN/最佳实践/设备接入/使用Paho接入物联网平台/Paho-MQTT Java接入示例.md)。

## 前提条件

已完成[一型一密文档](/intl.zh-CN/设备接入/设备安全认证/一型一密.md)中的以下步骤：

1.  创建产品。
2.  开启动态注册。
3.  添加设备。
4.  产线烧录。

## pom.xml配置

在pom.xml文件中，添加以下依赖，引入阿里fastjson包。

```
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>fastjson</artifactId>
  <version>1.2.61</version>
</dependency>
```

## 示例代码

**说明：**

-   当设备未激活时，动态注册可以反复进行，每次获取到的deviceSecret不一样。您需要保证只成功执行一次动态注册，并将获取的deviceSecret固化到本地。
-   已激活的设备如需再次动态注册，需要重置云端设备。您可调用[ResetThing](/intl.zh-CN/云端开发指南/云端API参考/设备管理/ResetThing.md) API进行重置。

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
 * 设备动态注册。
 */
public class DynamicRegisterByHttps {

    // 地域ID，填写您的产品所在地域ID。
    private static String regionId = "cn-shanghai";

    // 定义加密方式。可选MAC算法：HmacMD5、HmacSHA1、HmacSHA256，需和signmethod一致。
    private static final String HMAC_ALGORITHM = "hmacsha1";

    /**
     * 动态注册。
     * 
     * @param productKey 产品key
     * @param productSecret 产品密钥
     * @param deviceName 设备名称
     * @throws Exception
     */
    public void register(String productKey, String productSecret, String deviceName) throws Exception {

        // 请求地址。
        URL url = new URL("https://iot-auth." + regionId + ".aliyuncs.com/auth/register/device");

        HttpsURLConnection conn = (HttpsURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-type", "application/x-www-form-urlencoded");
        conn.setDoOutput(true);
        conn.setDoInput(true);

        // 获取URLConnection对象对应的输出流。
        PrintWriter out = new PrintWriter(conn.getOutputStream());
        // 发送请求参数。
        out.print(registerdBody(productKey, productSecret, deviceName));
        // flush输出流的缓冲。
        out.flush();

        // 获取URLConnection对象对应的输入流。
        BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        // 读取URL的响应。
        String result = "";
        String line = "";
        while ((line = in.readLine()) != null) {
            result += line;
        }
        System.out.println("----- register result -----");
        System.out.println(result);

        // 关闭输入输出流。
        in.close();
        out.close();
        conn.disconnect();
    }

    /**
     * 生成动态注册请求内容。
     * 
     * @param productKey 产品key
     * @param productSecret 产品密钥
     * @param deviceName 设备名称
     * @return 动态注册payload
     */
    private String registerdBody(String productKey, String productSecret, String deviceName) {

        // 获取随机值。
        Random r = new Random();
        int random = r.nextInt(1000000);

        // 动态注册参数。
        JSONObject params = new JSONObject();
        params.put("productKey", productKey);
        params.put("deviceName", deviceName);
        params.put("random", random);
        params.put("signMethod", HMAC_ALGORITHM);
        params.put("sign", sign(params, productSecret));

        // 拼接payload。
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
     * 动态注册签名。
     * 
     * @param params 签名参数
     * @param productSecret 产品密钥
     * @return 签名十六进制字符串
     */
    private String sign(JSONObject params, String productSecret) {

        // 请求参数按字典顺序排序。
        Set<String> keys = getSortedKeys(params);

        // sign、signMethod除外
        keys.remove("sign");
        keys.remove("signMethod");

        // 组装签名明文。
        StringBuffer content = new StringBuffer();
        for (String key : keys) {
            content.append(key);
            content.append(params.getString(key));
        }

        // 计算签名。
        String sign = encrypt(content.toString(), productSecret);
        System.out.println("sign content=" + content);
        System.out.println("sign result=" + sign);

        return sign;
    }

    /**
     * 获取JSON对象排序后的key集合。
     *
     * @param json 需要排序的JSON对象
     * @return 排序后的key集合
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
     * 使用HMAC_ALGORITHM加密。
     * 
     * @param content 明文
     * @param secret 密钥
     * @return 密文
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
     * 二进制转十六进制字符串。
     * 
     * @param b 二进制数组
     * @return 十六进制字符串
     */
    private String byte2hex(byte[] b) {
        StringBuffer sb = new StringBuffer();
        for (int n = 0; b != null && n < b.length; n++) {
            String stmp = Integer.toHexString(b[n] & 0XFF);
            if (stmp.length() == 1) {
                sb.append('0');
            }
            sb.append(stmp);
        }
        return sb.toString().toUpperCase();
    }

    public static void main(String[] args) throws Exception {

        String productKey = "您的设备productKey";
        String productSecret = "您的设备productSecret";
        String deviceName = "您的设备deviceName";

        // 进行动态注册。
        DynamicRegisterByHttps client = new DynamicRegisterByHttps();
        client.register(productKey, productSecret, deviceName);

        // 动态注册成功后，需要固化deviceSecret。
    }
}
```

