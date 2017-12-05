---
title: JavaScript——部分ES6新特性
date: 2017-11-30 20:45:32
tags: [JavaScript,ES6]
categories: [面试准备,CSS]
---
本章主要讲了部分常用的ES6新特性，写的比较简单
<!--more  -->
#### 默认参数
```
var a = function(m,n){
    var m = m || 50;
    var n = n || 'es';
    ...
}
```
变为直接放在函数签名中，因为如果参数为0,在JavaScript中为false
```
var a = function(m=50,n='es'){
    //do something
}
```
规范：设定了默认值的入参，应该放在没有设置默认值的参数之后

#### 模板字符串
```
var myname = 'wlfsmile';
var yourname = 'youname';

var name = 'your name is'+ yourname +'and my name is'+ myname;
```
变为
```
var name = `your name is ${yourname} and my name is ${myname}`;
```

#### 解构赋值
+ ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值  

数组
```
    var [a,b,c]=[11,22,33]
    console.log(a,b,c)//11 22 33

    var [a, ,b] = [1, 2, 3, 4, 5];
    console.log(a); // => 1
    console.log(b); // => 3
```
对象
```
var{name,age}={name:"张三",age:"20"}
    console.log(name,age)//张三 20
```
解构json
```
var jike = {"name":"tom","age":"23","sex":"男"};
    var {name,age,sex}=jike;
    console.log(name,age,sex)//tom 23 男

    function cal(a,b){
        var ret1 = a+b;
        var ret2 = a-b;
        var ret3 = a*b;
        var ret4 = a/b;
        return [ret1,ret2,ret3,ret4]
    }
    var [r1,r2,r3,r4] = cal(10,5);
    console.log(r1,r2,r3,r4)//15 5 50 2

```
#### let 和 const
##### let
+ 无变量提升
+ 有块级作用域
+ 禁止重复声明

##### const
+ 无变量提升
+ 有块级作用域
+ 禁止重复声明
+ 禁止重复赋值
+ 必须要附初始值

#### 箭头函数
##### 特性
+ 共享父级this对象
+ 共享父级arguments
+ 不能当做构造函数（因为箭头函数没有自己的this对象）

##### 语法
+ 当箭头函数入参只有一个时可以省略入参括号
+ 当入参多余一个或没有入参时必须写括号
+ 当函数体只有一个 return 语句时可以省略函数体的花括号与 return 关键字

##### this
```
//before
var obj = {
    arr: [1, 2, 3, 4, 5, 6],
    getMaxPow2: function() {
        var that = this,
            getMax = function() {
                return Math.max.apply({}, that.arr);
            };
        
        return Math.pow(getMax(), 2);
    }
}

//after
var obj = {
    arr: [1, 2, 3, 4, 5, 6],
    getMaxPow2: function() {
        var getMax = () => {
            return Math.max.apply({}, this.arr);
        }

        return Math.pow(getMax(), 2);
    }
}
```

+ 在箭头函数中，函数体内部没有自己的 this，默认在其内部调用 this 的时候，会自动查找其父级上下文的 this 对象（如果父级同样是箭头函数，则会按照作用域链继续向上查找）

注意
+ 有的情况函数需要自己的this，例如DOM事件绑定时候回调函数，需要使用this操作DOM，这时候只能使用传统的匿名函数
+ 在严格模式下，如果箭头函数的上层函数均为箭头函数，那么this对象将不可用

##### arguments
+ 当函数体中有另外一个函数，并且该函数为箭头函数时，该箭头函数的函数体中可以直接访问父级函数的 arguments 对象。

```
function getSum() {
    var example = () => {
        return Array
            .prototype
            .reduce
            .call(arguments, (pre, cur) => pre + cur);
    }

    return example();
}
getSum(1, 2, 3); // => 6
```
+ 由于箭头函数本身没有 arguments 对象，所以如果他的上层函数都是箭头函数的话，那么 arguments 对象将不可用。

#### 类与继承
+ 本质为对原型链的二次包装
+ 类没有提升
+ 不能使用字面量定义属性
+ 动态继承类的构造方法中super优先于this

```
/* 类不会被提升 */
let puppy = new Animal('puppy'); // => ReferenceError

class Animal {
    constructor(name) {
        this.name = name;
    }

    sleep() {
        console.log(`The ${this.name} is sleeping...`);
    }

    static type() {
        console.log('This is an Animal class.');
    }
}

let puppy = new Animal('puppy');

puppy.sleep();    // => The puppy is sleeping...

/* 实例化后无法访问静态方法 */
puppy.type();     // => TypeError

Animal.type();    // => This is an Animal class.

/* 实例化前无法访问动态方法 */
Animal.sleep();   // => TypeError

/* 类不能重复定义 */
class Animal() {} // => SyntaxError
```
注意
+ 类的定义中有一个特殊的constructor()，该方法名固定，表示该类的构造函数（方法），在类的实例化过程中会被调用
+ 类中无法像对象一样使用 prop: value 或者 prop = value 的形式定义一个类的属性，我们只能在类的构造方法或其他方法中使用 this.prop = value 的形式为类添加属性。

##### 类的继承
```
class Programmer extends Animal {
    constructor(name) {
        /* 在 super 方法之前 this 不可用 */
        console.log(this); // => ReferenceError
        super(name);
        console.log(this); // Right!
    }
    
    program() {
        console.log("I'm coding...");
    }

    sleep() {
        console.log('Save all files.');
        console.log('Get into bed.');
        super.sleep();
    }
}

let coder = new Programmer('coder');
coder.program(); // => I'm coding...
coder.sleep();   // => Save all files. => Get into bed. => The coder is sleeping.
```

+ 如果子类有构造方法，那么在子类构造方法中使用 this 对象之前必须使用 super() 方法运行父类的构造方法以对父类进行初始化。
+ 在子类方法中我们也可以使用 super 对象来调用父类上的方法。如示例代码中子类的 sleep() 方法：在这里我们重写了父类中的 sleep() 方法，添加了两条语句，并在方法末尾使用 super 对象调用了父类上的 sleep() 方法。

#### 模块化（import和export）
+ export暴露，import引入
+ 封闭的代码（每个模块都有自己完全独立的代码块，跟作用域类似，但是更加封闭）
+ 无限制导出导出
+ 默认严格模式下运行

参考[ES6 常用新特性讲解](https://segmentfault.com/a/1190000010230939)