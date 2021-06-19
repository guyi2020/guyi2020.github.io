---
title: "MySQL优化"
date: 2021-04-23T13:08:00+08:00
lastmod: 2021-04-23T13:08:00+08:00
draft: false
tags: ["MySQL"]
categories: ["技术"]

weight: 10
contentCopyright: MIT
mathjax: true
autoCollapseToc: true
---

## 1. 选择优化的数据类型

- 更小的数据类型
  - 因为它们占用更少的磁盘，内存和CPU缓存，并且处理时需要的CPU周期更少。
  - 如果无法确认哪个数据类型好，就选择认为不会超过范围的最小类型。
- 简单就好
  - 需要更少的CPU周期。例如：整型比字符串操作代价更低，因为字符集和校对规则使字符比较比整型比较更加复杂。
- 尽量避免用NULL
  - NULL是列的默认属性。通常需要指定列为NOT NULL，除非是真的需要存储NULL值。可为NULL的列使得索引，索引统计和值比较都更加复杂。但是把NULL的列改为NOT NULL列所带来的性能也很小。尽量避免设置为可NULL的列。



## 2. 整数类型

- 有两种类型的数字:整数(whole number)和实数(real number)。如果存储整数，可以使用这几种整数类型: **TINYINT**，**SMALLINT**， **MEDIUMINT**，**INT**，**BIGINT**。分别使用8，16，24，32，64位存储空间。

- 可选属性**UNSIGNED**属性，表示不允许负值，大致可以使正数上限提高一倍。
- 有符号和无符号类型使用相同的存储空间，并有相同的性能。
- 整数类型宽度，如：INT(10)，大多数应用是没有意义的。对于存储和计算，INT(1) 和 INT(10)是相同的。

3. 关于INT(11)存储时间戳的优点如下：

   a. INT占4个字节，DATETIME占8个字节；

   b. INT存储索引的空间比DATETIME小，查询快，排序效率高；

   c. 在计算机时间差等范围问题，比较方便。
   
   

## 3. 字符串类型

### varchar

varchar类型用于存储可变长字符串，它比定长类型更节省空间（越短的字符串使用越少的空间）。

varchar需要使用1或2个额外字节记录字符串的长度。如果列的最大长度小于或等于255字节，则使用1个字节表示，否则使用2个字节。

> 注意：如果表使用了ROW_FORMAT=FIXED创建的话，每一行都会使用定长存储，这会浪费时间。

1. 创建表

`CREATE TABLE char_test(char_col char(10));`

2. 插入数据

`INSERT INTO char_test(char_col) VALUES ('string1'),(' string2'),('string3 ');`

3. 拼接查询

`SELECT CONCAT("'",char_col,"'") FROM char_test;`



### char

char类型是定长，适合存储很短的字符串，或者所有值接近同一个长度。例如存储密码MD5值。对于经常变更的数据，CHAR也比VARCHAR更好，因为定长的CHAR类型不用一产生碎片。对于非常短的列，更有效率。

注意：当存储char值时，MySQL会删除所有的末尾空格。
