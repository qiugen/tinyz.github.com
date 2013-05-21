---
layout: post
title: "Unix时间戳与日期的相互转换"
category: Database
tags: [UNIX_TIMESTAMP,Date,MySQL]
---
Unix时间戳（Unix timestamp）在日常使用很广泛，它是一个无符号整数，表示了自 '1970-01-01 00:00:00' UTC(Epoch) 以来的秒数。在日常应用中常使用Unix时间戳来存储时间以便在不同的时区都能通过转化显示正确的时间，避免了许多不必要的麻烦。因此了解 Unix Timestamp 和 Timestamp 之间的转化很有必要。

### MySQL中的时间格式
+ DATE： 只有日期部分而没有时间，格式为 'YYYY-MM-DD' 。支持的范围为 '1000-01-01' 至 '9999-12-31'。
+ DATETIME：包含日期和时间两部分，格式为 'YYYY-MM-DD HH:MM:SS'。支持的范围为 '1000-01-01 00:00:00' 至 '9999-12-31 23:59:59'。
+ TIMESTAMP：包含日期和时间两部分， 范围为 '1970-01-01 00:00:01' UTC 至 '2038-01-19 03:14:07' UTC。

### 利用MySQL内置函数进行时间转化
可以MySQL内置的 UNIX_TIMESTAMP([date]) 和 FROM_UNIXTIME(unix_timestamp[,format]) 进行日期转换。

UNIX_TIMESTAMP()除了接受以上列出的时间格式外，还接受诸如 YYMMDD 和 YYYYMMDD 的时间格式。

FROM_UNIXTIME（）将  Unix timestamp 转换为当地时间。*值得注意的是：*由于涉及到当地时间，二者转换的结果随着时区的不同而不同，并不是一一对应的关系。以下为一些简单的例子：
{% highlight sql %}
mysql> select UNIX_TIMESTAMP();
+------------------+
| UNIX_TIMESTAMP() |
+------------------+
|       1362586112 |
+------------------+
1 row in set (0.00 sec)

mysql> select UNIX_TIMESTAMP('2013-01-01');
+------------------------------+
| UNIX_TIMESTAMP('2013-01-01') |
+------------------------------+
|                   1356969600 |
+------------------------------+
1 row in set (0.00 sec)

mysql> select FROM_UNIXTIME(0);
+---------------------+
| FROM_UNIXTIME(0)    |
+---------------------+
| 1970-01-01 08:00:00 |
+---------------------+
1 row in set (0.00 sec)

mysql> select FROM_UNIXTIME(1356969600);
+---------------------------+
| FROM_UNIXTIME(1356969600) |
+---------------------------+
| 2013-01-01 00:00:00       |
+---------------------------+
1 row in set (0.00 sec)

mysql> select FROM_UNIXTIME(1356969600,'%Y%m%d %H:%i:%S');
+---------------------------------------------+
| FROM_UNIXTIME(1356969600,'%Y%m%d %H:%i:%S') |
+---------------------------------------------+
| 20130101 00:00:00                           |
+---------------------------------------------+
1 row in set (0.00 sec)

{% endhighlight %}

### DATE_FORMAT() 函数用于以不同的格式显示日期/时间数据
语法为：DATE_FORMAT(date,format)，其中date 参数是合法的日期。format 规定日期/时间的输出格式。
{% highlight bash %}
格式	描述
%a	缩写星期名
%b	缩写月名
%c	月，数值
%D	带有英文前缀的月中的天
%d	月的天，数值(00-31)
%e	月的天，数值(0-31)
%f	微秒
%H	小时 (00-23)
%h	小时 (01-12)
%I	小时 (01-12)
%i	分钟，数值(00-59)
%j	年的天 (001-366)
%k	小时 (0-23)
%l	小时 (1-12)
%M	月名
%m	月，数值(00-12)
%p	AM 或 PM
%r	时间，12-小时（hh:mm:ss AM 或 PM）
%S	秒(00-59)
%s	秒(00-59)
%T	时间, 24-小时 (hh:mm:ss)
%U	周 (00-53) 星期日是一周的第一天
%u	周 (00-53) 星期一是一周的第一天
%V	周 (01-53) 星期日是一周的第一天，与 %X 使用
%v	周 (01-53) 星期一是一周的第一天，与 %x 使用
%W	星期名
%w	周的天 （0=星期日, 6=星期六）
%X	年，其中的星期日是周的第一天，4 位，与 %V 使用
%x	年，其中的星期一是周的第一天，4 位，与 %v 使用
%Y	年，4 位
%y	年，2 位
{% endhighlight %}

### 利用python进行日期转化
主要应用到 python time 模块中的 mktime([tuple]) 和 localtime（[seconds]）函数：二者是两个作用相反的函数，mktime() 将表示当地时间的 tuple 转化为 Unix时间戳，而 localtime()将以秒表示的Unix时间戳转化为当地时间。此外，gmtime()函数将Unix时间戳转化为格林尼治标准时间（GMT）。

{% highlight python %}
>>> t=(2013,1,1,0,0,0,0,0,0)
>>> d=time.mktime(t)
>>> d
1356969600.0
>>> time.localtime(d)
time.struct_time(tm_year=2013, tm_mon=1, tm_mday=1, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=1, tm_yday=1, tm_isdst=0)

>>> d1=datetime.datetime.strptime('2013-01-01','%Y-%m-%d')
>>> print time.mktime(d1.timetuple())
1356969600.0
>>> d1.timetuple()
time.struct_time(tm_year=2013, tm_mon=1, tm_mday=1, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=1, tm_yday=1, tm_isdst=-1)
>>> print time.gmtime(1356969600.0)
time.struct_time(tm_year=2012, tm_mon=12, tm_mday=31, tm_hour=16, tm_min=0, tm_sec=0, tm_wday=0, tm_yday=366, tm_isdst=0)

{% endhighlight %}

*更多可参考：*  
[MySQL文档](http://dev.mysql.com/doc/refman/5.6/en/date-and-time-functions.html)   
[python time](http://docs.python.org/2/library/time.html)
