# GenerateOTAUploadURL

调用该接口生成升级包文件上传到OSS的URL及详细信息。

## 使用说明

该接口与其他接口结合使用完成升级包创建。创建升级包的步骤：

1. 调用本接口生成升级包文件上传到对象存储（OSS）的信息。

本接口的返回参数包含：

-   调用OSS [PostObject](~~31988~~)上传升级包文件的请求参数：**Key**、**OSSAccessKeyId**、**Signature**和**Policy**。
-   调用[CreateOTAFirmware](~~147311~~)创建升级包的请求参数**FirmwareUrl**。

2. 请在本接口返回结果后的1分钟之内，使用[OSS SDK](~~52834~~)调用[PostObject](~~31988~~)接口上传升级包文件。上传文件的代码示例，请参见下文返回参数的用途章节。

**说明：** 本接口返回的参数信息有效期为1分钟，请在1分钟内上传升级包。上传的升级包文件大小不能超过1,000 MB。

3. 升级包上传完成后，请在60分钟内，调用物联网平台API [CreateOTAFirmware](~~147311~~)接口创建升级包。

如果上传了升级包，但未调用CreateOTAFirmware接口创建升级包，上传的文件将被系统定期自动清理。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=GenerateOTAUploadURL&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GenerateOTAUploadURL|系统规定参数。取值：GenerateOTAUploadURL。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |
|FileSuffix|String|否|apk|升级包文件扩展名。可选扩展名：bin、apk、tar、gz、tar.gz、zip、gzip。

 默认扩展名为bin。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Struct| |调用成功时，返回的文件上传信息。详情见以下参数信息。 |
|FirmwareUrl|String|https://iotx-ota.oss-cn-shanghai.aliyuncs.com/ota/65dfcda0473be29836dfde585472\*\*\*\*/ck2nfzljo00023g7kysg0\*\*\*\*.bin|文件的URL，即文件在对象存储（OSS）上的存储地址。

 升级包文件上传成功后，使用此参数调用[CreateOTAFirmware](~~147311~~)接口创建升级包。 |
|Host|String|https://iotx-ota.oss-cn-shanghai.aliyuncs.com|OSS的接入域名。 |
|Key|String|ota/65dfcda0473be29836dfde585472\*\*\*\*/ck2nfzljo00023g7kysg0\*\*\*\*.bin|调用OSS API PostObject上传对象（即文件）的名称，包含OSS对象的完整路径。 |
|OSSAccessKeyId|String|cS8uRRy54Rsz\*\*\*\*|OSS Bucket拥有者的AccessKeyId。

 该OSS Bucket将存储文件。 |
|ObjectStorage|String|OSS|对象存储类型。默认为OSS。 |
|Policy|String|eyJleHBpcmF\*\*\*\*|OSS通过该参数验证请求表单域的合法性。 |
|Signature|String|v6lViO4FBvfquajQjg20K5hK\*\*\*\*|根据**AccessKeySecret**和**Policy**计算出的签名信息。调用OSS API时，OSS验证该签名信息，从而确认Post请求的合法性。 |
|UtcCreate|String|2019-11-04T06:21:54.607Z|生成文件上传URL的时间，UTC格式。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|74C2BB8D-1D6F-41F5-AE68-6B2310883F63|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。**true**表示调用成功，**false**表示调用失败。 |

## 返回参数的用途

调用OSS [PostObject](~~31988~~)接口时，使用本接口的返回参数值作为请求参数值，将您编辑好的升级包文件上传到对象存储（OSS）。

以下是向对象存储OSS上传文件的Java代码示例。

-   在pom.xml中添加以下依赖。

    ```
    <dependency>
      <groupId>org.apache.httpcomponents</groupId>
      <artifactId>httpclient</artifactId>
      <version>4.5.3</version>
    </dependency>
    
    <dependency>
      <groupId>org.apache.httpcomponents</groupId>
      <artifactId>httpmime</artifactId>
      <version>4.5.10</version>
    </dependency>
    
    ```

-   上传升级包文件的代码如下。

    ```
    public static boolean postObject(String key,
                                      String host,
                                      String policy,
                                      String ossAccessKeyId,
                                      String signature,
                                      String data) throws IOException {
      CloseableHttpClient httpClient = HttpClients.createDefault();
      HttpPost uploadFile = new HttpPost(host);
    
      MultipartEntityBuilder builder = MultipartEntityBuilder.create();
      builder.addTextBody("key", key, ContentType.TEXT_PLAIN);
      builder.addTextBody("policy", policy, ContentType.TEXT_PLAIN);
      builder.addTextBody("OSSAccessKeyId", ossAccessKeyId, ContentType.TEXT_PLAIN);
      builder.addTextBody("signature", signature, ContentType.TEXT_PLAIN);
      builder.addTextBody("success_action_status", "200", ContentType.TEXT_PLAIN);
      builder.addBinaryBody("file", data.getBytes());
    
      HttpEntity multipart = builder.build();
      uploadFile.setEntity(multipart);
      CloseableHttpResponse response = httpClient.execute(uploadFile);
    
      if (response.getStatusLine().getStatusCode() == 200) {
        return true;
      }
    
      return false;
    }
    
    ```


## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=GenerateOTAUploadURL
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<GenerateOTAUploadURLResponse>
    <Data>
        <Key>ota/65dfcda0473be29836dfde585472****/ck2nfzljo00023g7kysg0****.bin</Key>
        <Host>https://iotx-ota.oss-cn-shanghai.aliyuncs.com</Host>
        <Policy>eyJleHBpcmF****</Policy>
        <OSSAccessKeyId>cS8uRRy54Rsz****</OSSAccessKeyId>
        <ObjectStorage>OSS</ObjectStorage>
        <UtcCreate>2019-11-04T06:21:54.607Z</UtcCreate>
        <Signature>PKmRTy40QxqIUUWy325SCT/****</Signature>
        <FirmwareUrl>https://iotx-ota.oss-cn-shanghai.aliyuncs.com/ota/65dfcda0473be29836dfde585472****/ck2nfzljo00023g7kysg0****.bin</FirmwareUrl>
    </Data>
    <RequestId>B6E77674-09C4-4647-BF85-59CB72A72E4B</RequestId>
    <Success>true</Success>
</GenerateOTAUploadURLResponse>
```

`JSON` 格式

```
{
    "Data": {
        "Key": "ota/65dfcda0473be29836dfde585472****/ck2nfzljo00023g7kysg0****.bin",
        "Host": "https://iotx-ota.oss-cn-shanghai.aliyuncs.com",
        "Policy": "eyJleHBpcmF****",
        "OSSAccessKeyId": "cS8uRRy54Rsz****",
        "ObjectStorage": "OSS",
        "UtcCreate": "2019-11-04T06:21:54.607Z",
        "Signature": "PKmRTy40QxqIUUWy325SCT/****",
        "FirmwareUrl": "https://iotx-ota.oss-cn-shanghai.aliyuncs.com/ota/65dfcda0473be29836dfde585472****/ck2nfzljo00023g7kysg0****.bin"
    },
    "RequestId": "B6E77674-09C4-4647-BF85-59CB72A72E4B",
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/Iot)查看更多错误码。

