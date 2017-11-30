---
title: JavaScript——引用类型
date: 2017-11-23 20:51:20
tags: [面试准备,JavaScript]
---
#### Object
##### 创建方法
+ 对象字面量
+ new

<!--more  -->
##### new一个对象的过程
+ 创建一个对象
+ 将构造函数的作用域赋给新对象（this指向这个新对象）
+ 执行构造函数中的代码（为新对象添加属性）
+ 返回新对象

#### Array
##### 创建方法
+ 使用Array构造函数
+ 数组字面量

##### 检测方法
+ instanceof
    + if(value instanceof Array){//do Something}
+ Array.isArray()

##### 转换方法
+ toString():返回由数组中每个值的字符串形成而拼接而成的一个以逗号分隔的字符串
+ valueOf():返回数组
+ toLoacleString():创建一个数组值以逗号分隔的字符串

##### 栈方法（后进先出）
+ push():添加到数组末尾，返回修改后数组长度
+ pop():从数组末尾移除最后一项，返回移除的项

##### 队列方法（先进先出）
+ push()
+ shift():移除第一项，返回移除项
+ unshift():数组前端添加任意个项，返回新数组长度

##### 重排序方法
+ reverse()：反转排序
+ sort():升序

##### 操作方法
+ concat():添加。里面的值添加合为一个结果数组
+ slice():取出其中项
+ splice():插入项

##### 位置方法
+ indexOf():indexOf(要查找的项,（可选）表示查找起点的位置索引)，从前到后
+ lastIndexOf():从末尾向前
+ 没有找到返回-1

##### 迭代方法
##### 归并方法

#### Date
#### RegExp
#### Function
+ 函数是对象，函数名是指针

##### 函数声明与函数表达式
```
//函数声明
function sum(num1,num2){
    //do something
}

//函数表达式
var sum = function(num1,num2){
    //do something
};
```
 >解析器在向执行环境中加载数据时，解析器会先读取函数声明，并使其在执行任何代码之前可用（可访问），函数表达式则必须等到解析器执行到它所在代码行，才会被真正的解释执行。（函数声明提升）

##### 内部属性（对象）
+ arguments
+ this

##### 函数属性
+ length
+ prototype

##### 函数方法(非继承)
+ call()
+ apply()
+ bind()

#### 基本包装类型
+ Boolean Number String
```
//若
var s1 = "some text";
var s2 = s1.substring(2);
//s1是字符串，没有方法，但是后台自动创建一个对应的基本包装类型的对象(String)
//实际是
var s1 = new String("some text");
var s2 = s1.substring(2);
s1 = null;
```
##### 过程
+ 创建String类型的一个实例
+ 在实例上调用指定方法
+ 销毁实例

##### 基本包装类型与引用类型的区别
+ 对象的生存期
>使用new操作符创建的引用类型的实例在执行流离开当前作用域之前一直保存在内存内。而自动创建的基本包装类型对象，值存在于一行代码执行的瞬间

#### 单体内置对象
+ 定义
>由ECMAScript提供的、不依赖与宿主环境的对象这些对象在ECMAScript程序执行之前就已经存在了（ECMA-262）  
意思就是，不必显示地实例化内置对象，因为已经实例化了

+ 例子
    + Object Array String
    + Global:大多数ECMAScript实现中不能直接访问Global对象，Web浏览器实现了承担该角色的window对象。全局变量和函数都是Global对象的属性
    + Math