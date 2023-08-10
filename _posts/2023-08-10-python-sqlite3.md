---
layout: post
title: Python如何使用sqlite3？
categories: [Python]
description: 该文会比较详细的讲述Python如何使用自带的sqlite3模块。
keywords: Python, sqlite3, E-Notebook_SQLite
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

SQLite3是一种轻量级、嵌入式的关系型数据库管理系统。它是一个C语言库，提供了SQL数据库引擎。SQLite3拥有小巧、高性能、可靠、自包含以及无服务器设计等特点。

# [源码仓库E-Notebook_SQLite](https://github.com/Kim-Qian/E-Notebook_SQLite)

该程序实现了一个基于[sqlite3](https://www.sqlite.org/index.html)的记事本功能。

支持写入，读取，修改，删除，查找，排序文本等功能，并能按多种方式显示结果。

# [runoob](https://www.runoob.com/sqlite/sqlite-python.html)

## 连接数据库
下面的 Python 代码显示了如何连接到一个现有的数据库。如果数据库不存在，那么它就会被创建，最后将返回一个数据库对象。

```Python
#!/usr/bin/python

import sqlite3

conn = sqlite3.connect('test.db')

print ("数据库打开成功")
```
在这里，您也可以把数据库名称复制为特定的名称 :memory:，这样就会在 RAM 中创建一个数据库。现在，让我们来运行上面的程序，在当前目录中创建我们的数据库 test.db。您可以根据需要改变路径。保存上面代码到 sqlite.py 文件中，并按如下所示执行。如果数据库成功创建，那么会显示下面所示的消息：

    Open database successfully

## 创建表

下面的 Python 代码段将用于在先前创建的数据库中创建一个表：

```Python
#!/usr/bin/python

import sqlite3

conn = sqlite3.connect('test.db')
print ("数据库打开成功")
c = conn.cursor()
c.execute('''CREATE TABLE COMPANY
       (ID INT PRIMARY KEY     NOT NULL,
       NAME           TEXT    NOT NULL,
       AGE            INT     NOT NULL,
       ADDRESS        CHAR(50),
       SALARY         REAL);''')
print ("数据表创建成功")
conn.commit()
conn.close()
```

上述程序执行时，它会在 test.db 中创建 COMPANY 表，并显示下面所示的消息：

    数据库打开成功
    数据表创建成功

## INSERT 操作

下面的 Python 程序显示了如何在上面创建的 COMPANY 表中创建记录：

```Python
#!/usr/bin/python

import sqlite3

conn = sqlite3.connect('test.db')
c = conn.cursor()
print ("数据库打开成功")

c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (1, 'Paul', 32, 'California', 20000.00 )")

c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (2, 'Allen', 25, 'Texas', 15000.00 )")

c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (3, 'Teddy', 23, 'Norway', 20000.00 )")

c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00 )")

conn.commit()
print ("数据插入成功")
conn.close()
```

上述程序执行时，它会在 COMPANY 表中创建给定记录，并会显示以下两行：

    数据库打开成功
    数据插入成功

## SELECT 操作

下面的 Python 程序显示了如何从前面创建的 COMPANY 表中获取并显示记录：

```Python
#!/usr/bin/python
import sqlite3

conn = sqlite3.connect('test.db')
c = conn.cursor()
print ("数据库打开成功")

cursor = c.execute("SELECT id, name, address, salary  from COMPANY")
for row in cursor:
   print "ID = ", row[0]
   print "NAME = ", row[1]
   print "ADDRESS = ", row[2]
   print "SALARY = ", row[3], "\n"

print ("数据操作成功")
conn.close()
```
上述程序执行时，它会产生以下结果：

```
数据库打开成功
ID =  1
NAME =  Paul
ADDRESS =  California
SALARY =  20000.0

ID =  2
NAME =  Allen
ADDRESS =  Texas
SALARY =  15000.0

ID =  3
NAME =  Teddy
ADDRESS =  Norway
SALARY =  20000.0

ID =  4
NAME =  Mark
ADDRESS =  Rich-Mond
SALARY =  65000.0

数据操作成功
```

## UPDATE 操作

下面的 Python 代码显示了如何使用 UPDATE 语句来更新任何记录，然后从 COMPANY 表中获取并显示更新的记录：

```Python
#!/usr/bin/python

import sqlite3

conn = sqlite3.connect('test.db')
c = conn.cursor()
print ("数据库打开成功")

c.execute("UPDATE COMPANY set SALARY = 25000.00 where ID=1")
conn.commit()
print "Total number of rows updated :", conn.total_changes

cursor = conn.execute("SELECT id, name, address, salary  from COMPANY")
for row in cursor:
   print "ID = ", row[0]
   print "NAME = ", row[1]
   print "ADDRESS = ", row[2]
   print "SALARY = ", row[3], "\n"

print ("数据操作成功")
conn.close()
```

上述程序执行时，它会产生以下结果：

```
数据库打开成功
Total number of rows updated : 1
ID =  1
NAME =  Paul
ADDRESS =  California
SALARY =  25000.0

ID =  2
NAME =  Allen
ADDRESS =  Texas
SALARY =  15000.0

ID =  3
NAME =  Teddy
ADDRESS =  Norway
SALARY =  20000.0

ID =  4
NAME =  Mark
ADDRESS =  Rich-Mond
SALARY =  65000.0

数据操作成功
```

## DELETE 操作

下面的 Python 代码显示了如何使用 DELETE 语句删除任何记录，然后从 COMPANY 表中获取并显示剩余的记录：

```Python
#!/usr/bin/python

import sqlite3

conn = sqlite3.connect('test.db')
c = conn.cursor()
print ("数据库打开成功")

c.execute("DELETE from COMPANY where ID=2;")
conn.commit()
print "Total number of rows deleted :", conn.total_changes

cursor = conn.execute("SELECT id, name, address, salary  from COMPANY")
for row in cursor:
   print "ID = ", row[0]
   print "NAME = ", row[1]
   print "ADDRESS = ", row[2]
   print "SALARY = ", row[3], "\n"

print ("数据操作成功")
conn.close()
```

上述程序执行时，它会产生以下结果：

```
数据库打开成功
Total number of rows deleted : 1
ID =  1
NAME =  Paul
ADDRESS =  California
SALARY =  20000.0

ID =  3
NAME =  Teddy
ADDRESS =  Norway
SALARY =  20000.0

ID =  4
NAME =  Mark
ADDRESS =  Rich-Mond
SALARY =  65000.0

数据操作成功
```

# 进阶

## 改变SELECT的排序方式

```Python
cursor = c.execute("SELECT created, title, notes, note_group FROM Data ORDER BY created DESC")
cursor = c.execute("SELECT created, title, notes, note_group FROM Data ORDER BY created ASC")
```

即在FROM XXX后加上 ORDER BY DESC或ASC

## SELECT特定的数据，如：

```Python
"SELECT * FROM students WHERE age > ?"
```
就会查找students表中age栏目大于?的记录

## 若发生错误，则回滚事务

```Python
try:
   cursor.execute('INSERT INTO users (name, age) VALUES (?, ?)', ('Alice', 30))
   cursor.execute('INSERT INTO users (name, age) VALUES (?, ?)', ('Bob', 25))
   conn.commit()
except:
   conn.rollback()
```
- 请用try, except包围

# 温馨提示

- 创建表格时最好写上

      IF NOT EXISTS
      
- 修改数据库完后一定要close()前commit()提交更改
- update时不要忘记加where条件，否则会更新所有数据，例如在想要更新数据库中一位用户的权限为管理员时，如果没有加上where条件，那么所有用户的权限都会变为管理员
- 删除数据时不要忘记加where条件，否则会删除所有数据，例如在想要删除数据库中一位用户时，没有加上where条件，那么所有用户都会被删除
- 占位符要用 ? , 而不是 %s, 一定要注意(?, ?, ?)中的占位符要与待插入的数据个数一致
- 数据库操作完后一定要close()关闭数据库