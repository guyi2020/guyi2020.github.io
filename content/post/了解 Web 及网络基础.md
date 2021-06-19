---
title: "了解 Web 及网络基础"
date: 2021-01-18T16:25:17+08:00
lastmod: 2021-01-19T19:01:05+08:00
draft: false
tags: ["HTTP"]
categories: ["技术"]

weight: 10
contentCopyright: MIT
mathjax: true
autoCollapseToc: true

---

## 1. TCP/IP协议分层

{{% admonition abstract 协议分层 %}}
- 应用层

- 传输层

- 网络层

- 数据链路层

  {{% /admonition %}}



{{% admonition  quote 提示  %}}
如果某些地方需要改动，只需要把变动的层修改逻辑。各层对外接口部分规划好，内部设计就比较灵活了。分层好处之一。
{{% /admonition %}}

### 应用层

{{% admonition info %}}

应用层决定了向用户提供应用服务时通信的活动。TCP/IP 协议族内预存了各类通用的应用服务。比如，FTP（File Transfer Protocol，文件传输协议）和 DNS（Domain Name System，域 名系统）服务就是其中两类。 HTTP 协议也处于该层。

{{% /admonition %}}

### 传输层

{{% admonition info %}}

传输层对上层应用层，提供处于网络连接中的两台计算机之间的数据传输。__TCP__（Transmission Control Protocol，传输控制协议）和 __UDP__（User Data Protocol，用户数据报 协议）。

{{% /admonition %}}

### 网络层

{{% admonition info %}}

网络层用来处理在网络上流动的数据包。数据包是网络传输的最小数据单位。与对方计算机之间通过多台计算机或网络设备进行传输时，网络层所起的作用就是在众多的选项内选择一条传输路线。

{{% /admonition %}}

### 链路层

{{% admonition info %}}

处理连接网络的硬件部分（网卡，光纤）。

{{% /admonition %}}



## 2. TCP/IP 通信传输流

{{% admonition info IP协议 %}}

发送端在层与层之间传输时，每经过一层必定会打上一个该层的所属的首部信息。

接收端同理，每经过一层时，会把对应的首部消去。

{{% /admonition %}}

### 

## 3. 与 HTTP 关系密切的协议 : IP、TCP 和 DNS

### 负责传输的 IP 协议
{{% admonition info IP协议 %}}


IP协议的作用就是把数据包传输给对方。需要通过IP地址和MAC地址才能确保准确送达到对方。

IP地址就是节点被分配的地址，MAC地址就是网卡的固定地址。IP地址和MAC地址进行配对。

使用ARP协议凭借MAC地址进行通信，**ARP**是一种解析地址的协议，根据通信方的IP反查对应的MAC地址。

{{% /admonition %}}

### 确保可靠性的 TCP 协议

{{% admonition info TCP协议 %}}

TCP协议位于传输层，提供可靠的字节流服务。为了更容易传输大数据，将数据分割以报文段为单位的数据包进行管理，能够确认数据最终是否送到对方。才用__三次握手__策略。

{{% /admonition %}}

### 负责域名解析的 DNS 服务

{{% admonition info DNS服务 %}}

DNS服务是和 HTTP 协议一样位于应用层的 协议。它提供域名到 IP 地址之间的解析服务。

{{% /admonition %}}



## 4. URI和URL

{{% admonition info %}}

与 URI（Uniform Resource Identifier ，统一资源标识符）相比，我们更熟悉 URL（Uniform Resource Locator，统一资源定位符）。URL正是使用 Web 浏览器等 访问 Web 页面时需要输入的网页地址。

{{% /admonition %}}

{{% admonition abstract URL   %}}

**Uniform**

规定统一的格式可方便处理多种不同类型的资源，而不用根据上下文 环境来识别资源指定的访问方式。另外，加入新增的协议方案（如 http: 或 ftp:）也更容易。

**Resource**

资源的定义是“可标识的任何东西”。除了文档文件、图像或服务（例 如当天的天气预报）等能够区别于其他类型的，全都可作为资源。另 外，资源不仅可以是单一的，也可以是多数的集合体。

**Identifier**

表示可标识的对象。也称为标识符。

{{% /admonition %}}

{{% admonition tips 提示   %}}

URI 用字符串标识某一互联网资源，而 URL表示资源的地点（互联 网上所处的位置）。可见 URL是 URI 的子集。

{{% /admonition %}}

### URI 格式

> http://user:pass@www.abc.com:80/dir/index.htm?id=1#ch1

{{% admonition info  http %}}

使用 http: 或 https: 等协议方案名获取访问资源时要指定协议类型。不区分字母大小写，最后附一个冒号（:）。 也可使用 data: 或 javascript: 这类指定数据或脚本程序的方案名。 

{{% /admonition %}}

{{% admonition info  登录信息%}}

登录信息（认证） 指定用户名和密码作为从服务器端获取资源时必要的登录信息（身份 认证）。此项是可选项。 

{{% /admonition %}}

{{% admonition info  服务器地址 %}}

服务器地址使用绝对 URI 必须指定待访问的服务器地址。地址可以是类似 abc.com 这种 DNS 可解析的名称，或是 192.168.1.1 这类 IPv4 地址 名，还可以是 [0:0:0:0:0:0:0:1] 这样用方括号括起来的 IPv6 地址名。

{{% /admonition %}} 

{{% admonition info  服务器端口号 %}}

服务器端口号指定服务器连接的网络端口号。此项也是可选项，若用户省略则自动 使用默认端口号。 

{{% /admonition %}} 

{{% admonition info  文件路径 %}}

带层次的文件路径 指定服务器上的文件路径来定位特指的资源。这与 UNIX 系统的文件 目录结构相似。

{{% /admonition %}} 

{{% admonition info  查询字符串  %}}

查询字符串针对已指定的文件路径内的资源，可以使用查询字符串传入任意参 数。此项可选。 

{{% /admonition %}} 

{{% admonition info 片段标识符   %}}

片段标识符使用片段标识符通常可标记出已获取资源中的子资源（文档内的某个 位置）。

该项也为可选 项。

{{% /admonition %}}