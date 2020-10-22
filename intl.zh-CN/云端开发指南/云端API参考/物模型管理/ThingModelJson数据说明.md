---
keyword: [IoT, ThingModelJson, extendConfig]
---

# ThingModelJson数据说明

调用某些物模型相关API时，请求参数或返回参数中包含参数ThingModelJson，该参数的取值格式为物模型功能定义在物联网平台系统的存储结构，与TSL中的数据结构有所不同。

ThingModelJson中所有字段按照key的字符排序。

## 数据结构

```
{
  "_ppk":{
       "description":"test",
       "version":"159244410****"
  }
  "events":[],
  "productKey":"al12345****",
  "properties":[],
  "services":[]
}
```

|参数|类型|说明|
|--|--|--|
|productKey|String|物模型所属产品的ProductKey。|
|\_ppk|String|物模型版本信息，包含version和description参数。|
|version|String|当前物模型版本号。仅发布后的正式版物模型才有此参数。|
|description|String|当前物模型版本的描述。仅发布后的正式版物模型才有此参数。|
|properties|List|物模型中的属性列表。关于属性数据结构中包含的参数说明，请参见[属性数据结构规范](#section_2ra_ly5_hdb)。每个属性数据结构中，可以使用extendConfig来定义物模型扩展描述的配置，请参见[extendConfig数据结构规范](#section_6bu_yuq_41z)。当属性无扩展描述时，无需传入extendConfig。 |
|services|List|物模型中的服务列表。关于服务数据结构中包含的参数说明，请参见[服务数据格式规范](#section_ltc_ec1_kmx)。每个服务数据结构中，可以使用extendConfig来定义物模型扩展描述的配置，请参见[extendConfig数据结构规范](#section_6bu_yuq_41z)。当服务无扩展描述时，无需传入extendConfig。 |
|events|List|物模型中的事件列表。关于事件数据结构中包含的参数说明，请参见[事件数据格式规范](#section_211_jtv_7kn)。每个事件数据结构中，可以使用extendConfig来定义物模型扩展描述的配置，请参见[extendConfig数据结构规范](#section_6bu_yuq_41z)。当事件无扩展描述时，无需传入extendConfig。 |

## 属性数据结构规范

以下表格展示属性定义中需包含的参数和说明。

|参数|类型|是否必需|说明|
|--|--|----|--|
|productKey|String|是|物模型所属产品的ProductKey。|
|identifier|String|是|属性的标识符。可包含大小写英文字母、数字、下划线（\_），长度不超过50个字符。 **说明：** 不能用以下词汇作为标识符：set、get、post、property、event、time、value。 |
|dataType|String|是|属性值的数据类型。 可选值：ARRAY、STRUCT、INT、FLOAT、DOUBLE、TEXT、DATE、ENUM、BOOL。

不同数据类型，可传入的参数不同。详情请参见本文中对应数据类型的数据规范章节。 |
|name|String|是|属性名称。可包含中文汉字、大小写英文字母、数字、短划线（-）、下划线（\_）和英文句号（.），且必须以中文汉字、英文字母或数字开头，长度不超过30个字符，一个中文汉字计为一个字符。|
|rwFlag|String|是|在云端可以对该属性进行的操作类型。 -   READ\_WRITE：读写。
-   READ\_ONLY：只读。
-   WRITE\_ONLY：只写。 |
|dataSpecs|Object|否|数据类型（dataType）为非列表型（INT、FLOAT、DOUBLE、TEXT、DATE、ARRAY）的数据规范存储在dataSpecs中。 **说明：**

-   除属性、服务、事件和参数定义数据以外，其他数据都属于数据规范。
-   dataSpecs和dataSpecsList之中必须传入且只能传入一个，请根据实际数据类型传入。 |
|dataSpecsList|List|否|数据类型（dataType）为列表型（ENUM、BOOL、STRUCT）的数据规范存储在dataSpecsList中。 **说明：**

-   除属性、服务、事件和参数定义数据以外，其他数据都属于数据规范。
-   dataSpecs和dataSpecsList之中必须传入且只能传入一个，请根据实际数据类型传入。 |
|required|Boolean|是|是否是标准品类的必选属性。 -   true：是
-   false：否 |
|custom|Boolean|是|是否是自定义功能。 -   true：是
-   false：否 |

## 服务数据格式规范

以下表格展示服务定义中需包含的参数和说明。

|参数|类型|是否必填|说明|
|--|--|----|--|
|productKey|String|是|物模型所属产品的ProductKey。|
|identifier|String|是|服务的标识符。可包含大小写英文字母、数字、下划线（\_），长度不超过50个字符。 **说明：** 不能用以下词汇作为标识符：set、get、post、property、event、time、value。 |
|serviceName|String|是|服务名称。可包含中文汉字、大小写英文字母、数字、短划线（-）、下划线（\_）和英文句号（.），且必须以中文汉字、英文字母或数字开头，长度不超过30个字符，一个中文汉字计为一个字符。|
|inputParams|List|否|服务的输入参数。数据结构说明，请参见[输入、输出参数结构规范](#section_owk_mxx_wq8)。|
|outputParams|List|否|服务的输出参数。数据结构说明，请参见[输入、输出参数结构规范](#section_owk_mxx_wq8)。|
|required|Boolean|是|是否是标准品类的必选服务。 -   true：是
-   false：否 |
|callType|String|是|服务的调用方式。 -   ASYNC：异步调用
-   SYNC：同步调用 |
|custom|Boolean|是|是否是自定义功能。 -   true：是
-   false：否 |

## 事件数据格式规范

以下表格展示事件定义中需包含的参数和说明。

|参数|类型|是否必填|说明|
|--|--|----|--|
|productKey|String|是|物模型所属产品的ProductKey。|
|identifier|String|是|事件的标识符。可包含大小写英文字母、数字、下划线（\_），长度不超过50个字符。 **说明：** 不能用以下词汇作为标识符：set、get、post、property、event、time、value。 |
|eventName|String|是|事件名称。可包含中文汉字、大小写英文字母、数字、短划线（-）、下划线（\_）和英文句号（.），且必须以中文、英文或数字开头，长度不超过30个字符，一个中文汉字计为一个字符。|
|eventType|String|是|事件类型。 -   INFO\_EVENT\_TYPE：信息。
-   ALERT\_EVENT\_TYPE：告警。
-   ERROR\_EVENT\_TYPE：故障。 |
|outputdata|List|否|事件的输出参数。数据结构说明，请参见[输入、输出参数结构规范](#section_owk_mxx_wq8)。|
|required|Boolean|是|是否是标准品类的必选事件。 -   true：是
-   false：否 |
|custom|Boolean|是|是否是自定义功能。 -   true：是
-   false：否 |

## 输入、输出参数结构规范

服务或事件中的输入或输出参数的数据结构规范如下。

|参数|类型|是否必填|说明|
|--|--|----|--|
|dataType|String|是|参数值的数据类型。 可选值：ARRAY、STRUCT、INT、FLOAT、DOUBLE、TEXT、DATE、ENUM、BOOL。

各数据类型的数据规范，请参见本文中对应数据类型的数据规范章节。 |
|identifier|String|是|参数的标识符。可包含大小写英文字母、数字、下划线（\_），长度不超过50个字符。 **说明：** 不能用以下词汇作为标识符：set、get、post、property、event、time、value。 |
|name|String|是|参数名称。可包含中文汉字、大小写英文字母、数字、短划线（-）、下划线（\_）和英文句号（.），且必须以中文汉字、英文字母或数字开头，长度不超过30个字符，一个中文汉字计为一个字符。|
|direction|String|是|表示参数是输入参数还是输出参数。 -   PARAM\_INPUT：输入参数。
-   PARAM\_OUTPUT：输出参数。 |
|paraOrder|Integer|是|参数的序号。从0开始排序，且不能重复。|
|dataSpecs|Object|否|数据类型（dataType）为非列表型（INT、FLOAT、DOUBLE、TEXT、DATE、ARRAY）的数据规范存储在dataSpecs中。 **说明：**

-   除属性、服务、事件和参数定义数据以外，其他数据都属于数据规范。
-   dataSpecs和dataSpecsList之中必须传入且只能传入一个，请根据实际数据类型传入。 |
|dataSpecsList|List|否|数据类型（dataType）为列表型（ENUM、BOOL、STRUCT）的数据规范存储在dataSpecsList中。**说明：**

-   除属性、服务、事件和参数定义数据以外，其他数据都属于数据规范。
-   dataSpecs和dataSpecsList之中必须传入且只能传入一个，请根据实际数据类型传入。 |
|custom|Boolean|是|参数是否隶属于自定义功能。 -   true：是
-   false：否 |

## INT、FLOAT、DOUBLE类型数据结构规范

当功能或参数的数据类型为INT、FLOAT或DOUBLE时，数据结构中包含的参数如下。

|参数|类型|是否必需|说明|
|--|--|----|--|
|dataType|String|是|取值为INT、FLOAT或DOUBLE。|
|max|String|是|最大值。取值为INT、FLOAT或DOUBLE。|
|min|String|是|最小值。取值为INT、FLOAT或DOUBLE。|
|step|String|是|步长。取值为INT、FLOAT或DOUBLE。|
|precise|String|否|精度。当dataType取值为FLOAT或DOUBLE时，可传入的参数。|
|defaultValue|String|否|传入此参数，可存入一个默认值。|
|unit|String|是|单位的符号。|
|unitName|String|是|单位的名称。|
|custom|Boolean|是|是否是自定义功能。 -   true：是
-   false：否 |

## DATE、TEXT类型数据结构规范

当功能或参数的数据类型为DATE或TEXT时，数据结构中包含的参数如下。

|参数|类型|是否必需|说明|
|--|--|----|--|
|dataType|String|是|取值为DATE或TEXT。|
|length|Long|是|数据长度，取值不能超过2048，单位：字节。dataType取值为TEXT时，需传入该参数。|
|defaultValue|String|否|传入此参数，可存入一个默认值。|
|custom|Boolean|是|是否是自定义功能。 -   true：是
-   false：否 |

## ARRAY类型数据规范

当功能或参数的数据类型为ARRAY时，数据结构中包含的参数如下。

|参数|类型|是否必需|说明|
|--|--|----|--|
|dataType|String|是|取值为ARRAY。|
|size|Long|是|数组中的元素个数。|
|childDataType|String|是|数组中的元素的数据类型。可选值：STRUCT、INT、FLOAT、DOUBLE或TEXT。|
|dataSpecs|Object|否|数据类型（dataType）为非列表型（INT、FLOAT、DOUBLE、TEXT、DATE、ARRAY）的数据规范存储在dataSpecs中。 **说明：**

-   除属性、服务、事件和参数定义数据以外，其他数据都属于数据规范。
-   dataSpecs和dataSpecsList之中必须传入且只能传入一个，请根据实际数据类型传入。 |
|dataSpecsList|List|否|数据类型（dataType）为列表型（ENUM、BOOL、STRUCT）的数据规范存储在dataSpecsList中。 **说明：**

-   除属性、服务、事件和参数定义数据以外，其他数据都属于数据规范。
-   dataSpecs和dataSpecsList之中必须传入且只能传入一个，请根据实际数据类型传入。 |
|custom|Boolean|是|是否是自定义功能。 -   true：是
-   false：否 |

## 枚举、布尔类型数据规范

当功能或参数的数据类型为BOOL或ENUM时，数据结构中包含的参数如下。

|参数|类型|是否必需|说明|
|--|--|----|--|
|dataType|String|是|取值为BOOL或ENUM。|
|name|String|是|枚举项的名称，可包含中文汉字、大小写英文字母、数字、下划线（\_）和短划线（-），且必须以中文汉字、英文字母或数字开头。长度不超过20个字符，一个中文汉字计为一个字符。|
|value|Integer|是|枚举值。|
|custom|Boolean|是|是否是自定义功能。 -   true：是
-   false：否 |

## STRUCT类型数据结构规范

当功能或参数的数据类型为STRUCT时，数据结构中包含的参数如下。

|参数|类型|是否必需|说明|
|--|--|----|--|
|dataType|String|是|取值为STRUCT。|
|identifier|String|是|结构体中的子参数的标识符。可包含大小写英文字母、数字、下划线（\_），长度不超过50个字符。 **说明：** 不能用以下词汇作为标识符：set、get、post、property、event、time、value。 |
|name|String|是|结构体中的子参数名称。可包含中文汉字、大小写英文字母、数字、短划线（-）、下划线（\_）和英文句号（.），且必须以中文汉字、英文字母或数字开头，长度不超过30个字符，一个中文汉字计为一个字符。|
|childDataType|String|否|结构体中的子参数数据类型。 可选值：INT、FLOAT、DOUBLE、TEXT、DATE、ENUM、BOOL。 |
|childName|String|是|结构体中的子参数名称。可包含中文汉字、大小写英文字母、数字、短划线（-）、下划线（\_）和英文句号（.），且必须以中文汉字、英文字母或数字开头，长度不超过30个字符，一个中文汉字计为一个字符。|
|dataSpecs|Object|否|数据类型（dataType）为非列表型（INT、FLOAT、DOUBLE、TEXT、DATE、ARRAY）的数据规范存储在dataSpecs中。

**说明：**

-   除属性、服务、事件和参数定义数据以外，其他数据都属于数据规范。
-   dataSpecs和dataSpecsList之中必须传入且只能传入一个，请根据实际数据类型传入。 |
|dataSpecsList|List|否|数据类型（dataType）为列表型（ENUM、BOOL、STRUCT）的数据规范存储在dataSpecsList中。 **说明：**

-   除属性、服务、事件和参数定义数据以外，其他数据都属于数据规范。
-   dataSpecs和dataSpecsList之中必须传入且只能传入一个，请根据实际数据类型传入。 |
|custom|Boolean|是|是否是自定义功能。 -   true：是
-   false：否 |

## extendConfig数据结构规范

每个属性、事件或服务数据结构中，可以使用extendConfig来定义物模型扩展描述的配置。扩展描述为设备通信协议到标准物模型的映射关系。

**说明：** 返回数据中的configCode是系统为单个功能定义扩展描述生成的唯一标识符。

目前系统支持接入网关协议为Modbus、OPC UA或自定义的设备配置扩展描述。不同类型的扩展描述需要满足不同的数据规范：

**Modbus类型**

Modbus只支持属性类型的物模型扩展描述。

**说明：** 为了完整展示extendConfig的结构，以下示例中包含所有参数，不代表实际使用中可能出现的组合。各参数的使用场景请参见参数说明。

```
{
  "identifier":"extend1",
  "writeFunctionCode":0,
  "writeOnly":0,
  "registerAddress":"0xFE",
  "operateType":"coilStatus",
  "scaling":0.1,
  "pollingTime":1000,
  "trigger":1,
  "bitMask":128,
  "originalDataType":{
     "type":"uint64",
     "specs":{
        "registerCount":4,
        "swap16":0,
        "reverseRegister":0}
  }
}
```

|参数|类型|说明|
|--|--|--|
|identifier|String|属性唯一标识符（产品下唯一）。|
|registerAddress|String|寄存器地址，必须以0x开头，且限制范围是0x0~0xFFFF，例如0xFE。|
|operateType|String|操作类型，取值：-   coilStatus：线圈状态
-   inputStatus：离散量输入
-   holdingRegister：保持寄存器
-   inputRegister：输入寄存器 |
|writeFunctionCode|Integer|读写操作，对于不同操作类型（operateType），可选的取值不同：-   coilStatus：
    -   5：读写（读0x01，写0x05）
    -   15：读写（读0x01，写0x0F）
    -   0：只读0x01
    -   6：只写0x05
    -   15：只写0x0F
-   inputStatus：
    -   0：只读0x02
-   holdingRegister：
    -   6：读写（读0x03，写0x06）
    -   16：读写（读0x03，写0x10）
    -   0：只读0x03
    -   6：只写0x06
    -   16：只写0x10
-   inputRegister：
    -   0：只读0x04 |
|writeOnly|Integer|是否只写。-   0：非只写。
    -   当writeFunctionCode取值不为0（表示读写）时，writeOnly为0表示支持读写。
    -   当writeFunctionCode取值为0（表示只读）时，writeOnly必须为0。
-   1：只写。

仅当writeFunctionCode取值不为0（表示读写）时，writeOnly可以为1，表示仅支持写。 |
|scaling|Number|缩放因子，不能为0。string、bool无该参数。 |
|pollingTime|Integer|采集间隔，单位是ms。无需传入，将使用设备配置的采集间隔。|
|trigger|Integer|数据上报方式。1代表按时上报，2代表变更上报。|
|bitMask|Integer|bool特有的参数。掩码，取值：1、2、4、8、16、32、64、128、256、512、1024、2048、4096、8192、16384、32768，即1<<\(0~15\)。 |
|originalDataType|Object|原始数据类型描述。|
|type|String|原始数据类型，需要为基础类型：int16、uint16、int32、uint32、int64、uint64、float、double、string、bool、customized data（按大端顺序返回hex data）。|
|specs|Object|部分数据类型特有的参数。|
|registerCount|Integer|string、customized data特有的参数。寄存器的数据个数。 |
|swap|Integer|除string、customized data外，其他数据类型特有的参数。是否交换寄存器内高低字节，把寄存器内16位数据的前后8个bit互换（byte1byte2 -\> byte2byte1）。

-   0：不交换
-   1：交换 |
|reverseRegister|Integer|除string、customized data外，其他数据类型特有的参数。是否交换寄存器顺序，把原始数据32位数据的前后16个bit互换（byte1byte2byte3byte4 -\> byte3byte4byte1byte2）。

-   0：不交换
-   1：交换 |

**OPC UA类型**

OPC UA支持属性、服务、事件类型的物模型扩展描述。

```
{
  "identifier":"extend2",
  "displayName":"Action",
  "inputData":[
    {
      "identifier":"xxxx",
      "index":1
    },
    {
      "identifier":"xxxx",
      "index":2 
    }
  ],
  "outputData":[
     {
      "identifier":"xxxx",
      "index":1
    },
    {
      "identifier":"xxxx",
      "index":2
    }
  ]
}
```

|参数|类型|说明|
|--|--|--|
|identifier|String|属性、服务、事件的唯一标识符（产品下唯一）。|
|displayName|String|属性、事件需要传入displayName，服务不需要传入。|
|inputData|List|输入数据。|
|outputData|List|输出数据。|
|identifier|String|输入数据、输出数据的唯一标识符（产品下唯一）。|
|index|Integer|索引。inputData中的index不能重复，outputData中的index不能重复。|

**自定义类型**

自定义类型支持属性、服务、事件类型的物模型扩展描述。

```
{
  "identifier":"xxx",
  "customize":{}
}
```

|参数|类型|说明|
|--|--|--|
|identifier|String|属性、服务、事件的唯一标识符（产品下唯一）。|
|customize|Object|自定义JSON。|

## 校验

您可以通过[json-schema](https://github.com/everit-org/json-schema)对ThingModelJson中的入参进行预校验。

校验Demo如下。

```
package com.aliyun.iot.thingmodel;

import java.io.InputStream;
import java.util.ArrayList;
import java.util.Arrays;

import org.everit.json.schema.Schema;
import org.everit.json.schema.ValidationException;
import org.everit.json.schema.loader.SchemaLoader;
import org.json.JSONObject;
import org.json.JSONTokener;

/**
 * @author: ***
 * @date: 2020-01-14 15:11
 */
public class ThingModelJsonValidator {

    public static void main(String[] args) throws Exception {

        try (InputStream inputStream = ThingModelJsonValidator.class.getClassLoader().getResourceAsStream(
            "thing-model-schema.json")) {
            JSONObject rawSchema = new JSONObject(new JSONTokener(inputStream));
            Schema schema = SchemaLoader.load(rawSchema);
            long start = System.currentTimeMillis();
            JSONObject object = new JSONObject();
            String jsonStr = "{\n"
                + "\t\t\t\"productKey\": \"a1Q1Yrc****\",\n"
                + "\t\t\t\"name\": \"报警事件\",\n"
                + "\t\t\t\"identifier\": \"alarmEvent\",\n"
                + "\t\t\t\"eventName\": \"报警事件\",\n"
                + "\t\t\t\"eventType\": \"ALERT_EVENT_TYPE\",\n"
                + "\t\t\t\"outputData\": [\n"
                + "\t\t\t\t{\n"
                + "\t\t\t\t\t\"paraOrder\": 0,\n"
                + "\t\t\t\t\t\"direction\": \"PARAM_OUTPUT\",\n"
                + "\t\t\t\t\t\"dataSpecsList\": [\n"
                + "\t\t\t\t\t\t{\n"
                + "\t\t\t\t\t\t\t\"dataType\": \"ENUM\",\n"
                + "\t\t\t\t\t\t\t\"name\": \"防拆报警\",\n"
                + "\t\t\t\t\t\t\t\"value\": 0\n"
                + "\t\t\t\t\t\t},\n"
                + "\t\t\t\t\t\t{\n"
                + "\t\t\t\t\t\t\t\"dataType\": \"ENUM\",\n"
                + "\t\t\t\t\t\t\t\"name\": \"防拆报警解除\",\n"
                + "\t\t\t\t\t\t\t\"value\": 1\n"
                + "\t\t\t\t\t\t}\n"
                + "\t\t\t\t\t],\n"
                + "\t\t\t\t\t\"dataType\": \"ENUM\",\n"
                + "\t\t\t\t\t\"identifier\": \"alarmType\",\n"
                + "\t\t\t\t\t\"name\": \"报警类型\",\n"
                + "\t\t\t\t\t\"index\": 0,\n"
                + "\t\t\t\t\t\"custom\": true\n"
                + "\t\t\t\t}\n"
                + "\t\t\t],\n"
                + "\t\t\t\"outputParams\": [\n"
                + "\t\t\t\t{\n"
                + "\t\t\t\t\t\"index\": 0,\n"
                + "\t\t\t\t\t\"identifier\": \"alarmType\"\n"
                + "\t\t\t\t}\n"
                + "\t\t\t],\n"
                + "\t\t\t\"custom\": true\n"
                + "\t\t}";

            object.put("properties", new ArrayList<>());
            object.put("services", new ArrayList<>());
            object.put("events", Arrays.asList(com.alibaba.fastjson.JSONObject.parseObject(jsonStr)));
            object.put("productKey", "a1Q1Yrc****");
            schema.validate(object); // throws a ValidationException if this object is invalid
            //}
            System.out.println(System.currentTimeMillis() - start);
        }
        catch (ValidationException exception) {
            System.out.println(exception);
        }
    }

}
```

json\_schema的定义如下。

```
{
  "id": "http://json-schema.org/draft-04/schema#",
  "$schema": "http://json-schema.org/draft-04/schema#",
  "description": "Core schema meta-schema",
  "definitions": {
    "nameDefinition": {
      "type": "string",
      "pattern": "^[\\u4E00-\\u9FA5a-zA-Z0-9][\\u4E00-\\u9FA5a-zA-Z0-9_\\-\\(\\)\\uFF08\\uFF09\\u0020\\s\\.]{0,39}$"
    },
    "identifierDefinition": {
      "type": "string",
      "pattern": "[_a-zA-Z0-9]{1,50}"
    },
    "descriptionDefinition": {
      "type": "string",
      "pattern": ".{1,2048}"
    },
    "rwFlagDefinition": {
      "type": "string",
      "pattern": "(READ_WRITE|READ_ONLY|WRITE_ONLY)"
    },
    "callTypeDefinition": {
      "type": "string",
      "pattern": "(ASYNC|SYNC)"
    },
    "eventTypeDefinition": {
      "type": "string",
      "pattern": "(INFO_EVENT_TYPE|ALERT_EVENT_TYPE|ERROR_EVENT_TYPE)"
    },
    "requiredDefinition": {
      "type": "boolean"
    },
    "customDefinition": {
      "type": "boolean"
    },
    "directionDefinition": {
      "type": "string",
      "pattern": "(PARAM_INPUT|PARAM_OUTPUT)"
    },
    "argumentDefinition": {
      "required": [
        "identifier",
        "name",
        "dataType",
        "custom",
        "direction",
        "paraOrder"
      ],
      "properties": {
        "direction": {
          "$ref": "#/definitions/directionDefinition"
        },
        "paraOrder": {
          "type": "integer"
        },
        "identifier": {
          "$ref": "#/definitions/identifierDefinition"
        },
        "name": {
          "$ref": "#/definitions/nameDefinition"
        },
        "dataType": {
          "$ref": "#/definitions/dataTypeDefinition"
        },
        "description": {
          "$ref": "#/definitions/descriptionDefinition"
        },
        "custom": {
          "$ref": "#/definitions/customDefinition"
        },
        "dataSpecs": {
          "type": "object"
        },
        "dataSpecsList": {
          "type": "array"
        }
      }
    },
    "propertyDefinition": {
      "required": [
        "identifier",
        "name",
        "dataType",
        "productKey",
        "rwFlag",
        "custom",
        "required"
      ],
      "properties": {
        "identifier": {
          "$ref": "#/definitions/identifierDefinition"
        },
        "name": {
          "$ref": "#/definitions/nameDefinition"
        },
        "dataType": {
          "$ref": "#/definitions/dataTypeDefinition"
        },
        "rwFlag": {
          "$ref": "#/definitions/rwFlagDefinition"
        },
        "required": {
          "$ref": "#/definitions/requiredDefinition"
        },
        "description": {
          "$ref": "#/definitions/descriptionDefinition"
        },
        "custom": {
          "$ref": "#/definitions/customDefinition"
        },
        "dataSpecs": {
          "type": "object"
        },
        "dataSpecsList": {
          "type": "array"
        }
      }
    },
    "serviceDefinition": {
      "required": [
        "identifier",
        "serviceName",
        "productKey",
        "callType",
        "custom",
        "required"
      ],
      "properties": {
        "identifier": {
          "$ref": "#/definitions/identifierDefinition"
        },
        "serviceName": {
          "$ref": "#/definitions/nameDefinition"
        },
        "callType": {
          "$ref": "#/definitions/callTypeDefinition"
        },
        "required": {
          "$ref": "#/definitions/requiredDefinition"
        },
        "description": {
          "$ref": "#/definitions/descriptionDefinition"
        },
        "custom": {
          "$ref": "#/definitions/customDefinition"
        },
        "inputParams": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/argumentDefinition"
          }
        },
        "outputParams": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/argumentDefinition"
          }
        }
      }
    },
    "eventDefinition": {
      "required": [
        "identifier",
        "eventName",
        "eventType",
        "productKey",
        "custom",
        "required"
      ],
      "properties": {
        "identifier": {
          "$ref": "#/definitions/identifierDefinition"
        },
        "eventName": {
          "$ref": "#/definitions/nameDefinition"
        },
        "eventType": {
          "$ref": "#/definitions/eventTypeDefinition"
        },
        "required": {
          "$ref": "#/definitions/requiredDefinition"
        },
        "description": {
          "$ref": "#/definitions/descriptionDefinition"
        },
        "custom": {
          "$ref": "#/definitions/customDefinition"
        },
        "outputData": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/argumentDefinition"
          }
        }
      }
    },
    "dataTypeDefinition": {
      "enum": [
        "ARRAY",
        "STRUCT",
        "INT",
        "FLOAT",
        "DOUBLE",
        "TEXT",
        "DATE",
        "ENUM",
        "BOOL"
      ]
    },
    "dataSpecsDefinition": {
      "type": "object",
      "required": [
        "identifier",
        "isStd",
        "dataType",
        "childName",
        "name",
        "childDataType",
        "childSpecsDTO"
      ]
    }
  },
  "type": "object",
  "properties": {
    "productKey": {
      "type": "string"
    },
    "properties": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/propertyDefinition"
      }
    },
    "services": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/serviceDefinition"
      }
    },
    "events": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/eventDefinition"
      }
    }
  }
}
```

