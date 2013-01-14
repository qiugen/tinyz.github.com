---
layout: post
title: 测量Python代码运行的时间
category: Python
tags: [python, timeit]
---

Python 社区有句俗语： “python自己带着电池” ，别自己写计时框架。 Python 2.3 具备一个叫做 timeit 的完美计时工具可以测量python代码的运行时间。

#### timeit 模块

+ timeit 模块定义了接受两个参数的 Timer 类。两个参数都是字符串。 第一个参数是你要计时的语句或者函数。 传递给 Timer 的第二个参数是为第一个参数语句构建环境的导入语句。 从内部讲， timeit 构建起一个独立的虚拟环境， 手工地执行建立语句，然后手工地编译和执行被计时语句。
+ 一旦有了 Timer 对象，最简单的事就是调用 timeit()，它接受一个参数为每个测试中调用被计时语句的次数，默认为一百万次；返回所耗费的秒数。
+ Timer 对象的另一个主要方法是 repeat()， 它接受两个可选参数。 第一个参数是重复整个测试的次数，第二个参数是每个测试中调用被计时语句的次数。 两个参数都是可选的，它们的默认值分别是 3 和 1000000。 repeat() 方法返回以秒记录的每个测试循环的耗时列表。Python 有一个方便的 min 函数可以把输入的列表返回成最小值，如： min(t.repeat(3, 1000000))
你可以在命令行使用 timeit 模块来测试一个已存在的 Python 程序，而不需要修改代码。
+ 具体可参见文档： <http://docs.python.org/library/timeit.html>


**举例：**
{% highlight python %}

def test1():
    n=0
    for i in range(101):
        n+=i
    return n

def test2():
    return sum(range(101))

def test3():
    return sum(x for x in range(101))

if __name__=='__main__':
    from timeit import Timer
    t1=Timer("test1()","from __main__ import test1")
    t2=Timer("test2()","from __main__ import test2")
    t3=Timer("test3()","from __main__ import test3")
    print t1.timeit(1000000)
    print t2.timeit(1000000)
    print t3.timeit(1000000)
    print t1.repeat(3,1000000)
    print t2.repeat(3,1000000)
    print t3.repeat(3,1000000)
{% endhighlight %}

**执行结果：**

{% highlight bash %}
tiny@tiny-desktop:~/workspace/py$ python timetest.py 
7.99498915672
3.13702893257
10.6419789791
[8.2126381397247314, 8.6312708854675293, 8.6079621315002441]
[3.3426268100738525, 3.3914170265197754, 3.5281510353088379]
[11.097387075424194, 10.941920042037964, 10.874698877334595]
{% endhighlight %}


####利用time模块

* 利用time模块（仅作练习之用，不推荐）。 time.localtime(),  time.time(),  time.clock() 对比：
* time.localtime()，localtime返回的是struct_time，包含年月日，显然没有必要，更重要的是localtime()的精度依赖于time()
* time.time()，time返回的是UTC时间（seconds since the 00:00:00 UTC on January 1）。在很多系统，包括windows下精度很差，win32下的精度只有1/18.2秒。不过在Unix/Linux系统下，time()的精度还是很高的。
* Python的标准库手册推荐在任何系统下都尽量使用time.clock()。不过要注意是在win32系统下，这个函数返回的是真实时间（wall time），而在Unix/Linux下返回的是CPU时间。在win32下，这个函数的时间分辨率好于1微秒。

**举例：**
{% highlight python %}

def test():
    L=[]
    for i in range(100):
        L.append(i)

if __name__=='__main__':
    from time import clock
    start=clock()
    for i in range(1000000):
        test()
    finish=clock()
    print (finish-start)/1000000
{% endhighlight%}

**结果：**
{% highlight bash %}
1.749e-05
{% endhighlight%}

####其他方法

遇到复杂的程序，有很多性能分析工具可用。比如python的标准库里的profile可以统计程序里每一个函数的运行时间，并且提供了多样化的报表。（不了解，先记下来）

####参考：
* <http://docs.python.org/library/timeit.html>
* <http://woodpecker.org.cn/diveintopython/performance_tuning/timeit.html>
* <http://hi.baidu.com/shanyaodan0880/blog/item/b446617b7a98a5e42e73b3f2.html>
