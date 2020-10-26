# QueryDeviceBySQL

调用该接口通过类SQL语句快速搜索满足指定条件的设备。

## 限制说明

-   仅支持搜索华东2（上海）、华北2（北京）、华南1（深圳）地域您购买的实例中的设备。
-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** 子账号共享主账号配额。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=QueryDeviceBySQL&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|IotInstanceId|String|是|iot-cn-0pp1n8t\*\*\*\*|实例ID。 |
|SQL|String|是|SELECT \* FROM device where product\_key = "a1\*\*\*\*\*\*\*\*\*" limit 100, 20|查询设备的类SQL语句。具体要求和示例见下文请求参数补充说明。 |

使用QueryDeviceBySQL进行设备高级搜索时，类SQL语句由SELECT子句、WHERE子句、ORDER BY子句（可选）、LIMIT子句（可选）组成。长度限制400个字符。

示例：

`SELECT * FROM device WHERE product_key = "a1*********" order by active_time limit 0,10`

**SELECT子句**

`SELECT [field]/[count(*)] FROM device`

其中field为需要获取的字段，请参见下面的检索字段说明。也可填\*，获取所有字段。

如果需要获取count计数，则填count\(\*\)。

**WHERE子句**

`WHERE [condition1] AND [condition2]`

最多使用5个condition，且不支持嵌套，请参见下面的检索字段说明、运算符说明。

连接词支持AND、OR，最多使用5个连接词。

**ORDER BY子句（可选）**

ORDER BY子句用于实现自定义排序，可自定义排序的字段包括gmt\_create、gmt\_modified、active\_time。

该子句可不填，不填时随机排序。

**LIMIT子句（可选）**

LIMIT子句用于控制查询的偏移量，有两种用法：

-   limit k

    限制k <= 50，即单页最大为50。示例： `SELECT * FROM device WHERE product_key = "a1*****" limit 10`

-   limit n,k

    限制n + k <= 10000且k <= 50，即最大偏移量为10000且单页最大为50。示例： `SELECT * FROM device WHERE product_key = "a1*****" limit 40,10`


如果不填LIMIT子句，则默认为limit 20。

**检索字段说明**

|字段名

|类型

|说明 |
|-----|----|----|
|product\_key

|text

|设备所属产品ProductKey。 |
|iot\_id

|text

|设备标识符。默认返回iot\_id。 |
|name

|text

|设备名称。 |
|active\_time

|date

|设备激活时间。格式为yyyy-MM-dd HH:mm:ss.SSS，精确到毫秒。 |
|nickname

|text

|设备备注名称。 |
|gmt\_create

|date

|设备创建时间。格式为yyyy-MM-dd HH:mm:ss.SSS，精确到毫秒。 |
|gmt\_modified

|date

|设备信息最后一次更新时间。格式为yyyy-MM-dd HH:mm:ss.SSS，精确到毫秒。 |
|status

|text

|设备状态，取值：

 ONLINE：在线

 OFFLINE：离线

 UNACTIVE：未激活

 DISABLE：已禁用 |
|group.group\_id

|text

|设备分组ID。 |
|tag.tag\_name

|text

|设备标签名。 |
|tag.tag\_value

|text

|设备标签值。 |

**运算符说明**

|运算符

|支持的字段数值类型 |
|-----|-----------|
|=

|number、date、text |
|\>

|number、date |
|<

|number、date |
|LIKE

|text |

其中，LIKE支持前缀匹配，不支持后缀匹配或通配符匹配，且前缀不得少于4个字符，前缀固定以%结尾。示例：

`SELECT * FROM device where product_key = "a1*********" and name LIKE "test%" limit 10`

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。错误码详情，请参见[错误码](~~87387~~)。 |
|Data|Array of SimpleDeviceSearchInfo| |调用成功时，返回的设备信息。 |
|ActiveTime|String|Wed, 20-Feb-2019 02:16:09 GMT|设备激活时间，GMT格式。 |
|DeviceName|String|light|设备名称。 |
|GmtCreate|String|Wed, 20-Feb-2019 02:16:09 GMT|设备创建时间，GMT格式。 |
|GmtModified|String|Wed, 20-Feb-2019 02:16:09 GMT|设备信息最后一次更新时间，GMT格式。 |
|Groups|Array of SimpleDeviceGroupInfo| |设备分组信息。 |
|GroupId|String|a1d21d2fas|分组ID。 |
|IotId|String|Q7uOhVRdZRRlDnTLv\*\*\*\*00100|设备ID。物联网平台为该设备颁发的ID，设备的唯一标识符。 |
|Nickname|String|智能灯设备|设备的备注名称。 |
|ProductKey|String|a1BwAGV\*\*\*\*|设备所属产品ProductKey。 |
|Status|String|ONLINE|设备状态。取值：

 -   **ONLINE**：在线
-   **OFFLINE**：离线
-   **UNACTIVE**：未激活
-   **DISABLE**：已禁用 |
|Tags|Array of TagInfo| |设备标签信息。 |
|TagName|String|Color|标签名。 |
|TagValue|String|Red|标签值。 |
|ErrorMessage|String|系统异常|调用失败时返回的出错信息。 |
|RequestId|String|E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565|阿里云为该请求生成的唯一标识符。 |
|TotalCount|Long|100|当SELECT子句为`SELECT count(*) FROM device`时，返回的count计数。 |
|Success|Boolean|true|表示是否调用成功。

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceBySQL
&IotInstanceId=iot-cn-0pp1n8t****
&SQL=SELECT * FROM device where product_key = "a1*********" limit 100, 20
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<QueryDeviceBySQLResponse>
      <Data>
            <Status>OFFLINE</Status>
            <IotId>ii1*******</IotId>
            <GmtCreate>2020-04-04 16:38:17.000</GmtCreate>
            <ActiveTime>2020-04-04 16:38:18.607</ActiveTime>
            <GmtModified>2020-04-04 16:38:19.000</GmtModified>
            <ProductKey>a1*********</ProductKey>
            <DeviceName>testDevcieae7f3a</DeviceName>
      </Data>
      <Data>
            <Status>UNACTIVE</Status>
            <IotId>5wt*******</IotId>
            <GmtCreate>2020-04-04 16:37:32.000</GmtCreate>
            <Groups>
                  <GroupId>Ix4*******</GroupId>
            </Groups>
            <Groups>
                  <GroupId>Xrn*******</GroupId>
            </Groups>
            <Groups>
                  <GroupId>J9l*******</GroupId>
            </Groups>
            <GmtModified>2020-04-04 16:37:32.000</GmtModified>
            <ProductKey>a1*********</ProductKey>
            <DeviceName>testDevcie676a22</DeviceName>
      </Data>
      <RequestId>501CFABA-2C48-468D-B88C-3AA8E3B3A8F3</RequestId>
      <Success>true</Success>
</QueryDeviceBySQLResponse>
```

`JSON` 格式

```
{
  "RequestId": "501CFABA-2C48-468D-B88C-3AA8E3B3A8F3",
  "Data": [
    {
      "Status": "OFFLINE",
      "IotId": "ii1*******",
      "GmtCreate": "2020-04-04 16:38:17.000",
      "ActiveTime": "2020-04-04 16:38:18.607",
      "GmtModified": "2020-04-04 16:38:19.000",
      "ProductKey": "a1*********",
      "DeviceName": "testDevcieae7f3a"
    },
    {
      "Status": "UNACTIVE",
      "IotId": "5wt*******",
      "GmtCreate": "2020-04-04 16:37:32.000",
      "Groups": [
        {
          "GroupId": "Ix4*******"
        },
        {
          "GroupId": "Xrn*******"
        },
        {
          "GroupId": "J9l*******"
        }
      ],
      "GmtModified": "2020-04-04 16:37:32.000",
      "ProductKey": "a1*********",
      "DeviceName": "testDevcie676a22"
    }
  ],
  "Success": true
}
```

