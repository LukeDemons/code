---
layout: post
title: 配置Xshell密钥登录Linux
date: 2015-08-04
category: "Linux"
---


免密码登陆


## 背景科普

ssh登录提供两种认证方式：**口令(密码)**认证方式和**密钥(公钥&私钥)**认证方式。

- 口令(密码)认证方式是我们最常用的一种，通过输入密码登录到Linux。

- 密钥认证方式需使用私钥登录到Linux。下面将详细介绍如何使用密钥登录。

## 大体思路

一、先通过Linux系统或Xshell工具本身生成密钥(公钥&私钥)。

二、将公钥放到服务器，写入 *~/.ssh/authorized_keys* 文件中。

三、配置客户端使用私钥登录。

## 动手实践

一、通过Linux系统命令生成密钥

    ssh-keygen -t rsa

![ssh-keygen](images/2015/ssh-keygen.png "ssh-keygen")<br>
二、将公钥上传至服务器

	cat toshiba.pub >> /root/.ssh/authorized_keys

三、配置Xshell使用私钥登录

##### 1、打开Xshell，点击"New"按钮，弹出"New Session Properties"对话框，在"Connection"栏目中，输入刚刚配置好公钥(Public Key)的IP地址和端口，如下图所示：

![new-session-properties](images/2015/new-session-properties.png "new-session-properties")<br>

##### 2、点击左侧的"Authentication",切换到认证栏目，在"Method"选择"Public Key"认证，用户名输入"root"(公钥是放在root目录下的.ssh文件夹中)，在"User Key"中选择刚才生成的私钥"toshiba","Passphrase"中输入私钥的加密密码。

![properties](images/2015/controller-properties.png "properties")<br>
配置完毕，点击File -> open 选择相应Session即可。