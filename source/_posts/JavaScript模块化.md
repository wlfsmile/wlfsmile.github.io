---
title: JavaScript模块化
date: 2018-01-10 13:22:06
tags: [JavaScript,模块化]
categories: [面试准备,JavaScript]
---
### JavaScript——模块化

#### 什么是模块？
就好比写书的作者一样，会把一本书分成一个个的章节，再把每一章的内容分成若干节，然后构建成一本完整的书，让书更加有条理性和可阅读性。而作为一个程序员，就可以把代码分成一些模块，然后组成一个完整的“功能”

#### 模块化的好处——降低代码耦合性
+ 可维护性：模块的独立性，一个良好的模块会减少外面的代码对自己依赖，可以独立的去更新和改进
+ 命名空间：莫葵花开发封装变量，可以避免污染全局环境
+ 可复用性：将自己的模块引入到不同项目，避免复制粘贴

<!-- more -->

#### 模块化的实现

1. ES6之前
在ES6之前，JavaScript不支持类的概念，而刚好创建一个函数的时候，解析器会针对函数产生一个新的运行环境，函数运行完了之后就会销毁一切。从而就可以随意创建对象，达到了类似私有变量的效果，避免了父命名空间的冲突

##### 匿名函数
```js
(function(){
    //在函数的作用域中下面的变量是私有的
    var myGrades = [93,95,88,0,55,97];

    var average = function(){
        var total = myGrades.reduce(function(accumulator,item){
            return accumulator+item;
        },0)
        return 'Your average grade is ' + total / myGrades.length + '.';
    }

    var failing = function(){
        var failingGrades = myGrades.filter(function(item){
            return item < 70;
        })
        return "you failed" + failingGrades.length + 'times.';
    }
    console.log(failing()); //You failed 2 times.

}());
```
+ 注：要用圆括号把整个函数包起来，如果语句直接以关键词开始，会被认为是一个函数声明，而不是函数语句。函数声明必须有名字，函数语句才可以匿名

##### 把全局函数注入到匿名函数
```js
(function (globalVariable) {

  // 在函数的作用域中下面的变量是私有的
  var privateFunction = function() {
    console.log('this is private!');
  }

  // 通过全局变量设置下列方法的外部访问接口
  // 与此同时这些方法又都在函数内部

  globalVariable.each = function(collection, iterator) {
    if (Array.isArray(collection)) {
      for (var i = 0; i < collection.length; i++) {
        iterator(collection[i], i, collection);
      }
    } else {
      for (var key in collection) {
        iterator(collection[key], key, collection);
      }
    }
  };

  globalVariable.filter = function(collection, test) {
    var filtered = [];
    globalVariable.each(collection, function(item) {
      if (test(item)) {
        filtered.push(item);
      }
    });
    return filtered;
  };

  globalVariable.map = function(collection, iterator) {
    var mapped = [];
    globalUtils.each(collection, function(value, key, collection) {
      mapped.push(iterator(value));
    });
    return mapped;
  };

 }(globalVariable));
```

+ 在上例中,`globalVariable`是唯一一个全局变量，这种做法比完全匿名的闭包的好处是，代码结构更清晰，而且性能更好。我们能够看到函数内部传递进来了全局变量，所以依赖关系非常清晰。其次在函数内部调用 `globalVarible` 的时候，解释器能够直接找到局部的 `globalVarible`，就不用上溯到外部的 `globalVarible`.

##### 对象接口

```js
var myGradesCalculate = (function(){

    var myGrades = [93,95,88,0,55,97];
    // 通过接口在外部访问下列方法
    // 与此同时这些方法又都在函数内部
    return {
        average: function(){
            var total = myGrades.reduce(function(accumulator,item){
                return accumulator+item;
            },0);

            return 'Your average grade is ' + total / myGrades.length + '.';
        },
        failing: function() {
            var failingGrades = myGrades.filter(function(item) {
                return item < 70;
            });

            return 'You failed ' + failingGrades.length + ' times.';
        }
    }
})();

myGradesCalculate.failing(); // 'You failed 2 times.' 
myGradesCalculate.average(); // 'Your average grade is 71.33333333333333.'
```

+ 立即执行的匿名函数返回了一个对象。这样就算使用独立的对象接口

##### 把要公开的属性专门放在一个对象声明中返回

```js
var myGradesCalculate = (function () {
    
  var myGrades = [93, 95, 88, 0, 55, 91];
  
  var average = function() {
    var total = myGrades.reduce(function(accumulator, item) {
      return accumulator + item;
      }, 0);
      
    return'Your average grade is ' + total / myGrades.length + '.';
  };

  var failing = function() {
    var failingGrades = myGrades.filter(function(item) {
        return item < 70;
      });

    return 'You failed ' + failingGrades.length + ' times.';
  };
  // 将公有指针指向私有方法

  return {
    average: average,
    failing: failing
  }
})();

myGradesCalculate.failing(); // 'You failed 2 times.' 
myGradesCalculate.average(); // 'Your average grade is 71.33333333333333.'
```

+ 相比于前一种方法，这种是默认所有的变量和方法都是私有的，只在最后显示return对象的时候，才选择暴露出对外接口，接口比较清晰

##### CommonJS和AMD
上面这些方法都有一个共同点：**使用一个特定的全局模块名来把一些私有变量和方法包起来，然后通过闭包来创建一个私有的命名空间。**

缺点：
+ 无法管理不同模块之间的依赖关系。因为js是顺序执行的，所以必须搞清楚js文件的依赖关系，一旦js文件过多，顺序加载就很麻烦了。
+ 命名空间冲突无法解决

##### CommonJS
+ 每个JS文件都是一个独立的模块上下文，在这个上下文中默认创建的属性都是私有的。对其他文件是不可见的。

**做法：** 通过`module.exports`对象暴露对外接口。通过`require()`进行调用
**典例：** Node.js

**优势：**
+ 避免全局命名空间污染，require 进来的模块可以被赋值到自己随意定义的局部变量中
+ 依赖关系更加清晰

**特点：**
+ 主要适用场景是服务器端编程，采用的是**同步加载**模块策略。依赖的模块逐个加载

**劣势：**
+ 服务器模块加载主要来源硬盘或内存，所以加载速度比较快，同步加载影响不大。单是浏览器端，网络中加载一个模块比硬盘加载慢，在等待加载过程中，浏览器会挂起当前进程，知道模块下载完成

##### AMD（异步模块定义）
**使用方式：**
```js
define([moduleA,moduleB],function(moduleA,moduleB){
  console.log(moduleA.hello());
});
```
+ 第一个参数是一个数组，数组中有两个字符串也就是需要依赖的模块名称。AMD 会以一种**非阻塞**的方式，通过`appendChild`将这两个模块插入到`DOM`中。在两个模块都加载成功之后，define 会调用第二个参数中的回调函数，一般是函数主体。
+ `define`既是一种引用模块的方式，也是定义模块的方式

```js
//moduleA
define([],function(){
  return {
    hello: function(){
      console.log('hello');
    },
    goodbye: function(){
      console.log('bye');
    }
  }
});
```

**特点：**
+ 优先浏览器，异步载入模块
+ 支持在模块中使用对象、函数、构造函数、字符串、JSON等数据类型，而CommonJS只支持对象
+ AMD不支持Node里的一些诸如 IO,文件系统等其他服务器端的功能

##### UMD(通用模块定义规范)

对于需要同时支持 `AMD` 和 `CommonJS` 的模块而言，可以使用 UMD,并且UMD指出全局变量定义，所以UMD可以同时在客户端和服务端使用

```js
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
      // AMD
    define(['myModule', 'myOtherModule'], factory);
  } else if (typeof exports === 'object') {
      // CommonJS
    module.exports = factory(require('myModule'), require('myOtherModule'));
  } else {
    // Browser globals (Note: root is window)
    root.returnExports = factory(root.myModule, root.myOtherModule);
  }
}(this, function (myModule, myOtherModule) {
  // Methods
  function notHelloOrGoodbye(){}; // A private method
  function hello(){}; // A public method because it's returned (see below)
  function goodbye(){}; // A public method because it's returned (see below)

  // Exposed public methods
  return {
      hello: hello,
      goodbye: goodbye
  }
}));
```

+ 在执行UMD规范时，会优先判断是当前环境是否支持AMD环境，然后再检验是否支持CommonJS环境，否则认为当前环境为浏览器环境（window）。

2. ES6模块（原生JS）
**特点：**
+ JS原生支持的
+ 简洁语法并且支持**异步加载**
+ import 进来的模块对于调用它的模块来是说是实时**只读**的，而CommonJS只是相当于把代码复制过来

```js
//CommonJS
// lib/counter.js

var counter = 1;

function increment() {
  counter++;
}

function decrement() {
  counter--;
}

module.exports = {
  counter: counter,
  increment: increment,
  decrement: decrement
};


// src/main.js

var counter = require('../../lib/counter');

counter.increment();
console.log(counter.counter); // 1
```


```js
//import
// lib/counter.js
export let counter = 1;

export function increment() {
  counter++;
}

export function decrement() {
  counter--;
}


// src/main.js
import * as counter from '../../counter';

console.log(counter.counter); // 1
counter.increment();
console.log(counter.counter); // 2
```

#### 模块化与组件化

#### 参考资料：
[JavaScript模块化编程简史（2009-2016）](https://yuguo.us/weblog/javascript-module-development-history/)
[JavaScript 模块化入门Ⅰ：理解模块](https://zhuanlan.zhihu.com/p/22890374)
[JavaScript Modules: A Beginner’s Guide](https://medium.freecodecamp.org/javascript-modules-a-beginner-s-guide-783f7d7a5fcc)
[深入理解JavaScript系列（3）：全面解析Module模式](http://www.cnblogs.com/TomXu/archive/2011/12/30/2288372.html)

