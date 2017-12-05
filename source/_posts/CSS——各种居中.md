---
title: CSS——各种居中
date: 2017-12-05 20:39:26
tags: [CSS,居中]
categories: [面试准备,CSS]
---
##### 水平居中
###### 行内元素（inline）
<!-- more -->
```
    .parent{ //父级元素为block
        text-align: center;
    }
```
###### 块状元素（block）
```
    .child{
        margin: 0 auto;
    }
```
###### 多块状元素
```
    .parent{
        text-align: center;
    }
    .divs{
        display: inline-block;
        width: 100px;
        height: 50px;
    }

    //flex布局
    .parent{
        display: flex;
        justify-content: center;
    }
    .divs{
        width: 100px;
        height: 50px;
    }
```
##### 垂直居中
###### 行内元素
```
    //单个(将inline元素的高度和line-height设为一直即可)
    //多个
    .parent{
        width: 300px;
        height: 300px;
        dispaly: table-cell;
        vertical-align: middle;
    }
```
###### 块状元素
```
    //已知高度（将待居中元素设置为绝对定位，并且设置margin-top为居中元素高度一半的负值）
    .div{
        width:100px;
        heght:100px;
        position:absolute;
        top: 50%;
        margin-top: -50px;
    }
    //未知高度（使用transform,垂直移动-50%）
    .div{
        width: 100pc;
        top: 50%;
        position: absolute;
        transform: translateY(-50%);
    }
```
##### 水平垂直居中
###### 已知高度和宽度(使用绝对定位，将元素的margin-left和margin-top设为元素宽度和高度的一半负值)
```
    .div{
        width: 100px;
        height: 100px;
        position: absolute;
        top: 50%;
        left: 50%;
        margin-left: -50px;
        margin-top: -50px;
    }
```
###### 未知高度和宽度(将设置元素绝对定位，并且设置transform的translate为Ｘ，Ｙ轴同时移动-50%即可)
```
    .div{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }
```
###### flex布局
```
    .parent{
        display: flex;
        justify-content:center;
        align-items: center;
        /*父级设置高度查看效果*/
        height: 300px;
    }
    .div{
        width: 100px;
    }
```