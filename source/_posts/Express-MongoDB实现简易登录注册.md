---
title: Express+MongoDB实现简易登录注册
date: 2017-08-17 22:14:58
tags: [express,MangoDB]
categories: Node
---
<img src="/img/2017-8-17/1.JPG" align="center" />

这是我在学习express过程中参照网上实例做的一个小demo，实现用express+mongodb的一个简单的登录注册，其中大部分参照[实例博客](http://blog.csdn.net/miss_ll/article/details/53927873)

[完整代码地址](https://github.com/wlfsmile/Node/tree/master/login)

<!-- more -->
#### 前期准备（了解）
+ Node.js+Express
+ MongoDB
+ ejs引擎模板
+ bootstrap(可选,也可直接查资料)

#### 基本思路
+ 一个默认页（'/'）
+ 登录页（'/login'）
+ 注册页（'/register'）
+ 登录完成之后的一个显示页（'/home'）
+ 注销的功能（'/logout'）

基本是默认页一进来，有登录和注册两个按钮
点击不同按钮到不同的页面，登录页面点击登录，若信息正确，跳入/home
/home中点击注销，跳出登录

#### 基本步骤
1. 初始化项目
```
express -e login
```

2. 安装配置环境
```
cd login
npm install
```

以上步骤跟我上一篇文章有些重复，有兴趣的可以看一下我的[上一篇博客](http://wlfsmile.win/2017/08/14/%E4%BD%BF%E7%94%A8Node.js-Express-%E7%AE%80%E6%98%93%E6%9D%A5%E5%8F%91%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%AE%9E%E4%BE%8B/#more)

3. 创建剩下的ejs（views目录下）
+ login.ejs
+ register.ejs
+ home.ejs

#### 代码
1. index.ejs(默认页)
```
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
    <p><a href="/login">登录</a></p>
    <p><a href="/register">注册</a></p>
  </body>
</html>

```

2. register.ejs
```
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <link rel="stylesheet" href="stylesheets/bootstrap.min.css">  
  <script type="text/javascript" src="javascripts/jquery-3.1.0.min.js"></script>  
  <script type="text/javascript" src="javascripts/bootstrap.min.js"></script> 
  <body>
    //基本样式结构
    //js操作
    <script type="text/javascript">
      $('#login1').click(function(){
        location.href = '/login';
      });

      $('#register1').click(function(){
        var username = $('#name').val();
        var password = $('#password').val();
        var passwordAgin = $('#passwordAgin').val();

        if(password !== passwordAgin){
          $("#password").css("boder","1px solid red");  
          $("#passwordAgin").css("boder","1px solid red");  
        }else{
          var data = {
            username : username,
            password : password
          };
          $.ajax({
            url : '/register',
            type : 'POST',
            data : data,
            success : function(data){
              if (data.status == 1) {
                location.href = '/login';
              }
            },
            error : function(){ 
              location.href = '/sregister';
            }
          });
        }
      });
    </script>
  </body>
</html>

```

3. login.ejs(登录页)
+ 与上述的注册页类似

4. 