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

>8.通过Action创建函数来修改reducers里面的状态

```
在actions文件夹创建actionType.js

export default {
    CART_AMOUNT_INCREMENT: 'CART_AMOUNT_INCREMENT',
    CART_AMOUNT_DECREMENT: 'CART_AMOUNT_DECREMENT'
}
```