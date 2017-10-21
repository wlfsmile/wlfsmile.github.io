---
title: 浅谈Nginx前后端分离
date: 2017-10-19 19:52:43
tags: [nginx,前后分离]
categories: nginx
---
<img src="/img/2017-10-19/1.jpg" align="center" />
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
<img src="/img/2017-10-19/2.JPG" align="center" />
+ 若成功，打开浏览器输入localhost，会到Nginx欢迎页面

#### 配置反向代理
+ 打开Nginx目录下的conf/nginx.conf文件，到35行左右的代码，主要修改location属性
<img src="/img/2017-10-19/3.JPG" align="center" />
+ listen:监听端口，用户访问Nginx服务器的端口
+ server_name：服务名，无影响
+ location：
> 义资源类型与服务器中资源地址url的映射关系，可在/后面定义资源类型，可设置多个location
其中proxy_pass代表要反向代理的服务器资源url，只要资源类型匹配，在这个url下的子路径资源都可以访问到，
其中root代表本地的资源路径，同样只要资源类型匹配，这个路径下的子目录资源都可以被访问到，
一个location中只能配置一个root或proxy_pass。
+ 修改文件保存后,cmd在Nginx目录下。使用```nginx -s reload```命令，重启Nginx，无报错即可

#### 测试结果
+ 在url输入请求，仍然是Nginx的端口。但是却是转到了tomcat的8080端口
<img src="/img/2017-10-19/4.JPG" align="center" />
<img src="/img/2017-10-19/5.JPG" align="center" />