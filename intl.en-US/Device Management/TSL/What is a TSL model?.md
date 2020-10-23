---
keyword: [Internet of Things, IoT, IoT Platform, TSL model, TSL, feature definition, property, event, service]
---

# What is a TSL model?

A Thing Specification Language \(TSL\) model digitizes a physical entity and builds a data model for the entity on IoT Platform. A TSL model is used to describe the features of an entity. This article introduces the concepts of a TSL model and describes how to use a TSL model.

A TSL model is a JSON-formatted file. A TSL model digitizes a physical entity, such as a sensor, vehicle-mounted device, building, and factory. A TSL model includes the properties, services, and events of an entity. A TSL model describes what the entity is, what the entity can do, and what information the entity can provide. After you define properties, services, and events for a product, the feature definition of the product is complete.

Product features are classified into the following types: property, service, and event.

|Feature type|Description|
|:-----------|:----------|
|Property|Defines a type of data that devices can collect, for example, the temperature data that is collected by an environmental sensor. Properties support the GET and SET request methods. Application systems can initiate requests to obtain and set properties.|
|Service|Defines a device capability or method that can be invoked by the required applications. You can set input and output parameters. Compared with properties, services can be invoked by using a command to implement complex business logic. For example, you can invoke a service to perform a specific task.|
|Event|Defines a device runtime event. Events contain notifications that require further actions or responses. You can set multiple output parameters. For example, an event may be a notification about the completion of a task, a system failure, or a temperature alert. You can subscribe to or publish events.|

## Format of a TSL model

On the Define Feature tab of the Product Details page, you can click **TSL Model** to view the TSL in the JSON format.

Structure of a TSL model:

**Note:** To fully demonstrate the structure of the TSL model, the following example includes all parameters. However, the parameters may vary in actual scenarios. The string that follows each parameter is the description of the parameter. For the application scenarios of each parameter, see the parameter description.

```
{
    "schema": "The URL that you can visit to view the TSL model.",
    "profile": {
        "productKey": "The key of the product.",
    },
    "properties": [
        {
            "identifier": "The ID of the property. The ID must be unique for a product.",
            "name": "The property name.",
            "accessMode": "The level of access to the property. Valid values: r and w, where r refers to read-only and rw refers to read/write.",
            "required": "Specifies whether this property is required for a standard feature.",
            "dataType": {
                "type": "The data type of the property. Valid values: int, float, double, text, date (UTC timestamp in the string format. Unit: ms), bool (0 or 1), enum (integers that are defined in the same way as the data of the bool type), struct (a struct that can include the previous seven data types. "specs": [{}] is used to describe the included objects), and array (an array that can include int, double, float, text, and struct data types).",
                "specs": {
                    "min": "The minimum value. This parameter is available only for properties of the int, float, and double data types.",
                    "max": "The maximum value. This parameter is available only for properties of the int, float, and double data types.",
                    "unit": "Optional. The unit of the property. This parameter is available only for the properties of the int, float, and double data types.",
                    "unitName": "Optional. The name of the unit. This parameter is available only for the properties of the int, float, and double data types.",
                    "size": "The number of elements in the array. Maximum value: 512. This parameter is available only for the properties of the array data type.",
                    "step": "The step size. This parameter is unavailable for the properties of the text or enum data type.",
                    "length": "The length of the data. Maximum value: 10240. This parameter is available only for the properties of the text data type.",
                    "0": "The value of 0. This parameter is available only for the properties of the bool data type.",
                    "1": "The value of 1. This parameter is available only for the properties of the bool data type.",
                    "item": {
                        "type": "The type of the array element. This parameter is available only for the properties of the array data type."
                    }
                }
            }
        }
    ],
    "events": [
        {
            "identifier": "The ID of the event. The ID must be unique for a product. The post event is pre-defined and used to report properties.",
            "name": "The name of the event.",
            "desc": "The description of the event.",
            "type": "The type of the event. Valid values: info, alert, and error.",
            "required": "Specifies whether this event is required for a standard feature.",
            "outputData": [
                {
                    "identifier": "The ID of the output parameter. The ID must be unique for a product.",
                    "name": "The name of the output parameter.",
                    "dataType": {
                        "type": "The data type of the output parameter. Valid values: int, float, double, text, date (UTC timestamp in the string format. Unit: ms), bool (0 or 1), enum (integers that are defined in the same way as the data of the bool type), struct (a struct that can include the previous seven data types. "specs": [{}] is used to describe the included objects), and array (an array that can include int, double, float, text, and struct data types).",
                        "specs": {
                            "min": "The minimum value. This parameter is available only for output parameters of the int, float, and double data types.",
                            "max": "The maximum value. This parameter is available only for output parameters of the int, float, and double data types.",
                            "unit": "Optional. The unit of the property. This parameter is available only for output parameters of the int, float, and double data types.",
                            "unitName": "Optional. The name of the unit. This parameter is available only for output parameters of the int, float, and double data types.",
                            "size": "The number of elements in the array. Maximum value: 512. This parameter is available only for output parameters of the array data type.",
                            "step": "The step size. This parameter is unavailable for output parameters of the text or enum data type.",
                            "length": "The length of the data. Maximum value: 10240. This parameter is available only for output parameters of the text data type.",
                            "0": "The value of 0. This parameter is available only for output parameters of the bool data type.",
                            "1": "The value of 1. This parameter is available only for output parameters of the bool data type.",
                            "item": {
                                "type": "The type of the array element. This parameter is available only for output parameters of the array data type."
                            }
                        }
                    }
                }
            ],
            "method": "The name of a method that can access the event. The name is generated based on the value of the identifier parameter."
        }
    ],
    "services": [
        {
            "identifier": "The ID of the service. The ID must be unique for a product. The set and get services are pre-defined based on the value of the accessMode parameter that is specified in the properties.",
            "name": "The name of the service.",
            "desc": "The description of the service.",
            "required": "Specifies whether this service is required for a standard feature.",
            "callType": "The call type. Valid values: async and sync, where async refers to an asynchronous call and sync refers to a synchronous call.",
            "inputData": [
                {
                    "identifier": "The ID of the input parameter. The ID must be unique for a product.",
                    "name": "The name of the input parameter.",
                    "dataType": {
                        "type": "The data type of the input parameter. Valid values: int, float, double, text, date (UTC timestamp in the string format. Unit: ms), bool (0 or 1), enum (integers that are defined in the same way as the data of the bool type), struct (a struct that can include the previous seven data types. "specs": [{}] is used to describe the included objects), and array (an array that can include int, double, float, text, and struct data types).",
                        "specs": {
                            "min": "The minimum value. This parameter is available only for input parameters of the int, float, and double data types.",
                            "max": "The maximum value. This parameter is available only for input parameters of the int, float, and double data types.",
                            "unit": "Optional. The unit of the property. This parameter is available only for input parameters of the int, float, and double data types.",
                            "unitName": "Optional. The name of the unit. This parameter is available only for input parameters of the int, float, and double data types.",
                            "size": "The number of elements in the array. Maximum value: 512. This parameter is available only for input parameters of the array data type.",
                            "step": "The step size. This parameter is unavailable for input parameters of the text or enum data type.",
                            "length": "The length of the data. Maximum value: 10240. This parameter is available only for input parameters of the text data type.",
                            "0": "The value of 0. This parameter is available only for input parameters of the bool data type.",
                            "1": "The value of 1. This parameter is available only for input parameters of the bool data type.",
                            "item": {
                                "type": "The type of the array element. This parameter is available only for input parameters of the array data type."
                                }
                            }
                        }
                    }
                }
            ],
            "outputData": [
                {
                    "identifier": "The ID of the output parameter.",
                    "name": "The name of the output parameter.",
                    "dataType": {
                        "type": "The data type of the output parameter. Valid values: int, float, double, text, date (UTC timestamp in the string format. Unit: ms), bool (0 or 1), enum (integers that are defined in the same way as the data of the bool type), struct (a struct that can include the previous seven data types. "specs": [{}] is used to describe the included objects), and array (an array that can include int, double, float, text, and struct data types).",
                        "specs": {
                            "min": "The minimum value. This parameter is available only for output parameters of the int, float, and double types.",
                            "max": "The maximum value. This parameter is available only for output parameters of the int, float, and double types.",
                            "unit": "Optional. The unit of the property. This parameter is available only for output parameters of the int, float, and double data types.",
                            "unitName": "Optional. The name of the unit. This parameter is available only for output parameters of the int, float, and double data types.",
                            "size": "The number of elements in the array. Maximum value: 512. This parameter is available only for output parameters of the array data type.",
                            "step": "The step size. This parameter is unavailable for output parameters of the text or enum data type.",
                            "length": "The length of the data. Maximum value: 10240. This parameter is available only for output parameters of the text data type.",
                            "0": "The value of 0. This parameter is available only for output parameters of the bool data type.",
                            "1": "The value of 1. This parameter is available only for output parameters of the bool data type.",
                            "item": {
                                "type": "The type of the array element. This parameter is available only for output parameters of the array data type."
                            }
                        }
                    }
                }
            ],
            "method": "The name of a method that can access the service. The name is generated based on the value of the identifier parameter."
        }
    ]
}
```

You can view the extended information of the TSL model if the following settings are configured: The node type of a product is set to gateway sub-device and the gateway connection protocol is set to Modbus, OPC UA, or custom.

For example, assume that the node type of a product is set to gateway sub-device and the gateway connection protocol is set to Modbus. The extended information of the TSL model can be used in Link IoT Edge for the sub-devices. For more information, see [Link IoT Edge documentation](/intl.en-US/User Guide/Connect devices to Link IoT Edge/Official drivers/Modbus drivers.md).

Structure of the extended information of the TSL model:

**Note:** To fully demonstrate the structure of the TSL model, the following example includes all parameters. However, the parameters may vary in actual scenarios. The string that follows each parameter is the description of the parameter. For the application scenarios of each parameter, see the parameter description.

```
{
  "profile": {
    productKey: "The key of the product.",
  },
  "properties": [
    {
      "identifier": "The ID of the property. The ID must be unique for the product.",
      "operateType": "The operation type. Valid values: coilStatus, inputStatus, holdingRegister, and inputRegister.",
      "registerAddress": "The register address.",
      "originalDataType": {
        "type": "The type of original data. Valid values: int16, uint16, int32, uint32, int64, uint64, float, double, string, and bool.",
        "specs": {
          "registerCount": "The number of registers. This parameter is available only for properties of the string data type.",
          "swap16": "Specifies whether to switch the high byte and low byte in the register. If so, the first and last 8 bits of the 16-bit integer in the register are swapped (byte1byte2 -> byte2byte1). This parameter is available for properties of all data types except for string and bool.",
          "reverseRegister": "Specifies whether to switch the high byte and low byte in the register. If so, the first and last 16 bits of the 32-bit integer in the register are swapped (byte1byte2byte3byte4 -> byte3byte4byte1byte2). This parameter is available for properties of all data types except for string and bool."
        }
      },
      "scaling": "The zoom factor. This parameter is available for properties of all data types except for string and bool.",
      "trigger": "The data report method. Valid values: 1 and 2, where 1 refers to report data at a specific time and 2 refers to report data changes."
    }
  ]
}

            
```

## Procedure

1.  Define features for a product in the [IoT Platform console](http://iot.console.aliyun.com/). For more information, see [Add a TSL feature](/intl.en-US/Device Management/TSL/Add a TSL feature.md) and [Batch add TSL features](/intl.en-US/Device Management/TSL/Batch add TSL features.md).
2.  Implement the TSL model of the product. For more information, see [Link SDK documentation](https://www.alibabacloud.com/help/product/93051.htm).
3.  Debug uplink and downlink connections. After the TSL model is implemented, the devices of the product can report properties and events to IoT Platform. IoT Platform can send commands to devices to set properties or call services. For more information, see [Device properties, events, and services](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device properties, events, and services.md). IoT Platform can call the [SetDeviceDesiredProperty](/intl.en-US/Developer Guide (Cloud)/API reference/Use TSL models/SetDeviceDesiredProperty.md) API operation to specify the expected value for a device property. For more information about how a device can obtain the expected value of a property, see [Desired device property values](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Desired device property values.md).

