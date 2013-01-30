---
layout: post
title: "使用 Nginx 和 uwsgi 部署 MoinMoin"
category: MoinMoin
tags: [Nginx, uwsgi, MoinMoin]
---

####安装 Nginx

各发行版可以通过软件包管理应用来安装，编译安装解压源代码后在终端运行：

{% highlight bash %}
./configure
make
sudo make install
{% endhighlight %}

安装和编译选项见帮助或[文档]（http://wiki.nginx.org/InstallOptions）
 

####配置 Nginx
参见：http://wiki.nginx.org/MoinMoin

{% highlight bash %}
server_name wiki;
location /wiki {
   gzip off;
   include uwsgi_params;
   uwsgi_param SCRIPT_NAME /wiki;
   uwsgi_modifier1 30;
   uwsgi_pass unix:/path/to/uwsgi.socket;
 }
 location /wiki/moin_static196 {
   alias /path/to/MoinMoin/web/static/htdocs;
 }
{% endhighlight %}

 
####安装 MoinMoin

1. 下载源代码
2. 解压：$ tar xvfz moin-1.9.6.tar.gz
3. 安装：$ sudo python setup.py install

可以通过 python setup.py install --help 查看和安装相关的命令参数。如果想指定不同的数据（datafile）文件夹，可以通过 --install-data 来指定，如：--install-data = /var/www/moin；还可以通过 --prefix = PREFIX 指定不同的安装目录。

如果不安装直接使用或者安装时使用了 --prefix 指定了别的安装目录，即系统 python 路径下找不到 MoinMoin 相关文件的情况下，需要在 moin.wsgi 文件中指定相应的目录：sys.path.insert(0, 'PREFIX/lib/python2.3/site-packages')。如果运行 uwsgi 时配置文件不在当前或者系统路径中，也需要在 moin.wsgi 中指定（具体可参见 moin.wsgi， 里面做了注释和说明）。
 

####使用子路径

如果在服务器根路径下(/)运行 moinmoin，则不需要额外的设置，如果在子路径下运行（如： /wiki），则需要在 wikiconfig.py 中查找 url_prefix_static 并设置： url_prefix_static = '/wiki' + url_prefix_static

 

####MoinMoin 基本配置

 需要在 wikiconfig.py 文件中添加 superuser 、设定相应的权限和配置语言、主题等，关于权限相见[文档]（http://moinmo.in/HelpOnAccessControlLists#head-7a54879603c7bffec991b53c9381a45f658b2267）。

 

####运行与 uWsgi 命令

切换到 MoinMoin datafile 文件夹，运行：

uwsgi -s /tmp/moin.sock --wsgi-file wiki/server/moin.wsgi -M -p 4

关键在于指定 wsgi-file 的路径，关于 uwsgi 命令的具体参数可以运行 -h 来查看，以下列出了几个常用项：

-M：提供主进程

-P：指定进程或 worker 的数量

-t：超过时间后放弃

-limit-as：限制内存

-R：超过制定数量请求后重置

 

####升级与数据迁移

从 1.8 版本升级 1.9.X 步骤：

1. 正常安装 1.9.X 版本
2. 根据说明修改指定配置文件，如果原配置文件改动较少可直接在新配置文件中修改相应配置
3. 清除缓存:    # moin --config-dir=$(pwd) maint cleancache
4. 数据迁移：# moin  --config-dir=$(pwd) migration data

####权限相关
需要的权限主要有：
1. Nginx 读取 uwsgi.socket 和静态文档 htdocs 的权限，由于启动 Nginx 需要占用 80 端口
2. uWSGI 读取 wiki 目录的权限
