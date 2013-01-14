---
layout: post
title: "Python 中的生成器（generator） "
category: Python
tags: [python, generator]
---

生成器是python中一个非常酷的特性，python 2.2中引入后在2.3变成了标准的一部分。它能够让你在许多情况下以一种优雅而又更低内存消耗的方式简化无界（无限）序列相关的操作。
生成器是可以当做iterator使用的特殊函数，它功能的实现依赖于关键字yield，下面是它如何运作一个简单的演示(自己输入时注意缩进)：

{% highlight python %}

>>> def spam():
...   yield "first"
...   yield "second"
...   yield "third"
... 
>>> spam
<function spam at 0xb76d5ae4>
>>> for x in spam():
...  print x
... 
first
second
third

{% endhighlight %}

在函数spam（）内定义了一个生成器，但是对spam（）的调用永远只能获得一个单独的生成器对象，而不是执行函数里面的语句，这个对象（generator object）包含了函数的原始代码和函数调用的状态，这状态包括函数中变量值以及当前的执行点——函数在yield语句处暂停（suspended），返回当前的值并储存函数的调用状态，当需要下一个条目（item）时，可以再次调用next，从函数上次停止的状态继续执行，知道下一个yield语句。

**生成器和函数的主要区别在于函数 return a value，生成器 yield a value同时标记或记忆 point of the yield 以便于在下次调用时从标记点恢复执行。 yield 使函数转换成生成器，而生成器反过来又返回迭代器。**

有三种方式告诉循环生成器中没有更多的内容：  
1. 执行到函数的末尾（"fall off the end"）
2. 用一个return语句（它可能不会返回任何值）
3. 抛出StopIteration异常




