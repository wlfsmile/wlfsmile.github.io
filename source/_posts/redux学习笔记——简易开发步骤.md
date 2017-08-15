---
title: redux学习笔记——简易开发步骤
date: 2017-07-10 15:18:40
tags:
---
这篇文章主要是写我一开始学习redux的开发的步骤，比较适合刚刚入门的小白，不知道怎么开始下手代码,本篇文章以计数器为例。本篇文章参考[Redux学习笔记：Redux简易开发步骤](http://www.cnblogs.com/yinluhui0229/p/6709782.html) 
github代码地址[react-redux-counter](https://github.com/wlfsmile/Redux/tree/master/counter)

<!-- more -->

#### 项目结构
```
    .
    ├─.babelrc                            // babel的配置
    ├─.gitignore                          // git忽略上传的文件
    ├─package.json                        // npm命令包
    ├─readme.md                           // 项目介绍
    ├─node_modules
    ├─public                              // 展示页面html入口
    |  └index.html
    ├─src                        
    |      ├─components                   // react component
    |      |    ├─Counter.js
    |      ├─reducers
    |      |    ├─index.js                // 操作信息并改变state
    |      └index.js                      // 主文件js
```

#### 步骤
1. index.js(主文件js)
2. 定义render入口并调用Counter
```
    const render = () => ReactDOM.render(
        <Counter 
        value={}
        onIncrement={}
        onDecrement={}
         />,
        document.getElementById('root')
    )
```
3. 定义Counter，也就是React Component，生成DOM结构，render时触发。
```
    import React,{Component} from 'react';
    import {render} from 'react-dom';

    class Counter extends Component{

      constructor(props) {
        super(props);
      }

      render(){
        const {value,onDecrement,onIncrement} = this.props;
        return(
          <p>
            Clicked: {value} times
            {' '}
            <button onClick={onIncrement}>
              +
            </button>
            {' '}
            <button onClick={onDecrement}>
              -
            </button>
          </p>
        );
      }

    }

    export default Counter;
```

4. 初始化显示，手动调用render()，第一次初始化时定义，后续不在执行
```
    render();
```
5. 创建store，并绑定reducer
```
    const store = createStore(reducer); // createStore的第一个参数必须是个函数，store.dispatch()时执行，该函数就叫reducer
```
6. 定义Action，调用store.dispatch
```
    const render = () => ReactDOM.render(
        <Counter
            value={store.getState()}
            onIncrement={() => store.dispatch({ type: 'INCREMENT' })}
            onDecrement={() => store.dispatch({ type: 'DECREMENT' })}
        />,
        rootEl
    )

    render()
```
7. 定义Reducer，生成新的state(reducer/index.js)
```
    export default (state = 0, action) => {
      switch (action.type) {
        case 'INCREMENT':
          return state + 1
        case 'DECREMENT':
          return state - 1
        default:
          return state
      }
    }
```
8. 定义state变化监听(index.js主文件)
```
    store.subscribe(render)
```
