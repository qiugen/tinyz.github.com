---
layout: post
title: "Python 测试之 doctest"
category:
- Python
tags:
- python
- doctest
---

Python 中的 doctest 模块为 python 程序提供了简便的测试方法，doctest 查找代码docstring 中类似 bash 等交互环境中的 python 执行会话（python session），然后将这些字符串作为输入和输出测试模块是否按照预期执行。doctest 主要用于以下三个方面:

1. 验证 docstring 是否为最新：执行交互例子看是否和文档中的描述一致
2. 回归测试（regression testing）
3. 撰写教程文档:通过一些典型的输入输出例子描述软件包的功能

** docstring 是否能够成为测试文档或仅仅是说明文档取决于其中的例子或注释是否被强调（emphasized），即具有和 python 交互环境中相同的格式 **

下面是 Python 文档中关于 doctest 的一个例子：

{% highlight python %}

def factorial(n):
    """Return the factorial of n, an exact integer >= 0.
    If the result is small enough to fit in an int, return an int.
    Else return a long.

    >>> [factorial(n) for n in range(6)]
    [1, 1, 2, 6, 24, 120]
    >>> [factorial(long(n)) for n in range(6)]
    [1, 1, 2, 6, 24, 120]
    >>> factorial(30)
    265252859812191058636308480000000L
    >>> factorial(30L)
    265252859812191058636308480000000L
    >>> factorial(-1)
    Traceback (most recent call last):
        ...
    ValueError: n must be >= 0

    Factorials of floats are OK, but the float must be an exact integer:
    >>> factorial(30.1)
    Traceback (most recent call last):
        ...
    ValueError: n must be exact integer
    >>> factorial(30.0)
    265252859812191058636308480000000L

    It must also not be ridiculously large:
    >>> factorial(1e100)
    Traceback (most recent call last):
        ...
    OverflowError: n too large
    """

    import math
    if not n >= 0:
        raise ValueError("n must be >= 0")
    if math.floor(n) != n:
        raise ValueError("n must be exact integer")
    if n+1 == n:  # catch a value like 1e300
        raise OverflowError("n too large")
    result = 1
    factor = 2
    while factor <= n:
        result *= factor
        factor += 1
    return result


if __name__ == "__main__":
    import doctest
    doctest.testmod()

{% endhighlight%}

#### 测试 docstring 中的例子
可以通过命令进行测试：python example.py 或者 python example.py -v  


#### 测试文本文件中的例子
