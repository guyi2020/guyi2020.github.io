---
title: "简单的 HTTP 协议"
date: 2021-01-19T14:24:30+08:00
lastmod: 2021-01-20T20:12:35+08:00
draft: false
tags: ["HTTP"]
categories: ["技术"]

weight: 10
contentCopyright: MIT
mathjax: true
autoCollapseToc: true

---

## 1. HTTP 协议用于客户端和服务器端之间的通信

{{% admonition  info %}}
请求访问文本或图像等资源的一端称为客户端，而提供资源响应的一 端称为服务器端。
{{% /admonition %}}

### HTTP请求报文

```
POST /user HTTP/1.1      //请求行
Host: www.user.com
Content-Type: application/x-www-form-urlencoded
Connection: Keep-Alive
User-agent: Mozilla/5.0.      //以上是首部行
（此处必须有一空行）  //空行分割header和请求内容 
name=world   请求体
```

{{% admonition info POST %}}

起始行开头的**POST**表示请求访问服务器的类型，称为方法。

{{% /admonition %}}

{{% admonition info user%}}

随后的字符串 /user 指明了请求访问的资源对象， 也叫做请求 URI。

{{% /admonition %}}

{{% admonition info HTTP版本号 %}}

HTTP/1.1，即 HTTP 的版本 号，用来提示客户端使用的 HTTP 协议功能。

{{% /admonition %}}

>  请求报文是由请求方法、请求 URI、协议版本、可选的请求首部字段和内容实体构成的。



### HTTP返回报文

```
HTTP/1.1 200 OK 
Date: Tue, 10 Jul 2020 06:50:15 GMT 
Content-Length: 362
Content-Type: text/html

<html>
...
```

{{% admonition info HTTP版本号 %}}

HTTP/1.1，表示服务器对应的 HTTP 版本。

{{% /admonition %}}

{{% admonition info 状态码 %}}

200 OK 表示请求的处理结果的状态码（status code）和 原因短语（reason-phrase）。

{{% /admonition %}}

{{% admonition info 响应时间 %}}

下一行显示了创建响应的日期时间，是首部 字段（header field）内的一个属性。

{{% /admonition %}}

{{% admonition info 主体 %}}

接着以一空行分隔，之后的内容称为资源实体的主体（entity body）。

{{% /admonition %}}

> 响应报文基本上由协议版本、状态码（表示请求成功或失败的数字代 码）、用以解释状态码的原因短语、可选的响应首部字段以及实体主 体构成。

### 是不保存状态的协议

{{% admonition info 主体 %}}

使用 HTTP 协议，每当有新的请求发送时，就会有对应的新响应产 生。协议本身并不保留之前一切的请求或响应报文的信息。这是为了更快地处理大量事务，确保协议的可伸缩性，而特意把 HTTP 协议设计成如此简单的。HTTP/1.1 虽然是无状态协议，但为了实现期望的保持状态功能，于 是引入了 Cookie 技术。

{{% /admonition %}}

### HTTP 方法

{{% admonition info GET %}}

GET 方法用来请求访问已被 URI 识别的资源。指定的资源经服务器 端解析后返回响应内容。返回经过执行后的输出结果。

{{% /admonition %}}

{{% admonition info POST %}}

POST 方法用来传输实体的主体。返回 submit.cgi 接收数据的处理结果。

{{% /admonition %}}

{{% admonition info PUT %}}

PUT 方法用来传输文件。就像 FTP 协议的文件上传一样，要求在请 求报文的主体中包含文件内容，然后保存到请求 URI 指定的位置。响应返回状态码 204 No Content（比如 ：该 html 已存在于服务器上）。

{{% /admonition %}}

{{% admonition info HEAD %}}

HEAD 方法和 GET 方法一样，只是不返回报文主体部分。用于确认 URI 的有效性及资源更新的日期时间等。不返回报文主体。

{{% /admonition %}}

{{% admonition info DELETE %}}

DELETE 方法用来删除文件，是与 PUT 相反的方法。DELETE 方法按 请求 URI 删除指定的资源。

{{% /admonition %}}

{{% admonition info OPTIONS %}}

用来查询针对请求 URI 指定的资源支持的方法。返回服务器支持的方法。

{{% /admonition %}}

{{% admonition info TRACE %}}

让 Web 服务器端将之前的请求通信环回给客户端的方法。

{{% /admonition %}}

{{% admonition info CONNECT %}}

CONNECT 方法要求在与代理服务器通信时建立隧道，实现用隧道协 议进行 TCP 通信。主要使用 SSL（Secure Sockets Layer，安全套接 层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容 加 密后经网络隧道传输。

{{% /admonition %}}

### 持久连接

> HTTP/1.1 和一部分的 HTTP/1.0 想出了 持久连接（HTTP Persistent Connections，也称为 HTTP keep-alive 或 HTTP connection reuse）的方法。持久连接的特点是，只要任意一端 没有明确提出断开连接，则保持 TCP 连接状态。

{{% admonition tips 好处 %}}

持久连接的好处在于减少了 TCP 连接的重复建立和断开所造成的额 外开销，减轻了服务器端的负载。另外，减少开销的那部分时间，使 HTTP 请求和响应能够更早地结束，这样 Web 页面的显示速度也就相应提高了。

{{% /admonition %}}

### 管线化

> 持久连接使得多数请求以管线化（pipelining）方式发送成为可能。从 前发送请求后需等待并收到响应，才能发送下一个请求。管线化技术 出现后，不用等待响应亦可直接发送下一个请求。而管线化技术则比持久连接还 要快。请求数越多，时间差就越明显。

### 使用 Cookie 的状态管理

> 议这个特征的同时又要解决类似的矛盾问题，于是引入 了 Cookie 技术。Cookie 技术通过在请求和响应报文中写入 Cookie 信 息来控制客户端的状态。 Cookie 会根据从服务器端发送的响应报文内的一个叫做 Set-Cookie 的 首部字段信息，通知客户端保存 Cookie。当下次客户端再往该服务器 发送请求时，客户端会自动在请求报文中加入 Cookie 值后发送出 去。 服务器端发现客户端发送过来的 Cookie 后，会去检查究竟是从哪一 个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前 的状态信息。

 