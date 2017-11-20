---
title: CSS——浮动及清除浮动
date: 2017-10-21 22:49:07
tags: [CSS,浮动]
categories: [面试准备,CSS]
---
#### 浮动
+ 浮动目的：最初是为了其他内容（如文本）“围绕”该图像，后来CSS允许浮动任何元素
+ 浮动产生bug的原因：当一个内层元素浮动时，如果没有**关闭浮动**，父元素就不会再包含这个浮动的内层元素了，因为此时**浮动元素已经脱离了文档流**，导致外层不能被撑开
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

#### 浮动的影响
+ 背景不能显示，边框不能撑开，margin、padding不能正确显示，如下代码
<img src='/img/2017-10-21/float4.JPG' align="center" />

+ 效果展示
<img src='/img/2017-10-21/float3.JPG' align="center" />

从上面效果可以看出，父级元素的背景颜色未被显示，并且父级元素高度塌陷（父元素不写高度时，子元素写了浮动后，父元素会发生高度塌陷），box的高度为0

#### 清除浮动（使用较多的方法）
##### overflow
+ 在其父元素设置{overflow:hidden}，就是在以上代码的box元素上加上这一行代码
+ 原理：因为这个属性相当于是让父级紧贴内容，即可紧贴其对象内内容
##### clear:both
+ 在浮动元素下方添加空div,并给该元素写css样式{clear:both;}
    + 问题：增加了HTML和CSS代码量
##### 父级元素也浮动
+ 让浮动元素的父级元素也浮动
+ 问题：每个元素都得浮动，容易出问题
##### 伪类清除方法（主流）
+ 将父级div（clearfix）定义如下代码
<img src='/img/2017-10-21/float5.JPG' align="center" />
+ 原理： 利用伪类在元素内插入两个元素块

##### 清除浮动后的效果
<img src='/img/2017-10-21/float6.JPG' align="center" />