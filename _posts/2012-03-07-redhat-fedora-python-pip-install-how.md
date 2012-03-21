---
layout: post
title: "RedHat/Fedora Linux 上使用 yum 安装 python pip 模块"
category:
- Linux
tags:
- redhat
- fedora
- pip
- python
---

pip是一个可以替代 easy_install 的安装和管理 python 软件包的工具，具体可以安装的 python 包可以在这里查看 Python Package Index。

在 fedora 下提供了 python-pip 包用于安装 pip，和其他系统不同的是用 pip-python 命令来运行的：

{% highlight bash %}
tiny@i ~$ yum search python-pip

======================= N/S Matched: python-pip ========================
python-pip.noarch : Pip installs packages.  Python packages.  An
                  : easy_install replacement
tiny@i ~$ sudo yum install python-pip
　　
{% endhighlight %}
 
但是运行是却找不到 pip 命令，应该是考虑到和 pip 相同运行命令的包存在，为了避免冲突而使用了 pip-python：

{% highlight bash %}
tiny@i ~$ which pip
 /usr/bin/which: no pip in (/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/tiny/.local/bin:/home/tiny/bin)
tiny@i ~$ rpm -ql python-pip
 /usr/bin/pip-python
 ...(省略若干行)

tiny@i ~$ which pip-python
 /usr/bin/pip-python
　　
{% endhighlight %}
 
但是，在我们使用不到传说中这个正牌 pip 的情况下，可以通过 pip 再次安装 pip（递归？），到时候就可以直接使用 pip 命令了：

{% highlight bash %}

tiny@i ~$ sudo pip-python install -U pip

tiny@i ~$ sudo pip install virtualenv
[sudo] password for tiny: 
Requirement already satisfied (use --upgrade to upgrade): virtualenv in /usr/lib/python2.7/site-packages
Cleaning up... 

{% endhighlight %}

#### Done!
