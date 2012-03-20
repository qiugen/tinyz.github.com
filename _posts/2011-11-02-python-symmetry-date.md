---
layout: post
title: "世界完全对称日计算（python）"
category: Python
tags: 
- Python
- 对称日
---

看到有人写的用 c++ 写的世界完全对称日（如今天：2011 1102）的计算，我也来用 python 玩玩
主要思路是将年份数字反转作为月日对应的数字，使其满足完全对称的条件，然后判断此日期是否有效，如果有效则输出

代码如下：
{% highlight python %}
# -*- coding: utf-8 -*-
# Author: TinyZ
# Filename: symmetry_date.py
# Date  : Nov 2, 2011
 
import datetime,time
 
def strToDatetime(datestr,format='%Y%m%d'):
    try:
        return datetime.datetime.strptime(datestr, format)
    except ValueError:
        return False
 
def symmetry_date(year_start, year_end):
    for i in xrange(year_start,year_end+1):
        test=str(i)+str(i)[::-1]
        result=strToDatetime(test)
        if result:
            print result.strftime('%Y-%m-%d')
 
if __name__=='__main__':
    symmetry_date(2000,2099)
{% endhighlight%}

输出结果：

{% highlight bash %}
[tiny@tiny python]$ python symmetry_date.py
2001-10-02
2010-01-02
2011-11-02
2020-02-02
2021-12-02
2030-03-02
2040-04-02
2050-05-02
2060-06-02
2070-07-02
2080-08-02
2090-09-02
{% endhighlight%}
