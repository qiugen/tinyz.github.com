---
layout: post
title: "Gnome 中增加快捷方式"
category: 
-Linux
tags:
-linux
-gnome
-快捷方式
---
最近升级了 fedora 17 beta，也一直在虚拟机中玩 archlinux，或许有一天我会因为其不错的口碑而转向 archlinux。此外，无论是和 Gnome 2 还是 Unity 相比，我都很喜欢 Gnome 3，尤其是左上角的 hot corner 切换活动视图尤为喜欢（尽管好多人吐槽这个），让我偶尔忘我地在 window 下也往左上角挥一下鼠标，此外再就是清爽的颜色和简洁的界面了，其实从一开始接触使用 linux 时使用 ubuntu 就不是很喜欢它的配色，Unity 之后更是感觉它的 Dock 有些笨笨的。当然，这些都是萝卜白菜个人喜好的问题，作为用户我们都应该感谢开发者们为 linux、为改变这个世界作出的无私贡献，每每有程序更新，都离不开 geek 们背后的辛勤劳动。

最近，想使用思维导图软件 XMind 及一个所见所得编辑 Markdown 的叫 ReText 的小程序，但是都没有提供安装包，而是可以直接运行，于是将它们放到了 /opt 目录，需要的仅仅是在菜单中给它们增加相应的快捷方式：

#### GNOME 用 .desktop 文件来填充应用程序视图
全局程序视图位于 /usr/share/applications 目录，单个用户则位于 ~/.local/share/applications/ 目录。Nautilus 不把他们识别为纯文本文件，可以使用终端显示或编辑它们。

__注意__: 删除一个 .desktop 文件并不卸载软件，只是删除他的桌面特性（如文件关联，快捷键等）。

#### .desktop 配置文件的一些参数
Name: 程序快捷方式的名称  
Comment: 程序快捷方式的描述  
Exec: 程序可执行文件的路径  
Terminal: 程序执行的方式，true为执行在命令行中，falase则相反   
Type: 程序类型，默认为Application  
Categories: 程序在Application面板中所属的分类   
StartupNotify: 设置是否现实程序启动和关闭的提示，默认为true   
Icon: 程序图标的路径，如果只填写名字，那么gnome会在 /usr/share/icons 里面寻找这个图片

以增加 ReText 快捷方式为例：
{% highlight bash %}
[Desktop Entry]
Name=ReText
Comment= Markdown Text editor
Exec=/opt/ReText/retext.py
Icon=/opt/ReText/icons/retext.png
StartupNotify=true
Terminal=false
Type=Application
Categories=Development;
X-KDE-SubstituteUID=false
MimeType=text/plain;
{% endhighlight %}  

#### 如果想让程序在右键打开方式中可见，需要设置相应的 MimeType
如在上述例子中的 __MimeType=text/plain__

