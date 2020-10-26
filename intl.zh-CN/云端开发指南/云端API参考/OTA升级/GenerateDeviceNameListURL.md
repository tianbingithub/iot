# GenerateDeviceNameListURL

调用该接口生成设备列表文件上传到OSS的URL及详细信息。在创建静态升级批次时，设备列表文件可用于指定要升级的设备。

## 设备列表文件要求

-   设备列表文件包含设备DeviceName，以换行或者英文逗号分隔。支持CSV格式，文件大小不能超过5 MB。
-   单个设备列表文件最多可包含升级包对应产品下的10,000个设备，否则使用该文件创建静态升级批次时将报错。

## 使用说明

该接口与其他接口结合使用完成设备列表文件上传。上传设备列表文件的步骤：

1. 调用本接口生成设备列表文件上传到对象存储（OSS）的信息。

本接口的返回参数包含：

调用OSS [PostObject](~~31988~~)上传设备列表文件的请求参数：**Key**、**AccessKeyId**、**Signature**和**Policy**。

2. 请在本接口返回结果后的1分钟之内，使用[OSS SDK](~~52834~~)调用[PostObject](~~31988~~)接口上传设备列表文件。上传文件的代码示例，请参见下文返回参数的用途章节。

**说明：** 本接口返回的参数信息有效期为1分钟，请在1分钟内上传设备列表文件。

3. 设备列表上传完成后，请在60分钟内，调用物联网平台API [CreateOTAStaticUpgradeJob](~~147496~~)创建静态升级批次。

如果上传了设备列表，但未调用CreateOTAStaticUpgradeJob创建静态升级批次，上传的文件将被系统定期自动清理。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=GenerateDeviceNameListURL&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GenerateDeviceNameListURL|系统规定参数。取值：GenerateDeviceNameListURL。 |
|IotInstanceId|String|否|iot-cn-0pp1n8t\*\*\*\*|实例ID。公共实例不传此参数；您购买的实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Struct| |调用成功时，返回的文件上传信息。详情见以下参数信息。 |
|AccessKeyId|String|cS8uRRy54Rsz\*\*\*\*|OSS Bucket拥有者的AccessKeyId。

 该OSS Bucket将存储文件。 |
|FileUrl|String|https://iotx-ota.oss-cn-shanghai.aliyuncs.com/ota/65dfcda0473be29836dfde585472\*\*\*\*/ck2nfzljo00023g7kysg0\*\*\*\*.csv|文件的URL，即文件在对象存储（OSS）上的存储地址。

 设备列表文件上传成功后，使用此参数调用[CreateOTAStaticUpgradeJob](~~147496~~)接口创建静态批量升级批次。 |
|Host|String|https://iotx-ota.oss-cn-shanghai.aliyuncs.com|OSS的接入域名。 |
|Key|String|ota/65dfcda0473be29836dfde585472\*\*\*\*/ck2nfzljo00023g7kysg0\*\*\*\*.csv|调用OSS API PostObject上传对象（即文件）的名称，包含OSS对象的完整路径。 |
|ObjectStorage|String|OSS|对象存储类型。默认为OSS。 |
|Policy|String|eyJleHBpcmF\*\*\*\*|OSS通过该参数验证请求表单域的合法性。 |
|Signature|String|v6lViO4FBvfquajQjg20K5hK\*\*\*\*|根据**AccessKeySecret**和**Policy**计算出的签名信息。调用OSS API时，OSS验证该签名信息，从而确认Post请求的合法性。 |
|UtcCreate|String|2019-11-04T06:21:54.607Z|生成文件上传URL的时间，UTC格式。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|74C2BB8D-1D6F-41F5-AE68-6B2310883F63|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 返回参数的用途

调用OSS [PostObject](~~31988~~)接口时，使用本接口的返回参数值作为请求参数值，将您编辑好的文件上传到对象存储（OSS）。

以下是向对象存储OSS上传文件的Java代码示例：

-   在pom.xml中添加以下依赖：

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

-   上传文件的代码如下：

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
  builder.addTextBody("AccessKeyId", ossAccessKeyId, ContentType.TEXT_PLAIN);
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
https://iot.cn-shanghai.aliyuncs.com/?Action=GenerateDeviceNameListURL
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<GenerateDeviceNameListURLResponse>
      <Data>
            <Policy>eyJleHBpcmF****</Policy>
            <FileUrl>https://iotx-ota.oss-cn-shanghai.aliyuncs.com/ota/65dfcda0473be29836dfde585472****/ck2nfzljo00023g7kysg0****.csv</FileUrl>
            <UtcCreate>2019-11-04T06:21:54.607Z</UtcCreate>
            <AccessKeyId>cS8uRRy54Rsz****</AccessKeyId>
            <Signature>v6lViO4FBvfquajQjg20K5hK****</Signature>
            <ObjectStorage>OSS</ObjectStorage>
            <Host>https://iotx-ota.oss-cn-shanghai.aliyuncs.com</Host>
            <Key>ota/65dfcda0473be29836dfde585472****/ck2nfzljo00023g7kysg0****.csv</Key>
      </Data>
      <RequestId>74C2BB8D-1D6F-41F5-AE68-6B2310883F63</RequestId>
      <Success>true</Success>
</GenerateDeviceNameListURLResponse>
```

`JSON` 格式

```
{
    "Data": {
        "Policy": "eyJleHBpcmF****",
        "FileUrl": "https://iotx-ota.oss-cn-shanghai.aliyuncs.com/ota/65dfcda0473be29836dfde585472****/ck2nfzljo00023g7kysg0****.csv",
        "UtcCreate": "2019-11-04T06:21:54.607Z",
        "AccessKeyId": "cS8uRRy54Rsz****",
        "Signature": "v6lViO4FBvfquajQjg20K5hK****",
        "ObjectStorage": "OSS",
        "Host": "https://iotx-ota.oss-cn-shanghai.aliyuncs.com",
        "Key": "ota/65dfcda0473be29836dfde585472****/ck2nfzljo00023g7kysg0****.csv"
    },
    "RequestId": "74C2BB8D-1D6F-41F5-AE68-6B2310883F63",
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/Iot)查看更多错误码。

