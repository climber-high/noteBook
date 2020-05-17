### immutable

>www.lodashjs.com

>react自带javaScript工具库lodash

```
cloneDeep深度克隆方法
import { cloneDeep } from 'lodash'

const state = {
    name: 'zs',
    courses: ['h5','java']
}
const newState = cloneDeep(state);
console.log(newState===state)
```

## immutable.js

>这个函式库能够**创建不可变的数据结构**，使开发者能更清楚的理解目前数据的状态，更能优化程序处理速度

>npm i immutable -S

`Map:`

```
import { Map } from 'immutable'
const state = {
    name: 'zs',
    courses: ['h5','java']
}

const newState = Map(state)
const newName = newState.set('name','ls'); //set方法返回新对象
console.log(newName.get('name'))  //重新获取新值
```


`List:`

```
import { List } from 'immutable'

const list1 = List([1,2]);
const list2 = list1.push(3,4,5);
console.log(list1.get(4),list2.get(4))
```

`fromJS:`

>用于复杂的数据结构时获取或修改值(getIn,setIn)

```
import { fromJS } from 'immutable'
const state = {
    name: 'zs',
    courses: ['h5','java']
}

const imState = fromJS(state)
console.log(imState.get('courses').get(0))
console.log(imState.getIn(['courses',0]))

const newImState = imState.setIn(['sexy'],'nan');
console.log(newImState.getIn(['sexy']));
```

>用于复杂的数据结构时操作值(updateIn)

```
import { fromJS } from 'immutable'
const state = {
    name: 'zs',
    courses: ['h5','java']
}

const imState = fromJS(state)
const newImState = imState.updateIn(['name'],v => v + '1' );
console.log(newImState.getIn(['name']));
```

`equals和is同样效果`

>判断两个数据结构的内容是否相同

```
const { Map, is } = require('immutable');
const map1 = Map({ a: 1, b: 2, c: 3 });
const map2 = Map({ a: 1, b: 2, c: 3 });

map1.equals(map2); // true
map1 === map2; // false

is(map1,map2) 
```

## redux-immutable

>cnpm i immutable redux-immutable -S

使用:
```
在合并reducers的index.js文件
import { combineReducers } from 'redux'
改成引用:
import { combineReducers } from 'redux-immutable'
```

>在reducer引入fromJS方法

```
import { fromJS } from 'immutable'

const initState = fromJS([{    //数据不会改变，只不过是复制一份重新操作
    id: 1,
    title: 'Apple',
    price: 8888,
    amount: 10
},{
    id: 2,
    title: 'Orange',
    price: 6666,
    amount: 12
}])

export default (state = initState, action) => {
    switch (action.type) {
        case actionType.CART_AMOUNT_INCREMENT:
            return state.map(item => {
                if(item.getIn(['id']) === action.payload.id) {
                    return item.update('amount',(v) => v+1)   //要把更新后的return出去
                }
                return item

            })
        case actionType.CART_AMOUNT_DECREMENT:
            return state.map(item => {
                if(item.getIn(['id']) === action.payload.id) {
                    return item.update('amount',v => v - 1)
                }
                return item
            })
        default:
            return state
    }
}
```

>在其他组件使用时，用getIn()等方法操作

```
import { connect } from 'react-redux'

const mapStateToProps = (state) => {
    return {
        cartList: state.getIn(['cart'])
    }
}
export default connect(mapStateToProps)(CartList)
```