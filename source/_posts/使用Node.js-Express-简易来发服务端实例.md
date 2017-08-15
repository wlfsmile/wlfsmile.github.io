---
title: 使用Node.js+Express 简易开发服务端实例
date: 2017-08-14 14:48:29
tags: [node.js,express]
categories: Node
---
> 本文主要写的是用NodeJS+Express进行的一个服务端的建议开发

<img src="/img/2017-8-14/1.png" alt="1.png" align="center" />
<!-- more -->
本文主要摘自[使用 NodeJS+Express 开发服务端](http://www.jianshu.com/p/db4df1938eca)
github代码地址：[demo]()

#### 环境配置要求
1. 安装Node.js环境，具体方法不做细说，可参考[阮一峰的官方网站](http://www.runoob.com/nodejs/nodejs-install-setup.html)

2. 安装express(都为全局安装)，npm有时候太慢，可安装淘宝镜像[cnpm](https://npm.taobao.org/)

```
    npm install express -g
    npm install express-generator -g
```

3. 初始化项目

```
    cd 你的文件目录
    express 项目名称（我设为APIServer）
```
<img src="/img/2017-8-14/2.JPG" alt="express" align="center" />
得到的目录结构如下

<img src="/img/2017-8-14/3.JPG" alt="express" align="center" /> + /bin:用来启动应用（服务器）
+ /public: 存放静态资源目录
+ /routes：路由用于确定应用程序如何响应对特定端点的客户机请求，包含一个URI（或路径）和一个特定的 HTTP 请求方法（GET、POST等）。每个路由可以具有一个或多个处理程序函数，这些函数在路由匹配时执行
+ /views: 模板文件所在目录 文件格式为.jade
+ 目录app.js程序main文件 这个是服务器启动的入口

#### 启动服务器
在终端最后的位置输出了如下两个命令
```
    install dependencies:
     $ cd APIServer && npm install  //进入项目并安装环境

    run the app:
     $ DEBUG=apiserver:* npm start //启动服务器

```
+ 启动服务器

```
    npm start
```

<img src="/img/2017-8-14/4.JPG" alt="express" align="center" /> 
+在浏览器中访问[http://localhost:3000/](http://localhost:3000/)

<img src="/img/2017-8-14/1.png" align="center" />

#### 基本使用
+ app.js

```
    var express = require('express');
    var path = require('path');
    var favicon = require('serve-favicon');
    var logger = require('morgan');
    var cookieParser = require('cookie-parser');
    var bodyParser = require('body-parser');
    var app = express();
    ///=======路由信息 （接口地址）开始 存放在./routes目录下===========//

    var routes = require('./routes/index');//home page接口
    var users = require('./routes/users'); //用户接口

    app.use('/', routes); //在app中注册routes该接口 
    app.use('/users', users);//在app中注册users接口
    ///=======路由信息 （接口地址 介绍===========//

    ///=======模板 开始===========//
    // view engine setup
    app.set('views', path.join(__dirname, 'views'));
    app.set('view engine', 'jade');
    ///=======模板 结束===========//

```

+ index.js

```
    var express = require('express');
    var router = express.Router();


    //定义一个get请求 path为根目录
    /* GET home page. */
    router.get('/', function(req, res, next) {
        res.render('index', { title: 'Express' });
    });

    module.exports = router;

```

定义一个路由的基本格式为
```
    app.METHOD(PATH, HANDLER)
```

其中
+ app: express的实例
+ METHOD: HTTP 请求方法(get/post之类)。
+ PATH: 服务器上的路径。
+ HANDLER: 在路由匹配时执行的函数。

#### 简单实现一个获取用户信息接口
+ 创建一个user.js文件,/routes/user.js
+ 定义一个User模型

```
function User(){
    this.name;
    this.city;
    this.age;
}
module.exports = User;

```

+ 切换到users.js
头部添加

```
var URL = require('url'); //请求url模块
var User = require('./user'); //引入user.js
```
并继续添加
```
router.get('/getUserInfo',function(req,res,next){
    var user = new User();
    var params = URL.parse(req.url,true).query;

    if(params.id == '1'){
        user.name = "ligh";
        user.age = "1";
        user.city = "北京市";
    }else{
        user.name = "SPTING";
        user.age = "1";
       user.city = "杭州市";
    }

    var response = {status:1,data:user};
    res.send(JSON.stringify(response));
})
```
其中
```
获取url参数 依赖于url模块 使用前需要使用  require('url')
var params = URL.parse(req.url, true).query;
```
<img src="/img/2017-8-14/5.JPG" alt="express" align="center" />
由于users.js路由信息已经在app.js注册
停止服务器 重新start服务器即可直接访问

+ 调用方式
[http://localhost:3000/users/getUserInfo?id=1](http://localhost:3000/users/getUserInfo?id=1)
或者
[http://localhost:3000/users/getUserInfo?id=2](http://localhost:3000/users/getUserInfo?id=2)

<img src="/img/2017-8-14/6.JPG" alt="express" align="center" /> <img src="/img/2017-8-14/7.JPG" alt="express" align="center" />
注意我们访问的方式为users/getUserInfo?id=1 而不是基于根
原因是我们在app.js注册方式为app.use('/users', users);
我们可以利用这种方式 开发模块功能 比如 你有另外一个模块为msg
我们注册为：app.use('/msgs', msgs);
调用方式为
http://localhost:3000/msgs/getUserMsgs?id=1
