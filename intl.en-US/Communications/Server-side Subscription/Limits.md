---
keyword: [Internet of Things, IoT Platform, IoT, device messages, service subscription, AMQP, limits]
---

# Limits

This topic describes the limits of AMQP service subscription.

|Limit|Description|
|:----|:----------|
|Authentication timeout|An authentication request is sent immediately after a connection is established. If the authentication is not successful within 15 seconds, the server closes the connection.|
|Data timeout|When the server establishes a connection with IoT Platform, the heartbeat time \(the idle-timeout parameter in the AMQP\) is required. The value ranges from 30 to 300, in seconds. After the connection is established, the server must send ping packets within the heartbeat time to maintain the connection. If no ping packet is sent within the heartbeat time, IoT Platform closes the connection. |
|Policy for message pushing retries|Messages accumulated due to consumer offline or slow message consumption are re-sent again. The interval between push retries is one minute.|
|The number of saved messages|Up to 100 million messages can be accumulated for a consumer group.|
|Message retention period|One day.|
|Limits on the real-time message push rate|The maximum QPS value for a consumer group is 1000.|
|Limits on the offline message push rate|The maximum QPS value for a consumer group is 200.|
|The number of consumer groups with which a product can be associated|A product can be associated with up to 10 consumer groups.|
|The number of products with which a consumer group can be associated|A consumer group can be associated with up to 1,000 products.|
|Maximum number of consumer groups|An account can have up to 1,000 consumer groups.|
|Maximum number of consumers|A consumer group can have up to 64 consumers.|
|Connection limits|The maximum number of consumer requests in a consumer group is 100 per minute.|

