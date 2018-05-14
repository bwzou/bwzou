---
layout: post
title: "访问数据库"
description: python也可以很方便的访问数据库，进行持久化操作。
date: 2018-05-15 00:00:00 +0800
categories: python
author: bwzou
---
MySQL是Web世界中使用最广泛的关系型数据库。首先因为其开源，也就是你可以免费使用，而DB2，Oracle，SQL Server等需要收费，没有特殊情况下一般也会首选MySQL。其次MySQL是为服务器端设计的数据库，能承受高并发访问，这样就可以适应很多场景。

本节我们主要学习如何用python访问MySQL。

## 安装MySQL
首先重MySQL官网下载最新[MySQL版本](https://dev.mysql.com/downloads/mysql/5.6.html)。MySQL是跨平台的，选择对应的平台下载安装文件，安装即可。

安装时，MySQL会提示输入root用户的口令，请务必记清楚。如果怕记不住，就把口令设置为root(这是你在访问数据库时的密码)。

在Windows上，安装时请选择UTF-8编码，以便正确地处理中文。

在Mac或Linux上，需要编辑MySQL的配置文件，把数据库默认的编码全部改为UTF-8。MySQL的配置文件默认存放在/etc/my.cnf或者/etc/mysql/my.cnf：
```
[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci
```
重启MySQL后，可以通过MySQL的客户端命令行检查编码：
```
$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor...
...

mysql> show variables like '%char%';
+--------------------------+--------------------------------------------------------+
| Variable_name            | Value                                                  |
+--------------------------+--------------------------------------------------------+
| character_set_client     | utf8                                                   |
| character_set_connection | utf8                                                   |
| character_set_database   | utf8                                                   |
| character_set_filesystem | binary                                                 |
| character_set_results    | utf8                                                   |
| character_set_server     | utf8                                                   |
| character_set_system     | utf8                                                   |
| character_sets_dir       | /usr/local/mysql-5.1.65-osx10.6-x86_64/share/charsets/ |
+--------------------------+--------------------------------------------------------+
8 rows in set (0.00 sec)
```
看到utf8字样就表示编码设置正确。

## 安装MySQL驱动
由于MySQL服务器以独立的进程运行，并通过网络对外服务，所以，需要支持Python的MySQL驱动来连接到MySQL服务器。有三种方式可以操作数据库：

- MySQLdb
- PyMySQL
- mysql.connector

但是这里我们只讲第二个即PyMysql，其他两个有兴趣可以自学。
##PyMySQL 
PyMySQL是Python中用于连接MySQL服务器的一个库，它遵循Python数据库API规范V2.0，并包含了pure-Python MySQL客户端库。

#### 安装PyMySQL
```python
pip install PyMysql
```
#### 使用PyMySQL
在链接数据库之前，先确保几件事情：1、MySQL数据库已经成功安装上。2、已经创建了数据库TESTDB。
```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

import pymysql

# 打开数据库连接
db = pymysql.connect("localhost", "root", "root", "testdb")

# 使用cursor()方法获取操作游标 
cursor = db.cursor()

# 使用execute方法执行SQL语句
cursor.execute("SELECT VERSION()")

# 使用 fetchone() 方法获取一条数据
data = cursor.fetchone()

print("Database version : %s " % data)

# 关闭数据库连接
db.close()
```
执行以上脚本输出结果如下：
```
Database version : 5.7.20-log 
```
我们来看一个完整的例子：
```python
import pymysql

def connectdb():
    print('连接到mysql服务器...')
    # 打开数据库连接
    # 用户名:root, 密码:root.,用户名和密码需要改成你自己的mysql用户名和密码，并且要创建数据库TESTDB，并在TESTDB数据库中创建好表Student
    db = pymysql.connect("localhost","root","root","testdb")
    print('连接上了!')
    return db

def createtable(db):
    # 使用cursor()方法获取操作游标
    cursor = db.cursor()

    # 如果存在表Sutdent先删除
    cursor.execute("DROP TABLE IF EXISTS Student")
    sql = """CREATE TABLE Student (
            ID CHAR(10) NOT NULL,
            Name CHAR(8),
            Grade INT )"""

    # 创建Sutdent表
    cursor.execute(sql)

def insertdb(db):
    # 使用cursor()方法获取操作游标
    cursor = db.cursor()

    # SQL 插入语句
    sql = """INSERT INTO Student
         VALUES ('001', 'CZQ', 70),
                ('002', 'LHQ', 80),
                ('003', 'MQ', 90),
                ('004', 'WH', 80),
                ('005', 'HP', 70),
                ('006', 'YF', 66),
                ('007', 'TEST', 100)"""

    try:
        # 执行sql语句
        cursor.execute(sql)
        # 提交到数据库执行
        db.commit()
    except:
        # 如果出现失败就进行回滚
        print('插入数据失败!')
        db.rollback()

def querydb(db):
    # 使用cursor()方法获取操作游标
    cursor = db.cursor()

    # SQL 查询语句
    #sql = "SELECT * FROM Student \
    #    WHERE Grade > '%d'" % (80)
    sql = "SELECT * FROM Student"
    try:
        # 执行SQL语句
        cursor.execute(sql)
        # 获取所有记录列表
        results = cursor.fetchall()
        for row in results:
            ID = row[0]
            Name = row[1]
            Grade = row[2]
            # 打印结果
            print("ID: %s, Name: %s, Grade: %d" % (ID, Name, Grade))
    except:
        print("Error: unable to fecth data")

def deletedb(db):
    # 使用cursor()方法获取操作游标
    cursor = db.cursor()

    # SQL 删除语句
    sql = "DELETE FROM Student WHERE Grade = '%d'" % (100)

    try:
       # 执行SQL语句
       cursor.execute(sql)
       # 提交修改
       db.commit()
    except:
        print('删除数据失败!')
        # 发生错误时回滚
        db.rollback()

def updatedb(db):
    # 使用cursor()方法获取操作游标
    cursor = db.cursor()

    # SQL 更新语句
    sql = "UPDATE Student SET Grade = Grade + 3 WHERE ID = '%s'" % ('003')

    try:
        # 执行SQL语句
        cursor.execute(sql)
        # 提交到数据库执行
        db.commit()
    except:
        print('更新数据失败!')
        # 发生错误时回滚
        db.rollback()

def closedb(db):
    db.close()

def main():
    db = connectdb()    # 连接MySQL数据库

    createtable(db)     # 创建表
    insertdb(db)        # 插入数据
    print('\n插入数据后:')
    querydb(db)
    deletedb(db)        # 删除数据
    print('\n删除数据后:')
    querydb(db)
    updatedb(db)        # 更新数据
    print('\n更新数据后:')
    querydb(db)

    closedb(db)         # 关闭数据库

if __name__ == '__main__':
    main()
```
要注意，在使用数据库之后记得关闭数据库。




