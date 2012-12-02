---
layout: post
title: "使用 Jekyll 在 Github 上发布静态站点"
category: 
- Jekyll
tags:
- github
- Jekyll
- blog
---
[Jekyll](http://jekyllrb.com/) 是一个静态站点生成器，它会根据网页源码生成静态文件。默认情况下，Github pages 会使用 Jekyll 编译上传的非以下划线开始的文件夹中的文件。发布站点时先在本地编写符合 Jekyll 规范的网站源码，然后上传到github，由github生成并托管整个网站。

#### 使用 Jekyll 发布使用静态博客具有以下的优点：

 * 使用 markdown 或者 textile 创建内容
 * 在任何地方使用 git 来管理
 * 可以利用终端来发布
 * 无需数据库
 * 省却了托管的麻烦


#### 如何安装
安装 Jekyll 的最好方法是使用 RubyGems 来安装，如果在安装过程中出现错误，记得安装 ruby-devel。具体步骤如下：
{% highlight bash %}
$ sudo yum install rubygems
$ sudo yum install ruby-devel
$ sudo gem install jekyll
$ sudo gem install rake
{% endhighlight %}

如果你想用 RDiscount 取代 Maruku 作为你的 Markdown 标记语言转换引擎，记得安装：
	$ sudo gem install rdiscount


使用时通过以下命令行参数执行 Jekyll：
	$ jekyll --rdiscount

或者直接在站点配置文件 _config.yml 中以默认使用 RDiscount：
	markdown: rdiscount


Jekyll 通过 [Pygments](http://pygments.org/) 支持超过百种语言的代码高亮，标记非常简单，基本就是 highlight+语言名称 这种格式。安装：
	$ sudo yum install python-pygments

#### 使用 Jekyll Bootstrap 来发布博客
[Jekyll-Bootstrap](http://jekyllbootstrap.com/) 提供了现成的 Jekyll blog 框架，而且支持更换主题。很适合初学者使用：

 * 在 Github 创建以名称为 USERNAME.github.com 的代码库（Repository）
 * 安装 Jekyll Bootstrap
{% highlight bash %}
$ git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
$ cd USERNAME.github.com
$ git remote set-url origin git@github.com:USERNAME/USERNAME.github.com.git
$ git push origin master
{% endhighlight %}
 * 发布后可以使用地址 http://USERNAME.github.com 来访问，初次发布需要等待几分钟

#### 本地环境运行
	$ cd USERNAME.github.com
	$ jekyll --server

#### rake命令支持：
{% highlight bash %}
rake page  # Create a new page.
rake post  # Begin a new post in ./_posts
rake preview  # Launch preview environment
rake theme:install  # Install theme
rake theme:package  # Package theme
rake theme:switch  # Switch between Jekyll-bootstrap themes.
{% endhighlight %}

