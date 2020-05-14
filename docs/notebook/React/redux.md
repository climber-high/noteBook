### redux

>www.bootcdn.cn  //redux

>redux.org.cn

>设计思想

```
1. web应用是一个状态机，视图与状态是一一对应的
2. 所有的状态，保存在一个对象里面(唯一数据源)
(flux和redux都不是必须和react搭配使用)
```

>Redux的使用的三大原则

```
1. 唯一的数据源
2. 状态是只读的(通过拷贝一份新状态来修改)
3. 数据的改变必须通过纯函数完成
```

#### 缺点

```
1.state修改不方便。每次修改都需要返回引用不同的对象，为了书写方便，不得不借助immutable这样的工具，
让调试不再直接可读(会有大量toJS的滥用导致的性能问题)

2.state很容易滥用。对程序而言，数据结构要比代码重要，但重视数据结构的人真心不多，
这会导致大部分的状态都会被放到state中，大部分组件都在connect store。

3.组件与store耦合度高，组件复用困难性能问题。

4.每次action会触发所有reducer的处理；每次state变更会导致所有connect的组件重新执行mapStateToProps，
这个函数写的不太理想或者组件没有对props的修改进行渲染判定会导致大量的组件重新render
```

## redux

>安装:npm i redux -S

使用:

>1.在src创建actions和reducers文件夹,再创建store.js文件

>2.在reducers文件夹创建cart.js的reducer

```
指定应用状态的变化如何响应 actions 并发送到 store 的，
记住 actions 只是描述了有事情发生了这一事实，并没有描述应用如何更新 state

const initState = [{
    id: 1,
    title: 'Apple',
    price: 8888,
    amount: 10
},{
    id: 2,
    title: 'Orange',
    price: 6666,
    amount: 12
}]

export default (state = initState, action) => {
    switch (action.type) {
        default:
            return state
    }
}
```

>3.在reducers文件夹创建index.js文件合并多个reducers

```
//使用combineReducers方法合并
import { combineReducers } from 'redux'
import cart from './cart'
export default combineReducers({
    cart
})
```

>4.在store.js创建store(Redux 应用只有一个单一的 store)

```
import { createStore } from 'redux'
import rootReducer from './reducers'  //引入合并reducers的文件

export default createStore(rootReducer)

提供 getState() 方法获取 state；
提供 dispatch(action) 方法更新 state；
通过 subscribe(listener) 注册监听器;
通过 subscribe(listener) 返回的函数注销监听器。
```

>5.在主文件入口引用store.js并把store传到App.js

```
import store from './store.js'

render(
    <App store={store}/>,
    document.querySelector('#root')
)
```

>6.在App.js引用CartList组件，再把参数传给子组件

```
<CartList store = {this.props.store}/>
```

>7.在CartList组件使用store的数据

```
constructor(){
    super()
    this.state = {
        cartList: []
    }
}
componentDidMount () {
    this.setState({
        cartList:this.props.store.getState().cart
    })
}
```

>8.创建action

```
在actions文件夹创建actionType.js，管理全部action

export default {
    CART_AMOUNT_INCREMENT: 'CART_AMOUNT_INCREMENT',
    CART_AMOUNT_DECREMENT: 'CART_AMOUNT_DECREMENT'
}

创建action,cart.js:
import actionType from './actionType'
export const increment = (id) => {
    return {
        type: actionType.CART_AMOUNT_INCREMENT,
        payload: {
            id
        }
    }
}

export const decrement = (id) => {
    return {
        type: actionType.CART_AMOUNT_DECREMENT,
        payload: {
            id
        }
    }
}
```

>9.在CartList组件,调用createStore()下的dispatch()函数，调用action的方法并把id传过去，
把action返回的type和payload传进dispatch里，reducer的第二个参数为这些数据

```
import { increment, decrement } from '../../actions/cart'

<button onClick={
    () =>{
        this.props.store.dispatch(decrement(item.id))
    }
}>-</button>
<span>{item.amount}</span>
<button onClick={
    () =>{
        this.props.store.dispatch(increment(item.id))
    }
}>+</button>

之后去action看decrement，increment方法是否被调用，参数是否传递过去
```

>10.之后在reducer进行状态的更改

```
import actionType from '../actions/actionType'

const initState = [{
    id: 1,
    title: 'Apple',
    price: 8888,
    amount: 10
},{
    id: 2,
    title: 'Orange',
    price: 6666,
    amount: 12
}]

export default (state = initState, action) => {  //action为CartList组件调用dispatch(*调用action方法返回的对象*)
    switch (action.type) {
        case actionType.CART_AMOUNT_INCREMENT:
            return state.map(item => {  //使用map返回新数组
                if(item.id === action.payload.id) {
                    item.amount += 1
                }
                return item  //把数据返回
            })
        case actionType.CART_AMOUNT_DECREMENT:
            return state.map(item => {
                if(item.id === action.payload.id) {
                    item.amount -= 1
                }
                return item
            })
        default:
            return state
    }
}
```

>11.在cartList组件调用createStore的subscribe()方法监听state的变化来改变视图

```
getState = () => {
    this.setState({
        cartList:this.props.store.getState().cart
    })
}
componentDidMount () {
    this.getState()
    this.props.store.subscribe(this.getState)
}
```

## React Redux

>安装:cnpm i react-redux -S