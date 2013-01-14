---
layout: post
title: "Centos 6 安装中文支持、ipython 以及 virtualbox guestaddtion"
category: Linux
tags: [centos, python, virtualbox]
---

#### 安装中文支持（默认为英文）
1. yum -y groupinstall chinese-support
2. 修改/etc/sysconfig/i18n，将LANG修改为LANG="zh_CN.UTF-8"
3. 重启，生效。
 
#### 安装ipython
1. Download the latest epel-release rpm from http://download.fedora.redhat.com/pub/epel/6/i386/
2. Install epel-release rpm:

    \#: rpm -Uvh epel-release*rpm

3. Install ipython rpm package:

    \#: yum install ipython

#### 在虚拟机中的情况下安装GuestAddtion
需要安装linux kernel headers 以及 development tools，命令：

1. \#: yum -y install kernel-devel
2. \#: yum -y groupinstall "development tools" 
3. \#: yum -y update

#### 更改主机名（hostname）
uname -a 查看hostname

1. sudo hostname tiny ，让hostname立刻生效
2. vi /etc/hosts 修改原hostname为 tiny
3. vi  /etc/sysconfig/network 修改原hostname为 tiny,这样可以让reboot重启后也生效
4. reboot重启，uname -a 重新检查下。

