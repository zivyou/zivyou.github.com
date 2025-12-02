---
title: weechat指南
date: 2019-10-08 14:00:36
tags: TECH
---

[TOC]
<!-- vim-markdown-toc GFM -->

* [常用命令 格式: /$cmd](#常用命令-格式-cmd)
* [设定](#设定)

<!-- vim-markdown-toc -->

## 常用命令 格式: /$cmd
1. /help $cmd: 查看命令的手册
2. /server add freenode8001 chat.freenode.net/8001  : 添加freenode:8001，并取名freenode8001
3. /connect freenode8001 username:mypassword: 使用用户名username连接到freenode:8001
4. /list -re open*: 查看以open开头的聊天室(支持正则)
5. /join #channel: 进入名为channel的channel
6. /close: close a server
7. /nick username: 使用昵称username
8. /msg NickServ identify ****: 输入用户名、密码，以空格相隔(相当于登录)
9. Ctrl+n: switch between buffer
10. Ctrl+x: switch between topic
11. /part "a message" : leave a channel
12. /Option+l: 纯显示聊天内容
13. Ctrl+r: 历史命令搜索
14. /window scroll 10/-10: buffer上下翻页


## 设定
1. /set irc.server_default.nicks "nick1, nick2" :设定默认的登录昵称
2. /set
3. 利用Alt+k来设定快捷键: /key bind (press Alt+k to get keys) (press your keys composite) /window scroll 1
