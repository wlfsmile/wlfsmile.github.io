---
title: hexo博客分支教训
date: 2017-08-16 13:17:43
tags: hexo
categories: hexo
---
<img src="/img/2017-8-16/1.png" align="center">

最近在写博客的时候突然想把自己的hexo分支完善一下，可是却不小心踩了坑。弄了好几个小时才弄好。最大的原因还是自己对分支管理掌握不够，搞的自己出现问题的时候狼狈不堪。
<!-- more -->
如果想要进行hexo博客分支备份，推荐[Hexo博客备份](http://www.jianshu.com/p/57b5a384f234)

#### 先说一下事情起因过程
+ 我发现当我切换到hexo分支时，本地的目录是master分支里面的东西，而我切换到master分支时，本地目录是hexo分支里面的东西（可能是我记错了，也可能是我一时疏忽大意，没管那么多，没仔细看）
+ 一不小心把master分支里面的东西传到了hexo分支
+ 然后把本地分支删除了
+ 后来索性把远程分支一起删了（就是这样，本地的东西也没了，还好我把博客的md文章全部备份了，不然哭死）
+ 发现仓库不能用了
后来我才知道，进行单独的分支管理，最好本地是有一个单独分支文件夹（反正我是喜欢这样）

#### 解决过程
+ 再创建一个hexo分支，将hexo设为默认分支
+ 把之前的博客文件夹弃用
+ 将github的东西克隆下来（会有博客的基本结构）

<img src="/img/2017-8-16/2.JPG" align="center">
+ 将之前保留的_config.yml，themes/，source/，scaffolds/，package.json，.gitignore复制过来
+ 执行```npm install```和```npm install hexo-deployer-git```（重要）（一开始我没执行hexo-deployer-git，然后执行hexo d 的时候就会有```ERROR Deployer not found: git```烦人错误）
+ 