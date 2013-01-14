---
layout: post
title: "Python十进制数学计算模块decimal"
category: Python
tags: [python, decimal]
---

####Python提供了decimal模块用于十进制数学计算，它具有以下特点

+ 提供十进制数据类型，并且存储为十进制数序列
+ 有界精度：用于存储数字的位数是固定的，可以通过decimal.getcontext（）.prec=x 来设定，不同的数字可以有不同的精度
+ 浮点：十进制小数点的位置不固定（但位数是固定的）

#### decimal的构建：
可以通过整数、字符串或者元组构建decimal.Decimal，对于浮点数需要先将其转换为字符串

#### decimal的context：
decimal在一个独立的context下工作，可以通过getcontext来获取当前环境。例如前面提到的可以通过decimal.getcontext().prec来设定小数点精度（默认为28）：
{% highlight bash %}
>>> from decimal import Decimal as D
>>> from decimal import getcontext  
>>> getcontext()
Context(prec=6, rounding=ROUND_HALF_EVEN, Emin=-999999999, Emax=999999999, capitals=1, flags=[Rounded, Inexact], traps=[DivisionByZero, InvalidOperation, Overflow])
>>> getcontext().prec = 6
>>> D(1)/D(3) 
Decimal('0.333333')
{% endhighlight%}

#### decimal和float性能对比：
{% highlight bash %}
$: python -mtimeit -s 'from decimal import Decimal as D' 'D("1.2")+D("3.4")'
$: python -mtimeit -s 'from decimal import Decimal as D' '1.2+3.4'
{% endhighlight%}
我在虚拟机中测试前者耗时是后者的1.7k倍，但这在某些运算（例如财务运算）中是值得的，但如果要对非整数做上百次的运算，应坚持使用float。


