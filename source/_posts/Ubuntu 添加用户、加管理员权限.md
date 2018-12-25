---
title: Ubuntu 添加用户、加管理员权限
date: 2016-05-23 20:34:18
tags: Ubuntu
---

# Ubuntu 添加用户、加管理员权限

Step1：添加新用户
useradd -r -m -s /bin/bash 用户名

Step2:配置新用户密码
passwd 用户名


Step3：给新添加的用户增加ROOT权限
chmod 777 /etc/sudoers
vim /etc/sudoers
然后添加：
用户名 ALL=(ALL:ALL) ALL

chmod 400 /etc/sudoers

另外，如果直接用useradd添加用户的话，可能出现没有home下的文件夹，以及shell无法自动补全的显现。出现此问题只要修改/etc/passwd下的/bin/sh为/bin/bash即可。