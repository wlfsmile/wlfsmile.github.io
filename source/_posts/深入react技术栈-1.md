---
title: 深入react技术栈-1
date: 2017-04-10 20:24:24
tags: react
categories: 前端
---
#### 第一章（基本介绍、JSX、React组件）
+ react是Facebook开源在github上的JavaScript库。把用户界面抽象成一个个组件。引用了JSX语法
<!-- more -->

##### 虚拟DOM
+ react把真实DOM树转成JavaScript对象树（虚拟DOM）。
+ 虚拟DOM提升了react的性能。还方便和其他平台集成。
+ react承载了构建HTML结构化页面的职责。是通过创建与更新虚拟元素来管理整个虚拟DOM。
##### JSX
+ 用意在于通过加入增强语法，使JavaScript更快、安全、简单。
+ 虚拟元素的构建和更新都是在内存中完成的，并不会真正的渲染到DOM中去。
+ react创建的虚拟元素分为DOM元素和组件元素两种。分别对应着原生DOM元素与自定义元素。
+ 因为元素有公共的表达方法，我们就可以让元素们彼此嵌套或混合。这种层层封装的组件元素就是所谓的react组件，最终可以用递归渲染的方式构建出完全的DOM元素树。
+ JXS是将HTML语法直接加入到JavaScript代码中，再通过翻译器转换到纯JavaScript后由浏览器执行。
##### JSX基本语法
+ JSX是类XML语法的ECMAScript扩展，可以说，JSX基本语法被XML囊括，但也有些许不同 
###### XML基本语法
+标签可以任意嵌套。可以清晰地看到DOM树状结构及其属性。

```
const List = () =>(
    <div>
        <Title>title</Title>
        <ul>
            <li>list</li>
            <li>list</li>
        </ul>
    </div>
);
```
+ 注意
    - 定义标签时，只允许被一个标签包裹。
    - 标签一定要闭合。
###### 元素类型
+ HTML标签首字母为小写，对应DOM元素；反之，则对应组件元素。
+ 依赖的组件中元素不再出现组件元素，就可以将完整的DOM树构建出来。
+ JSX还可以通过命名空间的方法使用组件元素，可以解决组件命名冲突和对一组组件进行归类。
+ 注释
    +   JSX中未定义注释的转换。不过在一个组件的子元素位置使用注释要用{}包起来。
```
    const App={
        <Nav>
            {/* 节点注释*/}
            <Person
            /* 多行
            注释 */
            name={window.name}
        </Nav>
    }
```
###### 元素属性
在JSX中，DOM和组件元素都有属性。
+ DOM元素的属性是标准规范化属性，除了class和for。
    + class——className
    + for——HTMLFor
+ 组件元素的属性是完全自定义的属性。
+ Boolean属性
     + 省略Boolean属性值会导致JSX任务bool值设为了true。
+ 展开属性
    + 如果事先不知道设置那些pros，最好不要设置。可以用ES6 rest/spread特性来提高效率
```
//可以将
const data = {name:'foo',value:'bar'};
const component = <Component name={data.name} value={data.value} />;
//写为
const data = {name:'foo',value:'bar'};
const component = <Component {...data}/>;
```
+ 自定义HTML属性
        + 如果在JSX中往DOM元素中传入自定义属性，react不会渲染。若要使用，要使用data-前缀。
        + 在自定义标签中任意属性都是被支持的。
        + 以aria-开头的网络无障碍属性同样可正常使用。
###### JavaScript属性表达式
+ 属性值要使用表达式，只要用{}替换""即可。
###### HTML转义

##### React组件
+ 组件封装的基本思路是面向对象思想。交互基本上以操作DOM为主，逻辑上是结构上需要改变哪里，我们就操作哪里。
+ 规范化标准组件
    + 基本的封装性。
    + 简单的生命周期呈现。
    + 明确的数据流动。数据指的是调用组件的参数。
###### React组件的构建
+ react组件由属性（pros）、状态（state）以及生命周期方法三个部分组成。
+ react自定义元素是库自己建成的
+ react渲染过程包含模板的概率，及JSX
+ react组件的实现均在方法与类中。所有可以相互隔离，但不包括样式
+ react引用方式遵循ES6 module标准
###### React组件的构建方法
+ React基本上由组件的构建方式、组件内的属性状态与生命周期方法组成。
+ React组件的构造方法：React.createClass、ES6 classes和无状态函数。
+    React.createClass
    +   是Reactz=最传统、兼容性最好的方法。

```
    const Button = React.createClass({
        getDefaultProps(){
            return{
                color:'blue',
                text:'Confirm',
            };
        },

        render(){
            const {color,text} = this.props;
            return(
                <button className={`btn btn-${color}`}>
                    <em>{text}</em>
                </button>
            );
        }
    });
    
```

+ ES6 classes 
    + 写法是通过ES6标准的类语法的方式来构建方法

```
import React,{Component} form 'react';
class Button extends Component{
    constructor(props){
        super(props);
    }
    static defaultProps = {
        color:'blue',
        text:'Confirm',
    };
    render(){
        const {color,text} = this.props;
        return(
            <button className={`btn btn-${color}`}>
                <em>{text}</em>
            </button>
        );
    }
}
```
+ 无状态函数
```
function Button({color='blue',text='Confirm'}){
    return(
        <button className={`btn btn-${color}`}>
            <em>{text}</em>
        </button>
    );
}
```