---
title: "HTTP 报文信息"
date: 2021-01-20T14:24:30+08:00
lastmod: 2021-01-21T12:12:35+08:00
draft: false
tags: ["HTTP"]
categories: ["技术"]

weight: 10
contentCopyright: MIT
mathjax: true
autoCollapseToc: true

---

## 1. HTTP 报文

{{% admonition  info %}}
用于 HTTP 协议交互的信息被称为 HTTP 报文。请求端（客户端）的 HTTP 报文叫做请求报文，响应端（服务器端）的叫做响应报文。 HTTP 报文本身是由多行（用 CR+LF 作换行符）数据构成的字符串文本。
{{% /admonition %}}

{{% admonition info 报文首部 %}}

服务器端或客户端需要处理的请求或响应的内容以及属性。

{{% /admonition %}}

{{% admonition info 报文主体 %}}

应被发送的数据。

{{% /admonition %}}

>  两者由最初出现的 空行（CR+LF）来划分。通常，并不一定要有报文主体。

## 2. 报文结构

![Local Picture](/blog/uploads/20210120/20210121142852.png "报文结构")

## 3. 编码提升传输速率

> HTTP 在传输数据时可以按照数据原貌直接传输，但也可以在传输过 程中通过编码提升传输速率。通过在传输时编码，能有效地处理大量 的访问请求。但是，编码的操作需要计算机来完成，因此会消耗更多 的 CPU 等资源。

### 报文主体和实体主体的差异

{{% admonition info 报文 %}}

报文（message） 是 HTTP 通信中的基本单位，由 8 位组字节流（octet sequence， 其中 octet 为 8 个比特）组成，通过 HTTP 通信传输。 

{{% /admonition %}}

{{% admonition info 实体 %}}

实体（entity） 作为请求或响应的有效载荷数据（补充项）被传输，其内容由实 体首部和实体主体组成。

{{% /admonition %}}



 