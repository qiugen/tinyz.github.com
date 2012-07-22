---
layout: post
title: Python 使用 MySQLdb 连接 mysql 及问题处理
category: 
- python
- mysql
tags:
- python
- mysqldb
- mysql
- TypeError
- UnicodeEncodeError
---

Python可以通过MySQLdb连接和操作mysql,在fedora中可以通过 sudo yum install MySQL-python 来安装，安装完成以后可以在python环境中 import MySQLdb 进行测试，安装和 import 都要注意大小写。
{% highlight bash %}
$:sudo yum search mysql|grep python
MySQL-python.x86_64 : An interface to MySQL
...(省略)
$:sudo yum install MySQL-python
{% endhighlight %}

####数据库连接

{% highlight python %}
#!/usr/bin/env python
#coding=utf-8
import MySQLdb
 
#建立和数据库的连接
conn = MySQLdb.connect(MYSQL_HOST, MYSQL_USER, MYSQL_PASS, MYSQL_DB, port=int(MYSQL_PORT))
#也可以通过制定参数进行连接,以省略一些参数或使用默认值
conn = MySQLdb.connect(host=MYSQL_HOST, port=MYSQL_PORT, passwd=MYSQL_PASS, db=MYSQL_DB)

#获取操作游标
cursor = conn.cursor()
#执行SQL创建数据库 
cursor.execute("""create database if not exists entries""") 
# 执行SQL查询
cursor.execute("""select title, text from entries order by id desc"""）
 
#关闭连接，释放资源
cursor.close()
{% endhighlight %}

####创建数据库，创建表，插入数据，插入多条数据
{% highlight python %}
#!/usr/bin/env python
#coding=utf-8
import MySQLdb
 
#建立和数据库系统的连接
conn = MySQLdb.connect(host=MYSQL_HOST, port=MYSQL_PORT, passwd=MYSQL_PASS)
 
#获取操作游标
cursor = conn.cursor()
#执行SQL,创建一个数据库. 
cursor.execute("""create database if not exists python""")
 
#选择数据库
conn.select_db('python');
#执行SQL,创建一个数据表.
cursor.execute("""create table test(id int, info varchar(100)) """)
 
value = [1,"inserted ?"];
#插入一条记录
cursor.execute("insert into test values(%s,%s)",value);
 
values=[]
#生成插入参数值
for i in range(20):
    values.append((i,'Hello mysqldb, I am recoder ' + str(i)))
#插入多条记录
cursor.executemany("""insert into test values(%s,%s) """,values);
 
#关闭连接，释放资源
cursor.close(); 
{% endhighlight %}

####查询
{% highlight python %}
#!/usr/bin/env python
#coding=utf-8
 
import MySQLdb
 
conn = MySQLdb.connect(host='localhost', user='root', passwd='longforfreedom',db='python')
cursor = conn.cursor()
count = cursor.execute('select * from test')
print '总共有 %s 条记录',count
 
#获取一条记录,每条记录做为一个元组返回
print "只获取一条记录:" 
result = cursor.fetchone();
print result
#print 'ID: %s info: %s' % (result[0],result[1])
print 'ID: %s info: %s' % result
 
#获取5条记录，注意由于之前执行有了fetchone()，所以游标已经指到第二条记录了，也就是从第二条开始的所有记录 
print "只获取5条记录:" 
results = cursor.fetchmany(5)
for r in results:
    print r
 
print "获取所有结果:" 
#重置游标位置，0,为偏移量，mode＝absolute | relative,默认为relative,
cursor.scroll(0,mode='absolute')
#获取所有结果
results = cursor.fetchall()
for r in results:
    print r
conn.close() 
{% endhighlight %}

####错误：Python TypeError: not all arguments converted during string formatting
当用python写insert语句时，出现以上错误：Python TypeError: not all arguments converted during string formatting
发生错误的原因在于格式化时placeholder的个数与实际参数的个数不匹配，需要检查参数的匹配问题。

####错误：UnicodeEncodeError: 'latin-1' codec can't encode characters in position 4-6: ordinal not in range(256)
应该是编码的错误（貌似MySQLdb默认连接数据库使用'latin-1'编码 ），解决：在连接mysql时指定编码：
{% highlight python %}
conn = MySQLdb.connect(MYSQL_HOST, MYSQL_USER, MYSQL_PASS,
                       MYSQL_DB, port=int(MYSQL_PORT), charset="utf8")
{% endhighlight %}




