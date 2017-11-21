---
title: react+express+mongodb搭建个人博客
date: 2017-11-21 19:34:43
tags: [react,node,express,mongodb]
---

这是本人用React+Express+mongodb搭建的一个简易博客系统，包括前端展示和后台管理界面。查看源码欢迎访问[我的github](https://github.com/wlfsmile/myBlog)

<!--more  -->
以下是参考我的源码后的操作

### 技术架构
#### 前端
+ 基础：HTML+CSS+JS+JQuery（使用的ajax交互，后期会考虑用fetch）
+ 框架：React+React-Router
+ 语法：ES6
+ 构建工具：Webpack
#### 后台
+ Node+Express搭建
#### 数据库 
+ MongoDB数据库

### 项目运行
#### 安装
+ 安装好node环境
+ 安装好mongodb
+ 可安装一个mongodb可视化工具（Robo 3T）
+ 把仓库克隆到本地
```
    git clone git@github.com:wlfsmile/myBlog.git
```
+ 安装配置环境
```
    cd myBlog
    npm i或者(cnpm,下同)
```
+ 全局安装webpack
```
    npm i -g webpack
```
+ 安装nodemon，让node自动重启
```
    npm install -g nodemon
```
#### 使用
+ 操作mongodb
    + 新建一个database，命名为blog
    + （可选）新建两个collection，为articles和comments，可自己先录入数据，也可以直接到后台管理界面去输入存入数据
+ 运行mongodb
```
    mongod --dbpath d:/mongodb/data(这是你mongodb的安装路径，我是装在d盘根目录下，所以路径为这个)
```
+ webpack编译打包,使用--watch可以让webpack自动重新构建
```
    webpack --watch
```
+ 运行服务器
```
    nodemon app.js
```
#### 访问
在浏览器的url栏中访问```localhost:8000```即可

### 目录结构
![目录结构](https://github.com/wlfsmile/myBlog/blob/master/images/tree.png)

<!-- <img src="https://github.com/wlfsmile/myBlog/blob/master/images/tree.png" align="center" /> -->

+ client/static: 所有静态页资源
    + be(fe): 后台管理(前端)展示页面 
        + assets：页面所有的静态资源（css/images之类）
        + component：react组件
        + views：后台管理（前端）react入口文件
        + index.html：react的根页面
    + build：webpack编译构建生成的文件
    + images：webpack生成的图片
    + views：error文件
+ server：后台文件夹
    + dbbase：数据文件
    + routes：所有路由
    + .babelrc：es6转码使用文件
    + app.js：node入口文件
    + package.json：配置环境文件
    + webpack.config.js：webpack配置文件

### 项目功能(持续更新)
#### 前端展示
+ 首页
+ 博客列表页
+ 文章详情页
+ 评论
+ about页
#### 后台管理
+ 新建文章页（实现提交markdown格式）
+ 更新/删除文章
+ 编辑about页