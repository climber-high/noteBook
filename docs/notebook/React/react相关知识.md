### react相关知识

>用于构建用户界面的javaScript库

>特点:虚拟DOM(render函数加载虚拟dom)、组件化、jsx语法

>Create React App 是一个官方支持的创建 React 单页应用程序

>vs code react插件:ES7 react

``` 
npm init react-app my-app
```

## 使用react（原理）

```
在src创建index.js并引用
import React from 'react'
import ReactDOM from 'react-dom'

//这种方式可以理解为创建了一个简单的react元素
//const app = <h1>welcome React!!!</h1>;

const createApp = (props) => {
    return (
        <div>  //只能有一个根元素
            {/* 只要在jsx里要插入js的代码，就加一层花括号即可，注释也是js，
            所以这里的注释就加了一层花括号 */}
            <h1>{props.title}</h1>
        </div>
    )
}

const app = createApp({
    title: 'react 16.8'
})

ReactDOM.render(
    app,
    document.querySelector('#root')
)
```

## 创建组件

>创建组件的第一种方式：使用箭头函数，但是这个名字要大写开始。

**render()是react dom提供的一个方法，这个方法通常只会用一次**

```
import React from 'react'
import ReactDOM from 'react-dom'

const App = (props) => {
  return (
    <div>
      <h1 title={props.title}>Welcome {props.title}!!!</h1>
    </div>
  )
}

ReactDOM.render(
  <App title="react" />,
  document.querySelector('#root')
)

/---------------原理--------------/
const createApp = (props) => {
    return (
        <div>  //只能有一个根元素
            {/* 只要在jsx里要插入js的代码，就加一层花括号即可，注释也是js，
            所以这里的注释就加了一层花括号 */}
            <h1>{props.title}</h1>
        </div>
    )
}

const app = createApp({
    title: 'react 16.8'
})

ReactDOM.render(
    app,
    document.querySelector('#root')
)
```

>创建组件的第二种方式:类组件,使用类继承React.Component

```
import React from 'react'
import {render} from 'react-dom'

const Pcom = () => <p>类组件</p>

class App extends React.Component{
    render(){
        return (
            <div>
                {this.props.desc}!!!
                <Pcom />  //组件嵌套
            </div>
        ) 
    }
}

render(
    <App desc="react" />,
    document.querySelector('#root')
)

/--------------原理---------------/
const app = new App({
    desc : 'react'
}).render();

render(
    app,
    document.querySelector('#root')
)
```

>在react16以前，使用这种方式来创建一个类组件

```
 React.createClass({
   render () {
     return <h1>xxxx</h1>
   }
 })
```

## jsx原理

```
import React, { Component } from 'react'
import { render } from 'react-dom'

// 所以react在真正的渲染的时候会把上面的代码编译为下面这个样子来运行，下面的代码就是合法的js代码
class App extends Component {
  render () {
    return (
      // React.createElement是一个方法，用于创建元素，可以有很多的参数，但是前两个是固定的：
      // 第一个可以理解为标签名
      // 第二个可以理解为标签的属性
      // 剩下的，你就继续写更多的子元素吧
      // React.createElement(type, [props], [...children])
      React.createElement(
        'div',
        {
          className: 'app',
          id: 'appRoot'
        },
        React.createElement(
          'h1',
          {
            className: 'title'
          },
          'jsx原理'
        ),
        React.createElement(
          'p',
          null,
          '类组件是继承React.Component的'
        ),
      )
    )
  }
}

render(
  <App />,
  document.querySelector('#root')
)
```

## 组件中的样式

```
import React, {Component} from 'react'
import {render} from 'react-dom'

class App extends Component{
    render () {
        const style = {'color':'red'}  //1.使用style内联创建
        return (
            <div>
                <div style = {style}>内联创建</div>

                <div className = 'txt'>class的方式</div>  
                //2.使用class的方式，但是在react里class要写成className
            </div>
        )
      }
}

render(
    <App />,
    document.querySelector('#root')
)
```

>要动态添加不同的className就可以使用第三方的包叫classNames

```
cnpm i classnames -S
```

例:
```
import React, {Component} from 'react'
import {render} from 'react-dom'
import classNames from 'classnames'

class App extends Component{
    render () {
        return (
          <div className={classNames('a', {'b':true}, {'c':false})}>
            666
          </div>
        )
      }
}

render(
    <App />,
    document.querySelector('#root')
)
```

>把样式抽出来当组件使用

```
第三方包:styled-components

cnpm i styled-components -S
```

例:
```
import React, {Component} from 'react'
import {render} from 'react-dom'
import styled from 'styled-components'

const Title = styled.h1`
  color: #F00;
  font-size: 20px;
`

class App extends Component{
    render () {
        return (
            <Title>666</Title>
        )
      }
}

render(
    <App />,
    document.querySelector('#root')
)
```

## 项目结构

```
vs code react插件:ES7 react
快捷键:rcc 自动生成代码块,类组件写法
快捷键:rfc 自动生成代码块,函数返回jsx写法
```

>index.js用于渲染组件，App.js为顶级父组件

```
src目录下创建components目录用来放各个组件，
组件文件夹用大写，目录下可以用index.js命名，import时自动找目录下的index.js文件
components目录下再创建index.js文件，负责导出所有组件

//index.js
import TodoHeader from './TodoHeader/TodoHeader'
export{
    TodoHeader,
}
//或者
//export {default as TodoHeader} from './TodoHeader/TodoHeader'
//export {default as TodoInput} from './TodoInput/TodoInput'


然后App.js就直接引入index.js中暴露出来的组件
import {TodoHeader, TodoInput} from './components/index.js'
```

## 空元素组件Fragment

>render()函数里返回的只能有一个根元素,如果不想要根标签是一个元素

```
import React, { Component, Fragment } from 'react'

render() {
    return (
        <Fragment></Fragment>
        <></>  或者直接空标签
    )
}
```

## 组件的数据挂载props和prop-types

>函数式组件

```
App.js 父组件
<TodoHeader title="待办事项" x={1}>  //传递数字用{}
    <div>123</div>
</TodoHeader>

子组件
export default function TodoHeader(props) {
    return (
        <div>
            <h1>{props.title}</h1>     //显示‘待办事项’
            <h1>{props.children}</h1>  //<div>123</div>
        </div>
    )
}
```

>类组件

```
App.js 父组件
<TodoList desc="list">
    列表
</TodoList>

子组件
export default class TodoList extends Component {
    render() {
        return (
            <ul>
                <li>{this.props.desc}</li>  //输出'list'
                <li>{this.props.children}</li>  //'输出列表'
            </ul>
        )
    }
}
```

>prop-types

```
判断传入的参数是否为该组件需要的数据类型，或有没有传这个参数，
如果类型不正确那么会报错

npm包
npm install --save prop-types

import ProoTypes from 'prop-types'
```

函数式组件
```
父组件
<TodoHeader title="待办事项" x={1}>
    <div>123</div>
</TodoHeader>

子组件
import PropTypes from 'prop-types'
export default function TodoHeader(props) {
    return (
        <div>
            <h1>{props.title}</h1>
        </div>
    )
}

//需要接收参数的数据类型，isRequired为是否必传过来该组件
TodoHeader.propTypes = {
    title : PropTypes.string,
    x : PropTypes.number.isRequired
}

//对传过来的参数设默认值，如果没有传就使用这个值
TodoHeader.defaultProps = {
    title : '默认title'
}
```

类组件
```
<TodoInput btnTxt="add"/>

import React, { Component } from 'react'
import PropTypes from 'prop-types'

export default class TodoInput extends Component {
    static propTypes = {
        btnTxt : PropTypes.string.isRequired
    }

    //对传过来的参数设默认值，如果没有传就使用这个值
    static defaultProps = {   
        btnTxt: '添加'
    }

    render() {
        return (
            <div>
                <input type="text"></input>
                <button>{this.props.btnTxt}</button>
            </div>
        )
    }
}
```

>验证JSON数据

```
static propTypes = {
    参数 : PropTypes.arrayOf(PropTypes.shape({
        id : PropTypes.number.isRequired,
        title : PropTypes.string.isRequired,
        isCompleted : PropTypes.bool.isRequired
    })).isRequired
}
 
```


## 组件内部的数据--state

>只有类组件有state

```
export default class App extends Component {

    constructor(props){
        super(props);
        this.state = {
            title : '待办事项'
        }
    }

    render() {
        return ()
    }
}
```

>非受控组件和完全受控组件

```
完全受控组件
数据由父组件提供，自己不能修改该数据

非受控组件
数据由本身提供，可修改传递数据给子组件使用

半受控组件
```

## 模板渲染语法

```
循环列表
render() {
    const data = this.props.todos
    return (
        <ul>
            {data.map((item,index) => {
                return <li key={item.id}>{item.title}</li>
            })}
        </ul>
    )
}

渲染包含html的字符串
render() {
    return (
        <Fragment>
            {
                <div dangerouslySetInnerHTML={{__html: this.state.article}} />
            }
        </Fragment>
    )
}
```

## setState

>setState是异步的

```
fun = () => {
  //第一个参数为对象
  this.setState({
    key : value
  }, () => {
    //获取最新的state，应该在这个回调里获取
  })

  //第一个参数为方法,prevState为上次的状态值
  this.setState((prevState) => {
    return {
      key : value
    }
  }, () => {
    //获取最新的state，应该在这个回调里获取
  })
}
```