---
layout: post
title: "在 fedora 18 上安装 fcitx 输入法"
category: Linux
tags: [fedora, fcitx]
---
1. 安装 fcitx 输入法及配置工具: sudo yum install fcitx-pinyin fcitx-configtool
2. 配置以允许使用 iBus 之外的输入法：gsettings set org.gnome.settings-daemon.plugins.keyboard active false
3. 在终端使用 $ im-chooser 或者利用输入法配置工具选择 fcitx（安装输入法选择： sudo yum install im-chooser）
4. 在输入法配置工具中注销或者利用终端注销 $ gnome-session-quit
5. 重新登入，即可用 Ctrl+Space 切换

