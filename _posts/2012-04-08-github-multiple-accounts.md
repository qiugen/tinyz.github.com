---
layout: post
title: "Github 多账户使用"
category:
- Github
tags: 
- github
- ssh
---
github使用SSH与客户端连接。如果是单用户，生成密钥对后，将公钥保存至github，每次连接时SSH客户端发送本地私钥（默认~/.ssh/id_rsa）到服务端验证。单用户情况下，连接的服务器上保存的公钥和发送的私钥自然是配对的。但是如果是多用户在连接时，远程保存的是自己的公钥，但是SSH客户端依然发送默认私钥，那么这个验证自然无法通过。不过，要实现多帐号下的SSH key切换在客户端做一些配置即可。

#### 为不同账户生成 SSH key

{% highlight bash %}
cd ~/.ssh
ssh-keygen -t rsa -C 'first@mail.com'
ssh-keygen -t rsa -C 'second@mail.com'
{% endhighlight %}

分别输入不同的文件名，如：id_rsa_first, id_rsa_second, 注意路径的选择（一般情况下为 ～/.ssh/)

**注意**：github根据配置文件的user.email来获取github帐号显示author信息，所以对于多帐号用户一定要记得将user.email改为相应的email(second@mail.com)。

#### 增加配置文件
在~/.ssh目录创建config文件，该文件用于配置私钥对应的服务器。内容如下：

{% highlight bash %}
# Default github: first account (first@mail.com)
# use: default
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_first


# Second github: second account (second@mail.com)
# use: git remote add test git@secnod.github.com:second-user/test.git
Host second.github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_second
{% endhighlight %}

**注意**:非默认账户源应使用 git@secnod.github.com，默认账户使用：git@github.com

如果出现以下问题：

> Bad owner or permissions on /home/tiny/.ssh/config  
> fatal: The remote end hung up unexpectedly

运行：
{% highlight bash %}
sudo chmod 644 ～/.ssh/config 
{% endhighlight%}



#### Done

