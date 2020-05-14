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

const initState = [{   //初始化状态
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

//创建reducer,固定写法是由两个参数，第一个为state并有一个初始值，第二个为action
export default (state = initState, action) => {
    //根据不同的action.type,做不同处理，每次返回一个新的state，返回类型要一样
    switch (action.type) {

        //一定要有default,当action不对时，就不处理，返回上次的state
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
export default combineReducers({  //导出合并后的reducer
    cart  //把多个reducer作为参数传入
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


第一种创建方式：写成一个对象，这是标准的action，但是，问题不方便传递动态参数
export const increment = {
    type: actionType.CART_AMOUNT_DECREMENT,
    payload: {
        id:123
    }
}

第二种创建方式：使用actionCreator，它是一个方法，返沪一个对象，这个对象才是真正的action
export const increment = (id) => {
    return {
        type: actionType.CART_AMOUNT_INCREMENT,
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
    () =>{  //里面是方法的调用，所以外面要套一层函数
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

>react-redux.js.org

>安装:cnpm i react-redux -S

>1.引入Provider组件，将<App />组件包裹起来，并且**必须传入store**属性

```
import { Provider } from 'react-redux'
import store from './store.js'

<Provider store={store}>
    <App />
</Provider>
```

>2.在任何子组件里都可以使用connect方法

```
import { connect } from 'react-redux'

export default connect()(CartList)
这样该组件的this.props就有dispatch函数，可以传入action中的方法并调用，
但是没有reducer中的状态

<button onClick={
    () =>{  //里面是方法的调用，所以外面要套一层函数
        this.props.dispatch(decrement(item.id))
    }
}>-</button>
```

>3.在connect()(CartList)第一个括号中传入一个函数

```
const mapStateToProps = (state) => {  //state实际上就是store.getState()的值
    //这里return了什么，在组件里就可以通过this.props来获取
    return {
        cartList: state.cart
    }
}

export default connect(mapStateToProps)(CartList该组件名)

mapStateToProps作用就是从store里把state注入到当前组件的props上

形参state就是reducer定义的state,可以直接使用(不需要再调用createStore下的getState方法了),
也不用再使用subscribe()也能监听state的变化来改变视图
```

```
import { increment, decrement } from '../../actions/cart'

可以将action中的方法，使用对象的形式传入到connect函数的第二个参数里面，
这个的主要作用是把action生成的方法注入到当前组件props上
export default connect(mapStateToProps,{ increment, decrement })(CartList)  //可以使用装饰器

就可以直接调用action里的方法
<button onClick={this.props.decrement.bind(this,item.id)}>-</button>
```

**其他创建的reducers，action，store按常规使用redux创建，react-redux只不过省了state需要层层传递的麻烦**

>简单流程:

```
1.创建reducers
2.合并reducers
3.createStore
4.Provider store={store}
5.connect(mapStateToProps)(YourComponent)
6.actionCreators
7.修改reducers
8.connect(mapStateToProps, {...actionCreators})(YourComponent)
```

## 异步action


