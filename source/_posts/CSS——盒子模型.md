---
title: CSS——盒子模型
date: 2017-10-23 21:48:15
tags: [CSS,盒子模型]
categories: [面试准备,CSS]
---
#### 个人理解
+ 对于任何元素，都是一个盒子模型

#### 标准盒子模型
<img src='/img/2017-10-23/box1.JPG' align="center" />
<!--more  -->
**element即content**
#### IE盒子模型
<img src='/img/2017-10-23/box2.JPG' align="center" />

+ 从上面可以看到，标准盒子模型和IE盒子模型都包含了content、padding、margin、border四个部分组成，不过相对于标准盒子模型，IE盒子模型的content包含了border和padding

#### 浏览器兼容
+ 代码顶部都要加doctype声明，这样大多数浏览器都会遵循标准w3c盒子模型
> IE5.x和6在怪异模式中使用自己的非标准模型，这些浏览器的width属性不是内容的宽度，而是内容、内边距、外边距的总格。  

> 虽然有方法解决这个问题。但是目前最好的解决方案是回避这个问题。也就是，不要给元素添加具有指定宽度的内边距，而是尝试将内边距或外边距添加到元素的父元素和子元素。

#### box-sizing属性介绍
```
    box-sizing:content-box|border-box|inherit
```
+ content-box:默认值。可使设置的宽度和高度应用戴元素的内容框，盒子的width只包含内容
    + 总宽度=margin+border+padding+width
+ border-box:设置的width是除了margin外的border+padding+content的总宽度，盒子的width包含border+padding+内容
    + 总宽度=margin+width
+ inherit：继承父元素的box-sizing属性值

#### 实际问题介绍
##### margin越界（第一个子元素的margin-top和最后一个子元素的margin-bottom的越界问题）
+ 当父元素没有边框border时，设置第一个子元素的margin-top值的时候，会出现margin-top值加在父元素上的现象
+ 解决方法有四个
    + 给父元素加边框（副作用）
    + 给父元素设定padding（副作用）
    + 给父元素设定overflow:hidden（副作用）
    + 父元素加前置内容生成
        ```
            .parent : before {
                content : " ";
                display : table;
            }
        ```
##### css 外边距合并（叠加）
> 两个上下方向相邻的元素框垂直相遇时，外边距会合并，合并后的外边距的高度等于两个发生合并的外边距中较高的那个边距值  
+ 注意： 只有普通文档流中块框的垂直外边距才会发生外边距合并。行内框、浮动框或绝对定位之间的外边距不会合并。
+ 方法：连个相邻元素最好只用margin：top/bottom