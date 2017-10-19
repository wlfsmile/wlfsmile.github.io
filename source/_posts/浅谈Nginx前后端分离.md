---
title: 浅谈Nginx前后端分离
date: 2017-10-19 19:52:43
tags: [nginx,前后分离]
categories: nginx
---
<img src="/img/2017-10-19/1.jpg" align="center">
<!--more  -->

#### 什么是Nginx
> Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器。特点是占有内存小，并发能力强。

#### 正向代理和反向代理
+ 正向代理：一般默认为正向代理，用户访问不了一个资源，通过代理服务器去访问这个资源，将响应头待回给用户，用户知道自己访问的是其他服务器的资源，代理服务器不会掩饰URL
+ 反向代理：代理服务器是在中间层，但是用户不知道自己访问的资源是其他服务器的资源，代理服务器会掩饰URL

#### 使用Nginx反向代理tomcat
##### 启动Nginx
+ 直接点击Nginx.exe
+ 使用命令行，cd 到Nginx目录下，start nginx
<img src="/img/2017-10-19/2.jpg">