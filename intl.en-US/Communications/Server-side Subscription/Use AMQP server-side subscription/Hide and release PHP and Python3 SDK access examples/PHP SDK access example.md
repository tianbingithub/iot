---
keyword: [IoT, Internet of Things, AMQP, server-side subscription to device message, SDK for PHP]
---

# PHP SDK access example

This article describes how to use SDK for PHP to connect a client to IoT Platform and receive messages from IoT Platform.

## Download SDK for PHP

The sample code is written based on the Stomp PHP library and allows you to connect a Stomp PHP client to IoT Platform over Simple Text Oriented Message Protocol \(STOMP\). To download a Stomp PHP client and view its instructions, visit [Stomp PHP](https://github.com/stomp-php/stomp-php).

## Sample code

For more information about the parameters in the following sample code, see [Connect an AMQP client to IoT Platform](/intl.en-US/Communications/Server-side Subscription/Use AMQP server-side subscription/Connect an AMQP client to IoT Platform.md).

```
<? php
require __DIR__ . '/vendor/autoload.php';
use Stomp\Client;
use Stomp\Network\Observer\Exception\HeartbeatException;
use Stomp\Network\Observer\ServerAliveObserver;
use Stomp\StatefulStomp;
function start_consume() {
    // For more information about the parameters, see Connect an AMQP client to IoT Platform.
    $accessKey = "${YourAccessKeyId}";
    $accessSecret = "${YourAccessKeySecret}";
    $consumerGroupId = "${YourConsumerGroupId}";
    // iotInstanceId: If you are using a purchased instance, you must specify the instance ID. If you are using a public instance, you can enter an empty string "".
    $iotInstanceId = "${YourIotInstanceId}";
    $timeStamp = round(microtime(true) * 1000);
    // The signature algorithm. Valid values: hmacmd5, hmacsha1, and hmacsha256.
    $signMethod = "hmacsha1";
    $clientId = "${YourClientId}";
    // For more information about how to configure the UserName, see the Connect an AMQP client to IoT Platform.
    // If you need to transmit messages in the binary format, you must specify encode=base64 in the userName parameter. Before IoT Platform sends these messages, it encodes these messages by using the Base64 algorithm. For more information, see the "Messages in the binary format" section.
    $userName = $clientId . "|authMode=aksign"
                . ",signMethod=" . $signMethod
                . ",timestamp=" . $timeStamp
                . ",authId=" . $accessKey
                . ",iotInstanceId=" . $iotInstanceId
                . ",consumerGroupId=" . $consumerGroupId
                . "|";
    $signContent = "authId=" . $accessKey . "&timestamp=" . $timeStamp;
    // Calculates a signature and specifies the signature for the password parameter. For more information, see Connect an AMQP client to IoT Platform.
    $password = base64_encode(hash_hmac("sha1", $signContent, $accessSecret, $raw_output = TRUE));
    // The endpoint. For more information, see Connect an AMQP client to IoT Platform. The port number is 61614.
    $client = new Client('ssl://${host}:61614');
    $sslContext = ['ssl' => ['verify_peer' => true, 'verify_peer_name' => false], ];
    $client->getConnection()->setContext($sslContext);

    // The listener that monitors the status of the connection between the client and IoT Platform.
    $observer = new ServerAliveObserver();
    $client->getConnection()->getObservers()->addObserver($observer);
    // The heartbeat setting. This setting enables IoT Platform to send a heartbeat packet every 10 seconds.
    $client->setHeartbeat(0, 10000);
    $client->setLogin($userName, $password);
    try {
        $client->connect();
    }
    catch(StompException $e) {
        echo "failed to connect to server, msg:" . $e->getMessage() , PHP_EOL;
    }
    //code works as usual
    $stomp = new StatefulStomp($client);
    $stomp->subscribe('/topic/#');
    return $stomp;
}

$stomp = start_consume();

while (true) {
    if ($stomp == null || ! $stomp->getClient()->isConnected()) {
        echo "connection not exists, will reconnect after 10s.", PHP_EOL;
        sleep(10);
        $stomp = start_consume();
    }

    try {
        // The business logic that processes messages.
        echo $stomp->read();
    }
    catch(HeartbeatException $e) {
        echo 'The server failed to send us heartbeats within the defined interval.', PHP_EOL;
        $stomp->getClient()->disconnect();
    } catch(Exception $e) {
        echo 'process message occurs error '. $e->getMessage() , PHP_EOL;
    }
}   
```

## Messages in the binary format

If you need to transmit messages in the binary format, you must use the Base64 algorithm to encode these messages because STOMP is a text-based protocol. Otherwise, messages may be truncated.

The following code shows how to specify encode=base64 in the userName parameter. This setting enables IoT Platform to sends messages after it encodes these messages by using the Base64 algorithm.

```
$userName = $clientId . "|authMode=aksign"
                . ",signMethod=" . $signMethod
                . ",timestamp=" . $timeStamp
                . ",authId=" . $accessKey
                . ",iotInstanceId=" . $iotInstanceId
                . ",consumerGroupId=" . $consumerGroupId
                . ",encode=base64" . "|";
```

