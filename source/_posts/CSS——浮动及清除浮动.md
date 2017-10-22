---
title: CSS——浮动及清除浮动
date: 2017-10-21 22:49:07
tags: [CSS,浮动]
categories: CSS
---
#### 浮动
+ 浮动目的：最初是为了其他内容（如文本）“围绕”该图像，后来CSS允许浮动任何元素
<!-- more  -->
##### 浮动和绝对定位的区别
+ 如下代码：   

html
```
    <div class="box">
        <div class="left"></div>
        <div class="right">
            我只是想测试一下哈哈哈哈哈哈哈哈哈
        </div>
    </div>     

```
css
```
    .left{
        width: 200px;
        height: 300px;
        background-color: red;
        position: absolute; //绝对定位  浮动则换成float:left
    }
    .right{
        width: 500px;
        height: 400px;
        background-color: blue;
    }

```
+ 效果  

绝对定位

<img src='/img/2017-10-21/float1.JPG' align="center" />

浮动

<img src='/img/2017-10-21/float2.JPG' align="center" />

+ 绝对定位：完全脱离文本流，并且相对于其包含块定位，之后的元素会彻底占据之前元素位置，文本也会
+ 浮动：文本环绕浮动元素

#### 清除浮动
##### overflow
+ 在其父元素设置{overflow:hidden}
