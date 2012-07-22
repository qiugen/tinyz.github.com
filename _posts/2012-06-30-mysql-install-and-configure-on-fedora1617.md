---
layout: post
title: Fedora16/17 Mysql 安装及配置
category: 
- Linux
tags:
- Mysql
- fedora
---
####1.安装 Mysql Server
	# yum install mysql mysql-server
####2.开启 MySQL server 及开机启动 MySQL
	# systemctl start mysqld.service  
	# systemctl enable mysqld.service
	ln -s '/usr/lib/systemd/system/mysqld.service' '/etc/systemd/system/multi-user.target.wants/mysqld.service'
	
	或者使用：
	# sudo chkconfig --levels 235 mysqld on
	注意：正在将请求转发到“systemctl enable mysqld.service”。

####3.MySQL Secure Installation
	# /usr/bin/mysql_secure_installation 
通过操作将进行以下各项过程

+ Set (Change) root password
+ Remove anonymous users
+ Disallow root login remotely
+ Remove test database and access to it
+ Reload privilege tables
output:
{% highlight bash %}
$ sudo /usr/bin/mysql_secure_installation 
[sudo] password for tiny: 




NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MySQL
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!


In order to log into MySQL to secure it, we'll need the current
password for the root user.  If you've just installed MySQL, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MySQL
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

Change the root password? [Y/n] n
 ... skipping.

By default, a MySQL installation has an anonymous user, allowing anyone
to log into MySQL without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MySQL comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...



All done!  If you've completed all of the above steps, your MySQL
installation should now be secure.

Thanks for using MySQL!

{% endhighlight %}

或者对于本地测试环境直接可以使用以下命令来初始化root密码：
	# mysqladmin -u root password [your_password_here]

####4.本地连接 Mysql
	$ mysql -u root -p
	$ mysql -h localhost -u root -p

####5.创建数据库，创建新用户及授予权限
{% highlight bash %}
## 创建数据库 flaskr ##
mysql> CREATE DATABASE flaskr;
 
## 创建用户 flaskr_user ##
mysql> CREATE USER 'flaskr_user'@'192.168.1.102' IDENTIFIED BY 'passwordxxx';
 
## 授予权限 ##
mysql> GRANT ALL ON flaskr.* TO 'flaskr_user'@'192.168.1.102';
 
##  FLUSH PRIVILEGES, reload the GRANT TABLES  ##
mysql> FLUSH PRIVILEGES;
{% endhighlight%}

