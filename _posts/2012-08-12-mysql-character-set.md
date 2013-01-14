---
layout: post
title: "MySQL创建数据库时指定编码很重要"
category: Database 
tags: [mysql, character, 编码]
---
在使用MySQL数据库的过程中，好几次发现在插入数据时产生的错误是由编码的错误而导致的，于是再次记下来以作提醒：**MySQL创建数据库时指定编码很重要**

+ 列出MYSQL支持的所有字符集：
	SHOW CHARACTER SET;

+ 当前MYSQL服务器字符集设置:
	SHOW VARIABLES LIKE 'character_set_%';

+ 当前MYSQL服务器字符集校验设置:
	SHOW VARIABLES LIKE 'collation_%';

+ 显示某数据库字符集设置:
	show create database database_name;

+ 显示某数据表字符集设置:
	show create table table_name;

+ 修改数据表字符集:
	alter table table_name default character set utf8;

+ 修改数据库字符集（但对已存在的表不影响）：
	alter database database_name character set utf8 collate utf8_general_ci;

+ 建库时指定字符集:
	create database database_name character set gbk collate gbk_chinese_ci;

+ 建表时指定字符集 
{% highlight sql %}
 CREATE TABLE `post` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(80) DEFAULT NULL,
  `body` text,
  `pub_date` datetime DEFAULT NULL,
  `category_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `category_id` (`category_id`),
  CONSTRAINT `post_ibfk_1` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=12 DEFAULT CHARSET=utf8;
{% endhighlight %}

####可以在配置文件my.cnf中更改默认字符集： 
	[client] 
	default-character-set=utf8 
	[mysqld] 
	default-character-set=utf8


MySQL创建 数据库时指定编码很重要，很多开发者都使用了默认编码，但是指定数据库的编码可以很大程度上避免导入导出带来的乱码问题。

**我们应该遵循的标准是：数据库，表，字段和页面或文本的编码要统一起来。**
