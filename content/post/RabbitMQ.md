---
title: "RabbitMQ消息队列"
date: 2021-05-25T13:08:00+08:00
lastmod: 2021-05-27T14:15:00+08:00
draft: false
tags: ["PHP","RabbitMQ"]
categories: ["技术"]

weight: 10
contentCopyright: MIT
mathjax: true
autoCollapseToc: true

---

## 1. RabbitMQ简介

消息队列（Message Queue）是一种应用间的通信方式，消息发送后可以立即返回。消息使用者再从MQ中取消息进行逻辑处理。对于消耗较大的请求，可以立马返回处理结果。减少服务器压力。为各个子系统之间解耦和异步处理。

## 2. RabbitMQ结构

![Local Picture](/blog/uploads/20210120/rabbitmq2.png "RabbitMQ")



RabbitMQ（消息队列）主要来确保消息的分发是，**Exchange**（交换机）、**RoutingKey**（路由）、**Queue**（队列）。从上面的图也可以看出来。 处理消息的接收、分发，主要在Broker模块中。

- **Exchange** 所有生产消息的入口都是到交换机这里。exchange通过进来的路由（RoutingKey），去和已binding的规则进行匹配，找到指定的队列。
- **RoutingKey** 我的理解，这里相当于一把钥匙。而binding的操作相当于一把锁头。
- **Queue** 消息的存放区域，等到消费者来取。
- **Binding Exchange**和**Queue**之间的一个绑定。

## 3. Docker安装RabbitMQ
`docker pull rabbitmq:management`

## 4. 创建容器并运行

> 15672是管理界面的端口，5672是服务的端口。这里顺便将管理系统的用户名和密码设置为admin admin

`docker run -dit --name Myrabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin -p 15672:15672 -p 5672:5672 rabbitmq:managemen`

## 5. 浏览器访问

![Local Picture](/blog/uploads/20210120/rabbitmq1.png "RabbitMQ")



## 6. 安装PHP扩展amqp扩展

> 注意：由于 docker 无法使用 vi，cmake，wget 等命令问题，则更新源 `apt-get update`

1. 安装amqp的依赖包，librabbitmq

   `apt-get install librabbitmq-dev`

2. 如果执行命令后报错

   `Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the   system variable OPENSSL_ROOT_DIR (missing: OPENSSL_CRYPTO_LIBRARY   OPENSSL_INCLUDE_DIR) (Required is at least version "0.9.8")`			

   则安装依赖包 `apt-get install libssl-dev`

3. 安装amqp扩展

   `pecl install amqp`

4. php.ini添加php扩展

   `extension = amqp.so`

5. 重启PHP，查看是否安装成功，打印phpinfo

   ![Local Picture](/blog/uploads/20210120/phpinfo1.png "phpinfo")

## 7. 代码

1. 生产者代码 producer.php

```PHP
<?php

try {

​    // 注意： host 是docker rabbitmq容器地址

​    // 可查容器ip地址命令： docker inspect --format='{{.NetworkSettings.IPAddress}}' Myrabbitmq 

​    // 定义mq连接信息

​    $amq = new AMQPConnection([

​        'host' => '172.17.0.2',

​        'vhost' => '/',

​        'port' => 5672,

​        'login' => 'admin',

​        'password' => 'admin'

​    ]);



​    // 连接mq

​    $amq->connect();

​    // 通过mq连接创建信道

​    $channel = new AMQPChannel($amq);

​    // 通过信道创建交换器对象

​    $exchange = new AMQPExchange($channel);

​    // 设置交换器名称

​    $exchange->setName('product_exchange');

​    // 设置交换器类型为direct

​    $exchange->setType(AMQP_EX_TYPE_DIRECT);

​    // 声明交换器

​    $exchange->declareExchange();

​    $data = [

​        'id' => 1,

​        'name' => 'hello world'

​    ];

​    $data = json_encode($data);

​    // 发布消息

​    $exchange->publish($data, 'self_define_routing_key');

} catch (\Exception $e) {

​    var_dump($e->getMessage());

}
```

2. 消费者代码 consume.php

```php
<?php

try {

​    // 交换器名称

​    $exchangeName = 'product_exchange';

​    // 队列名称

​    $queueName = 'createProduct';

​    // RouteKey

​    $routeKey = 'self_define_routing_key';

​    // 定义mq连接信息

​    $amq = new AMQPConnection([

​        'host' => '172.17.0.2',

​        'vhost' => '/',

​        'port' => 5672,

​        'login' => 'admin',

​        'password' => 'admin'

​    ]);

​    // 连接mq

​    $amq->connect();

​    // 通过mq连接创建信道

​    $channel = new AMQPChannel($amq);

​    // 通过信道创建交换器对象

​    $exchange = new AMQPExchange($channel);

​    // 设置交换器名称

​    $exchange->setName($exchangeName);

​    // 声明自己监听哪个队列

​    $queue = new AMQPQueue($channel);

​    $queue->setName($queueName);

​    $queue->declareQueue();

​    //绑定监听

​    $queue->bind($exchangeName,$routeKey);

​    $queue->consume(function ($envelope, $queue){

​          $data = $envelope->getBody();

​          var_dump($data);

​          //回应ack

​          $queue->ack($envelope->getDeliveryTag());

​    }); //阻塞监听

} catch (\Exception $e) {

​    var_dump($e->getMessage());

}
```



Please keep in mind that this and other tutorials are, well, tutorials. They demonstrate one new concept at a time and may intentionally oversimplify some things and leave out others. For example topics such as connection management, error handling, connection recovery, concurrency and metric collection are largely omitted for the sake of brevity. Such simplified code should not be considered production ready.
