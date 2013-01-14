---
layout: post
title: "Mysql忘记root密码情况的处理"
category: Database
tags: [mysql, 密码]
---
有一段时间没有写博客，在纸上、电脑上零零散散的记了好多多西，其中一些怕是都找不见了，还是要养成及时整理和记录的好习惯。某天拿一台闲置的旧服务器来用，结果mysql密码却忘记了，所以网上查了一下，记录如下：
停止mysql服务：
	#: /etc/init.d/mysql stop 
忽略权限检查启动mysql
	#: mysqld --skip-grant-tables&
无密码登入：
	$: mysql -u root
修改密码：
	> use mysql;
	> update user set Password=PASSWORD('newpasswd') where User='root';
刷新退出：
	> flush privileges;
	> quit
重启mysql：
	#: /etc/init.d/mysql start
如果是数据库是空的或者其中的数据不再需要，也可以直接删除数据库文件后重启，数据库将重新进行初始化。
数据库文件位置为： /var/lib/mysql
