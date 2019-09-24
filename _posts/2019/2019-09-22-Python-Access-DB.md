---
layout: post
title: Python Access DB
category: python
tags: [python,sqlite3]
---

# Python Access DB #

## sqlite3 ##

SQLite是一种嵌入式数据库，它的数据库就是一个文件。由于SQLite本身是C写的，而且体积很小，所以，经常被集成到各种应用程序中，甚至在iOS和Android的App中都可以集成。

```python 
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import os, sqlite3

db_file = os.path.join(os.path.dirname(__file__), 'test.db')
if os.path.isfile(db_file):
    os.remove(db_file)

# 初始数据:
conn = sqlite3.connect(db_file)
cursor = conn.cursor()
cursor.execute('create table user(id varchar(20) primary key, name varchar(20), score int)')
cursor.execute(r"insert into user values ('A-001', 'Adam', 95)")
cursor.execute(r"insert into user values ('A-002', 'Bart', 62)")
cursor.execute(r"insert into user values ('A-003', 'Lisa', 78)")
cursor.close()
conn.commit()
# conn.close()

cursor = conn.cursor()

cursor.execute('select * from user where name=\'Adam\'')

values = cursor.fetchall()

print(values)

cursor.close()
conn.commit()
conn.close()
```

可以参考廖老师的学习内容：
https://www.liaoxuefeng.com/wiki/1016959663602400/1017801751919456


## mysql ##

MySQL是Web世界中使用最广泛的数据库服务器。SQLite的特点是轻量级、可嵌入，但不能承受高并发访问，适合桌面和移动应用。而MySQL是为服务器端设计的数据库，能承受高并发访问，同时占用的内存也远远大于SQLite。

此外，MySQL内部有多种数据库引擎，最常用的引擎是支持数据库事务的InnoDB。

mysql 易学难懂，入门简单，精通难。（个人感觉）本人常年使用Oracle、MS sql ，对于这两个商业版的数据库，个人深爱不移，特别Oracle，安全稳定。无论是索引、函数、存储过程、执行计划的优化都优于mysql
但是mysql的优点就是免费，少数据量简单使用，大数据量配合一些内存组件，应用也非常简单。

### 代码 ###

```python 
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import mysql.connector

try:
    conn = mysql.connector.connect(user='root', password='root', database='python_test')

    cursor = conn.cursor()

    cursor.execute('create table user (id varchar(20) primary key, name varchar(20))')

    cursor.execute('insert into user (id,name) values (%s,%s)', ['1', 'Vincent'])

    print(cursor.rowcount)

    conn.commit()

    cursor.close();

    cursor = conn.cursor()

    cursor.execute('select * from user where id = %s', ['1'])

    values = cursor.fetchall()

    print(values)

finally:
    if cursor:
        cursor.close() 
    if conn:
        conn.close()

```

可以参考廖老师的学习内容：
https://www.liaoxuefeng.com/wiki/1016959663602400/1017802264972000