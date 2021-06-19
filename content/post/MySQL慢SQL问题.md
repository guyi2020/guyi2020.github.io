---
title: "MySQL慢SQL问题"
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

## 1. SQL 编写导致的慢 SQL 优化

- 字段类型转换导致不用索引，如字符串类型的不用引号，数字类型的用引号，这可能导致用不到索引扫描全表

- 不支持函数转换，字段前面不能加函数，否则使用不到索引

- 不要在字段前面加减运算

- 字符串比较长的可以考虑索引一部份减少索引文件大小，提高写入效率

- like % 在前面用不到索引

- 根据联合索引的第二个及以后的字段单独查询用不到索引

- 不要使用select \*，\* 是查询所有，效率慢

- 类似查询性别的情况，没必要加索引

- 排序尽量使用升序

- or 的查询尽量用 union 代替 （Innodb）

- 复合索引高选择性的字段排在前面

- order by / group by 字段包括在索引当中减少排序，效率会更高

- 在IN后面值的列表中，将出现最频繁的值放在最前面，出现得最少的放在最后面，减少判断的次数

  

## 2. 注意

1. 尽量规避大事务的 SQL，大事务的 SQL 会影响数据库的并发性能及主从同步
2. 分页语句 limit 的问题
3. 删除表所有记录请用 truncate，不要用 delete
4. 不让 mysql 干多余的事情，如计算
5. 输写 SQL 带字段，以防止后面表变更带来的问题，性能也是比较优的
6. 在 Innodb上用 select count(*)，因为 Innodb 会存储统计信息
7. 慎用 Oder by rand()



## 3. MySQL优化

