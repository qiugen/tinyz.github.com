---
layout: post
title: "Linux 增加sudo用户"
category: linux
tags: [linux, sudo]
---

####在Linux中增加sudo用户的步骤（以增加拥有sudo权限的用户tiny为例）：
增加用户：
	sudo add tiny
设置密码：
	sudo passwd tiny 
加入admin用户组：
	sudo gpasswd -a tiny admin 
创建home目录：
	cd /home
	sudo mkdir tiny
编辑 /etc/suders: sudo visudo (或者直接用编辑器编辑sudoer文件)
在  root ALL = (ALL) ALL 下面加入：
	tiny ALL = (ALL) ALL
保存退出（ctrl+o， ctrl+x）
创建成功后，可以远程以当前的用户名和密码利用SFTP（SSH File Transfer Protocol ）方式连接用户的home目录，方面上传和下载文件，这在服务器管理方面尤为方便。
