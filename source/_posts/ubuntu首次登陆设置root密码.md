---
title: ubuntu首次登陆设置root密码
top: true
categories: [Linux]
tags: [Linux, root密码]
toc: true
---
# 给root用户设置密码

打开终端，输入命令`sudo passwd`，输入当前用户的密码，然后系统会提示输入新密码` Enter new UNIX password:`并再次确认`Retype new UNIX password:`，此时的密码就是root新密码。修改成功后，输入命令su root，再输入新的密码就ok了