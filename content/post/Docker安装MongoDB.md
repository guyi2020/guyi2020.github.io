---
title: "Docker安装MongoDB"
date: 2021-06-15T12:08:00+08:00
lastmod: 2021-06-15T12:08:00+08:00
draft: false
tags: ["MongoDB","Docker"]
categories: ["技术"]

weight: 10
contentCopyright: MIT
mathjax: true
autoCollapseToc: true

---

1. ## 下载镜像

   `docker pull mongo:latest`

2. ## 运行容器

   `docker run -itd --name mongo -p 27017:27017 mongo --auth`

3. ## 设置用户和密码

   `docker exec -it mongo mongo admin`

4. ## 创建一个名为 admin，密码为 123456 的用户

   `db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},"readWriteAnyDatabase"]});`

5. ## 尝试用创建用户进行连接

   `db.auth('admin', '123456');`

6. ## 条件查询单条记录

   `db.training.find({student_id: ObjectId('60c327b2e3541113bb456082'), ai_teach: '2'})`

7. ## 添加记录

   `db.runoob.insert({"name":"测试","age":18})`

   > **注意:** *在 MongoDB 中，集合只有在内容插入后才会创建! 就是说，创建集合(数据表)后要再插入一个文档(记录)，集合才会真正创建。*

8. ## 条件更新记录

   `db.runoob.update({'name':'测试'}, {$set: {'age': 26}})`

9. ## 条件删除记录

   `db.runoob.remove({'name':'测试'})`
