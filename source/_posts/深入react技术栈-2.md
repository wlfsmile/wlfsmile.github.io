---
title: 深入react技术栈-2
date: 2017-04-10 20:31:30
tags: react
categories: 前端
---
#### 第一章（Raact数据流、React生命周期、React与DOM）
##### React数据流
在React中，数据是自项向下单向流动的。即从父组件到子组件。
+ 如果顶层组件初始化props，那么React会向下遍历整棵组件树，重新尝试渲染所有相关的子组件。而state只关心每个组件自己的内部的状态，这些状态只能在组件内改变。
<!-- more -->
###### state
+ 当组件内部使用库内置的setState方法时，最大的表现行为就是该组合会尝试重新渲染。
```
import React,{Component} form 'react';
class Counter extends Component{
    constructor(pros){
        super(props);
        this.state = {
            count:0,
        };
    }
    handleClick(e){
        e.preventDefault();
        this.setState({
            count : this.state.count + 1,
        });
    }
    render(){
        renturn(
            <div>
                <p>{this.state.count}</p>
                <a href="#" onClick={this.handleClick}>更新</a>
            </div>
        );
    }
}
```
上述例子是通过点击“更新”按钮不断的更新内部值。把组件内状态封装在现实中。
+ setState 是一个异步方法。一个生命周期内所有的setState方法会合并操作。

###### props
+ 是react用来让组件之间相互联系的一种机制。
+ react的单向数据流，主要的流动管道是props。其本身不可变的，组件的props一定来自默认属性或者通过父组件传递而来。
+ 组件的部分props
    + className：根节点的class。
    + classPrefix：class的统一前缀。有助于对样式与交互分离。
    + defaultActiveIndex和activeIndex：默认的激活索引。
    + onChange：回调函数。一般与activeIndex搭配使用。
+ react为props提供了默认配置，可通过defaultProps静态变量的方式定义。
```
static defaultProps ={
    classPrefix:'',
    onChange:()=>{},
};
```
+ 子组件prop
    + 在react中有一个重要且内置的prop——children，代表组件的子组件集合。
    + React.Children是React官方提供的一系列操作children的方法。提供诸如map、forEach、count等实用函数。
+ 组件props
+ 用function prop与父组件通信
    + 对于state，它的通信集中在组件内部。对于props，它的通信是父组件向子组件的传播。
+ propType
    + 用于规范props的类型与必需的状态。若组件定义了propType，那么在开发环境下，就会对组件的props值的类型作检查。  

##### React生命周期
 react组件的生命周期分为挂载、渲染和卸载。
 
###### 挂载或卸载过程
+ 组件的挂载
    + 主要做组件状态的初始化
    ```
    import React,{Component,PropTypes} form 'react';
    class App extends Component{
        static propTypes={
            //...
        };
        static defaultProps={
            //...
        };
        constructor(props){
            super(props);
            this.state={
                //...
            };
        }
        componentWillMount(){
            //...
        }
        componentDidMount(){
        //...
        }
        render(){
            return <div>This is a demo.</div>;
        }
    }
    
    ```
+ 两个生命周期方法
    + componentWillMount，在render方法之前执行。（渲染前）
    + componentDidMount，在render方法之后执行。（渲染后）
+ 执行setState方法
    + 在componentWillMount中，组件会更新state，但组件只渲染一次。无意义的执行
    + 在componentDidMount中，组件会再次更新，在初始化过程中就渲染了两次。
+ 组件的卸载
    + 只有componentWillMount这一个卸载前状态。

###### 数据更新过程
+ 指的是父组件向下传递props或组件自身执行setState方法时发生的一系列更新动作。
```
import React,{Component,PropTypes} from 'react';
class App extends Component{
    componentWillReceiveProps(nextProps){
        //this.setState({})
    }
    shouldComponentUpdate(nextProps,nextState){
        //...
    }
    componentWillUpdate(nextProps,nextState){
        //...
    }
    componentDidUpdate(prevProps,prevState){
        //...
    }
    render(){
        return <div>This is a demo.</div>
    }
}
```
+ 组件自身的state更新，会依次执行shouldComponentUpdate、componentWillUpdate、rebder和componentDidUpdate。
+ shouldComponentUpdate 接收需要更新的props和state。开发者加入必要的条件判断，当方法返回false，组件不再向下执行生命周期方法。
+ componentWillUpdate方法提供需要更新的props和state，代表更新过程中渲染前时刻；componentDidUpdate方法提供更新前的props和state，代表更新过程中渲染后时刻。
+ 如果组件是由父组件更新props而更新的，那么在shouldComponentUpdate之前会先执行componentWillReceiveProps方法。在此方法中调用setState不会有二次渲染。

##### React与DOM
+ 在组件开实现中，不会用到ReactDOM，只有在顶层组件以及由于React模型所限而不得不操作DOM的时候，才会用。

###### ReactDOM
+ findDOMNode
    + React提供的获取DOM元素的方法。
    ```
    DOMElement findDOMNode(ReactComponent component)
    ```
    + 当组件被渲染到DOM中之后，findDOMNode返回该React组件实例相应的DOM节点。它可以用于获取表单的value以及用于DOM的测量。
    ```
    //当前组件加载完时获取当前DOM
    import React,{Component} from 'react';
    import ReactDOM from 'react-dom';
    class App extends Component{
        componentDidMount(){
            //this为当前组件实例
            const dom = ReactDOM.findDOMNode(this);
        }
        render(){}
    }
    ```
    + findDOMNode只对已经挂载的组件有效。
+ render
    + 用于把React渲染的Virtual DOM渲染到浏览器的DOM当中。
    ```
    ReactComponent render(
        ReactElement element,
        DOMElement container,
        [function callback]
    )
    ```
    + 该方法把元素挂载到container中，并返回element实例（refs引用）。如果是无状态组件，render会返回null，当组件装载完毕时，callback就会被调用。
    + 再次更新时，React不会把组件重新渲染，而是用DOM diff算法做局部更新。
    
###### ReactDOM的不稳定方法
 + 有两种不稳定方法

###### refs
+ 组件中特殊的prop，可以附加到任何一个组件上。组件被调用时会指向一个实例，而refs会指向这个实例。
+ 可以是一个回调函数，这个回调函数会在组件挂载之后立即执行。
+  refs同样支持字符串，对于DOM操作，不仅可以使用findDOMNode获得该组件DOM，还可以使用refs获得组件内部的DOM。

###### React之外的DOM操作
+ React的 声明式渲染机制把复杂的DOM操作抽象为简单的state和props的操作，因此避免了很多直接的DOM操作。不过，仍有一些DOM操作是React无法避免的。