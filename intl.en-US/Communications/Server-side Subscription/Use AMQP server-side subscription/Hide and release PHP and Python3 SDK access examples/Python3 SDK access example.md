---
keyword: [IoT, Internet of Things, AMQP, server-side subscription to device message, Python 3, SDK for Python]
---

# Python3 SDK access example

This article describes how to use the SDK for Python3 to connect a client to IoT Platform and receive messages from IoT Platform.

## Development environment

Python 3.0 or later is used in this example.

## Download SDK for Python

The stomp.py library is used in this example. To download the library and view its instructions, visit [stomp.py](https://pypi.org/project/stomp.py/).

Install the stomp.py library. For more information, see [Installing Packages](https://packaging.python.org/tutorials/installing-packages/).

## Sample code

For more information about the parameters in the following sample code, see [Connect an AMQP client to IoT Platform](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Connect an AMQP client to IoT Platform.md).

```
# encoding=utf-8

import time
import sys
import hashlib
import hmac
import base64
import stomp
import ssl

def connect_and_subscribe(conn):
    # The parameters. For more information, see Connect an AMQP client to IoT Platform.
    accessKey = "${YourAccessKeyId}"
    accessSecret = "${YourAccessKeySecret}"
    consumerGroupId = "${YourConsumerGroupId}"
    # iotInstanceId: If you are using a purchased instance, you must specify the instance ID. If you are using a public instance, you can enter an empty string "".
    iotInstanceId = "${YourIotInstanceId}"
    clientId = "${YourClientId}"
    // The signature algorithm. Valid values: hmacmd5, hmacsha1, and hmacsha256.
    signMethod = "hmacsha1"
    timestamp = current_time_millis()
    // For more information about how to configure the UserName, see Connect an AMQP client to IoT Platform.
    userName = clientId + "|authMode=aksign" + ",signMethod=" + signMethod \
                    + ",timestamp=" + timestamp + ",authId=" + accessKey \
                    + ",iotInstanceId=" + iotInstanceId \
                    + ",consumerGroupId=" + consumerGroupId + "|"
    signContent = "authId=" + accessKey + "&timestamp=" + timestamp
    # Calculates a signature and specifies the signature for the password parameter. For more information, see Connect an AMQP client to IoT Platform.
    password = do_sign(accessSecret.encode("utf-8"), signContent.encode("utf-8"))
    
    conn.connect(userName, password, wait=True)
    conn.subscribe(destination='/topic/#', id=1, ack='auto')

class MyListener(stomp.ConnectionListener):
    def __init__(self, conn):
        self.conn = conn
    def on_error(self, headers, message):
        print('received an error "%s"' % message)
    def on_message(self, headers, message):
        print('received a message "%s"' % message)
    def on_disconnected(self):
        print('disconnected')
        connect_and_subscribe(self.conn)
    def on_heartbeat_timeout(self):
        print('on_heartbeat_timeout')
    def on_connected(self, headers, body):
        print("successfully connected")

def current_time_millis():
    return str(int(round(time.time() * 1000)))

def do_sign(secret, sign_content):
    m = hmac.new(secret, sign_content, digestmod=hashlib.sha1)
    return base64.b64encode(m.digest()).decode("utf-8")
# The endpoint. Do not prefix the endpoint with amqps://. For more information, see Connect an AMQP client to IoT Platform. The port number is 61614.
conn = stomp.Connection([('${YourHost}', 61614)])
conn.set_ssl(for_hosts=[('${YourHost}', 61614)], ssl_version=ssl.PROTOCOL_TLS)
conn.set_listener('', MyListener(conn)) 
connect_and_subscribe(conn)

time.sleep(100000)
conn.disconnect()
```

