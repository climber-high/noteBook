### context

>createContext是react提供的一个用于跨组件传值的方法。

```
import React, { Component, createContext } from 'react'
import { render } from 'react-dom'

// createContext这个方法的结果是一个对象，里面有两个组件：Provider和Consumer
// Provider用于提供状态
// Consumer用于接收状态
const {
  Provider,
  Consumer: CounterConsumer // 解构出来重新赋值给一个CounterConsumer的组件
} = createContext()


// 封装一个基本的Provider, 因为直接使用Provider，不方便管理状态
class CounterProvider extends Component {
  constructor () {
    super()
    // 这里的状态就是共享的，任何CounterProvider的后代组件都可以通过CounterConsumer来接收这个值
    this.state = {
      count: 100
    }
  }
  // 这里的方法也会继续通过Provider共享下去
  incrementCount = () => {
    this.setState({
      count: this.state.count + 1
    })
  }
  decrementCount = () => {
    this.setState({
      count: this.state.count - 1
    })
  }
  render () {
    return (
      // 使用Provider这个组件，它必须要有一个value值，这个value里可以传递任何的数据。一般还是传递一个对象比较合理。
      <Provider value={{
        count: this.state.count,
        onIncrementCount: this.incrementCount,
        onDecrementCount: this.decrementCount
      }}>
        {this.props.children}  //后代组件
      </Provider>
    )
  }
}

// 定义一个Counter组件
class Counter extends Component {
  render () {
    return (
      // 使用CounterConsumer来接收count, 
      <CounterConsumer>
        {
          // 注意！注意！注意！ Consumer的children必须是一个方法， 这个方法有一个参数，这个参数就是Provider的value
          ({ count }) => {
            return <span>{count}</span>
          }
        }
      </CounterConsumer>
    )
  }
}

class CountBtn extends Component {
  render () {
    return (
      <CounterConsumer>
        {
          ({onIncrementCount, onDecrementCount}) => {
            const handler = this.props.type === 'increment' ? onIncrementCount : onDecrementCount
            return <button onClick={handler}>{this.props.children}</button>
          }
        }
      </CounterConsumer>
    )
  }
}

class App extends Component {
  render () {
    return (
      <>
        <CountBtn type="decrement">-</CountBtn>
        <Counter />
        <CountBtn type="increment">+</CountBtn>
      </>
    )
  }
}

render(
  <CounterProvider>
    <App />
  </CounterProvider>,
  document.querySelector('#root')
)
```

```
1.createContext这个方法的结果是一个对象，里面有两个组件：Provider用于提供状态,
Consumer用于接收状态,结构赋值出来。

2.然后封装一个基本的Provider，Consumer重新赋值给一个新的Consumer的组件

3.创建一个类封装Provider,设置状态、方法等，render函数将Provider组件返回并添加value属性，
传入对象，Provider组件里面传入{this.props.children}

4.想要其他组件使用封装好的状态方法，用定义好的新的Consumer的组件，将这个组件需要返回的jsx用
定义好的新的Consumer的组件包裹起来，Consumer组件要求里面是一个function,里面的形参就是Provider
组件传过来的参数，用对象封装起来，可以把需要的状态解构出来

5.用来封装Provider的类组件，要把要使用context的父元素包裹起来，其子孙后代组件都可以使用Provider
提供的state
```