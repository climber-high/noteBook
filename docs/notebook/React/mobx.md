### mobx

>https://cn.mobx.js.org

>简单、可扩展的状态管理

>1.安装装饰器(HOC高阶组件，装饰器模式第二种方式)

>2.安装mobx

```
cnpm i mobx mobx-react -S
```

`使用:`

>1. 创建store文件夹用于放置mobx文件index.js

```
import { observable, computed, action } from "mobx";

class Counter{
    name = 'Counter'        //不可观察的状态
    @observable count = 100   //可观察的状态
    @computed get doubleCount(){  //computed用于计算属性
        return this.count*2
    }
    @action.bound increment(){   //.bound改变this指向
        this.count += 1
    }
    @action.bound decrement(){
        this.count -= 1
    }
}

const counterStore = new Counter();
export default counterStore
```

>2. 导入新建的mobx文件，当参数传给子组件

```
import counterStore from './store'

class App extends Component {
    render(){
        return (
            <>
                {/*可以把counterStore实例出来里面的方法传给子组件*/}
                <CountBtn onClick={counterStore.decrement} type="decrement">-</CountBtn>
                <Counter counterStore={counterStore} />
                <CountBtn onClick={counterStore.increment} type="increment">+</CountBtn>
            </>
        )
    }
}
```

>3. Counter组件

```
import React, { Component } from 'react'
import { observer } from "mobx-react";   //导入observer装饰器

@observer  //装饰器下不能有export default
class Counter extends Component {
    render () {
        return(
            <> 
                <span>{this.props.counterStore.count}</span>
            </>
        )
    }
}

export default Counter
```

>4. CountBtn 组件调用传过来的方法

```
import React, { Component } from 'react'

class CountBtn extends Component {
    render () {
        return(
            <>
                <button onClick={this.props.onClick}>{this.props.children}</button>
            </>
        ) 
    }
}

export default CountBtn
```

## mobx-react中的Provider组件和inject装饰器写法

>1. 创建store文件夹用于放置mobx文件index.js

```
import { observable, computed, action } from "mobx";

class Counter{
    name = 'Counter'        //不可观察的状态
    @observable count = 100   //可观察的状态
    @computed get doubleCount(){  //computed用于计算属性
        return this.count*2
    }
    @action.bound increment(){   //.bound改变this指向
        this.count += 1
    }
    @action.bound decrement(){
        this.count -= 1
    }
}

const counterStore = new Counter();
export default counterStore
```

>2. 在渲染App组件的App.js文件导入Provider组件

```
import { observer, Provider, inject } from "mobx-react";
import counterStore from './store'

<Provider counter={counterStore}>
    <App />
</Provider>

将App组件用Provider组件包裹起来，传入mobx文件
```

>3. 添加inject装饰器传入在Provider组件定义的参数，就可以使用该参数

```
import { observer, inject } from "mobx-react";

@inject('counter')
@observer
class App extends Component {
    render(){
        console.log(this.props)
        return (
            <>
                <CountBtn onClick={this.props.counter.decrement} type="decrement">-</CountBtn>
                <Counter />
                <CountBtn onClick={this.props.counter.increment} type="increment">+</CountBtn>
            </>
        )
    }
}
```

>4. 在任何组件都可以使用Provider组件定义的参数

```
import React, { Component } from 'react'
import { observer, inject } from "mobx-react";

@inject((store) => {     //store为Provider组件传入的参数，@inject可以传入一个方法
    return {
        count:store.counter.count,
        doubleCount:store.counter.doubleCount
    }
})
@observer
class Counter extends Component {
    render () {
        return(
            <> 
                <span>{this.props.count}</span>
                <span>{this.props.doubleCount}</span>
            </>
        )
    }
}

export default Counter
```