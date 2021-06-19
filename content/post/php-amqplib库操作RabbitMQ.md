---
title: "php-amqplib库操作RabbitMQ"
date: 2021-05-27T11:11:00+08:00
lastmod: 2021-05-28T11:12:00+08:00
draft: false
tags: ["PHP","RabbitMQ"]
categories: ["技术"]

weight: 10
contentCopyright: MIT
mathjax: true
autoCollapseToc: true

---

## 1. composer安装php-amqplib库

`composer require php-amqplib/php-amqplib`

## 2. 生产者代码

> **send.php**

```php
<?php

require_once __DIR__.'/vendor/autoload.php';

use PhpAmqpLib\Connection\AMQPStreamConnection;
use PhpAmqpLib\Message\AMQPMessage;

$connection = new AMQPStreamConnection('172.17.0.2', 5672, 'admin', 'admin');
$channel = $connection->channel();

$channel->queue_declare('hello', false, false, false, false);

$msg = new AMQPMessage('hello world!');

$channel->basic_publish($msg, '', 'hello');

echo " [x] Sent 'Hello World!'\n";

$channel->close();
$connection->close();
```

## 3. 消费者代码

> **receive.php**

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

use PhpAmqpLib\Connection\AMQPStreamConnection;

$connection = new AMQPStreamConnection('172.17.0.2', 5672, 'admin', 'admin');
$channel = $connection->channel();

$channel->queue_declare('hello', false, false, false, false);

echo " [*] Waiting for messages. To exit press CTRL+C\n";

$callback = function($msg) {
    echo ' [x] Received ', $msg->body, "\n";
};

$channel->basic_consume('hello', '', false, true, false, false, $callback);

while($channel->is_open()) {
    $channel->wait();
}

$channel->close();
$connection->close();
```

