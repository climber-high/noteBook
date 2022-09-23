## vue3

#### 1. 性能的提升

```
1. 打包减少41%
2. 初次渲染快55%,更新渲染快133%
3. 内存减少54%
```

#### 2. 源码的升级

```
1. 使用Proxy代替defineProperty实现响应式
2. 重写虚拟dom的实现，Tree-Shaking
```

#### 3. 更好的支持TypeScript

#### 4 新的特性

  1. Composition API (组合API)
    - setup配置
    - ref与reactive
    - watch与watchEffect
    - provide与inject
    - ......

  2. 新的内置组件
    - Fragment
    - Teleport
    - Suspense

  3. 其他改变
    - 新的生命周期钩子
    - data选项应始终被声明为一个函数
    - 移除keyCode支持作为v-on的修饰符
    - ......

### 创建Vue3.0项目

>1. 使用vue-cli创建

```
1. 查看@vue/cli版本: vue --version (版本在4.5.0以上)
2. 安装: npm install -g @vue/cli
3. 创建: vue create <project-name>
4. 启动: npm run serve
```

>2. 使用vite创建

vite官网: https://vitejs.cn

  - 新一代前端构建工具
  - 优势如下:
    - 开发环境中，无需打包操作，可快速的冷启动。
    - 轻量快速的热重载(HMR)。
    - 真正的按需编译，不再等待整个应该编译完成。

```
1. 创建项目: npm init vite-app <project-name>
2. 安装依赖: npm install
3. 运行: npm run dev
```

## 目录结构

>initOnSave:false 在配置文件关闭语法检查, 无效则需要在package.json删除"eslint:recommended", 修改后重启

>main.js 引入的不再是Vue构造函数，引入的是一个名为createAppDe的工厂函数

```
import { createApp } from 'vue'
import App from './App.vue'

// 创建应用实例对象(里面的属性和方法比vue2少)
const app = createApp(App)  // vue3写法

// 挂载
app.mount('#app') 

// 卸载
setTimeout(() => {
    app.unmount('#app')
}, 1000)


// vue2写法 (在vue3里不能用这种写法)
new Vue({
    router: router,
    store: store,
    render: h => h(App)
}).$mount('#app');
```

>App.vue的改变

```
1. template里不需要再有 根标签
```

## 常用 Composition API

#### 1. setup函数

```
1. Vue3.0中一个新的配置项，值为一个函数 setup(){}
2. 用到Composition API就需要setup
3. 组件中所用到的：数据、方法等待，均要配置在setup中
4. setup函数的两种返回值：
   - 若返回一个对象，则对象中的属性、方法，在模板中均可以直接使用
    setup(){
      return {name, age, hello}
    }

   - 若返回一个渲染函数，则可以自定义渲染内容
     import { h } from "vue"; // 渲染函数
     setup(){
      return () => { return h('h1', '张三') }
    }
5. vue3项目可以兼容vue2代码，vue2写法可以读取vue3里setup()里定义的属性和方法，
但vue3不能访问vue2里的属性和方法，vue2和vue3如果有相同的属性，vue3的属性值优先
6. 注意点：
    1. 尽量不要与vue2配置混用
      - vue2配置(data, methos, computed)中 可以访问 到setup中的属性、方法等待
      - 但setup中 不能访问到 vue2配置(data, methos, computed)
      - 如果有重名，setup优先
    2. setup不能是一个async函数，因为返回值不再是return的对象，而是promise, 模板看不到return对象中的属性
```

```vue
  <template>
    <div>{{name}}</div>
    <div>{{age}}</div>
    <div @click="hello">alert</div>
  </template>

  <script>
    export default {
      name: 'App',
      setup(){
        let name = '张三';
        let age = 18;
        function hello() {
          alert(`姓名:${name}`);
        }
        return {name, age, hello};
      }
    }
  </script>
```

#### 2. ref函数

- 作用: 定义一个响应式的数据
- 语法: `const xxx = ref(initValue)`
    - 创建一个包含响应式数据的`引用对象（reference对象）`
    - js中操作数据：`xxx.value`
    - 模板中读取数据：不需要value，直接`<div>{{xxx}}</div>`
- 备注:
    - 接收的数据可以是：基本类型、也可以是对象类型。
    - 基本类型的数据：响应式依然是靠`Object.defineProperty()`的`get`与`set`完成的。
    - 对象类型的数据：内部使用了vue3中的一个新函数---`reactive`函数(底层Proxy函数)

```vue
<template>
  <div>{{name}}</div>
  <div>{{age}}</div>
  <div>{{msg.sex}}</div>
  <button @click="changeInfo">按钮</button>
</template>

<script>
  import {ref} from 'vue';
  export default {
    name: 'App',
    setup(){
      let name = ref('张三');
      let age = ref(18);
      let msg = ref({
        sex: '男'
      })
      function changeInfo() {
        name.value = '李四';
        age.value = 20;
        msg.value.sex = '女'
      }
      return {name, age, changeInfo, msg}
    }
  }
</script>
```

- 可以获取html元素

```
<div ref="hello">123</div>

const hello = ref();
onMounted(() => {
  console.log(hello.value);
})
```

#### 3. reactive函数

- 作用：定义一个`对象类型`的响应式数据(基本类型不要用它，要用`ref`函数)
- 语法：`const 代理对象 = reactive(源对象)`接收一个对象(或数组)，返回一个`代理对象(proxy的实例对象,简称proxy对象)`
- reactive定义的响应式数据是深层次的。
- 内部基于ES6的Proxy实现，通过代理对象操作源对象内部数据进行操作。

```vue
<template>
  <div>{{person.name}}</div>
  <div>{{person.age}}</div>
  <div>{{person.msg.sex}}</div>
  <button @click="changeInfo">按钮</button>
</template>

<script>
  import {reactive} from 'vue';
  export default {
    name: 'App',
    setup(){
      let person = reactive({
        name: '张三',
        age: 18,
        msg : {
          sex: '男'
        }
      })

      function changeInfo() {
        person.name = '李四';
        person.age = 20;
        person.msg.sex = '女';
      }
      return {person, changeInfo}
    }
  }
</script>
```

#### 4. computed函数

>与vue2中computed配置功能一致`(都是实时计算值)`

> `vue2写法`，computed为`对象形式`

```javascript
computed: {
    msg() {
        return this.person.name + ":" + this.person.age;
    }
}
```

> `vue3写法`，computed为`函数形式`

```javascript
import {reactive, computed} from 'vue';

setup(){
    let person = reactive({
        name: '张三',
        age: 18
    })
    // 简写
    // let msg = computed(() => {
    //     return person.name + ":" + person.age
    // })
    // 完整写法
    let msg = computed({
        get() {
            return person.name + ":" + person.age
        },
        set(value) {
            const msg = value.split(":");
            person.name = msg[0];
            person.age = msg[1];
        }
    }) 
    return {person,msg}
}
```

#### 5. watch函数

> `vue2写法`，watch为`对象形式`

```javascript
watch: {
    // 简写
    // sum(newVal, oldVal) {
    //     console.log(newVal, oldVal)
    // }

    // 完整写法
    sum: {
        immediate: true,  // 立即监听
        deep: true,  // 监听深层属性
        handler(newVal, oldVal) {
            console.log(newVal, oldVal)
        }
    }
},
```

> `vue3写法`，watch为`函数形式`

- 1. 监听`ref()`定义的数据

```javascript
import {ref, watch} from 'vue';

setup(){
    let sum =ref(0);
    let msg = ref('你好');
    let person = ref({
        name: '张三',
        age: 18,
        job: {
            j1: {
                salary: 20
            }
        }
    })
    // 监听一个数据
    watch(sum, (newVal, oldVal) => {
        console.log(newVal, oldVal)
    }, {immediate: true});

    // 监听多个数据
    watch([sum, msg], (newVal, oldVal) => {
        // newVal和oldVal也是以数组形式返回
        console.log(newVal, oldVal)
    }, {immediate: true});

    // 监听用ref定义的对象
    watch(person, (newVal, oldVal) => {
        // 这里监听的是引用对象RefImpl，浅层的内存地址，可以用deep监听深层值的变化
        console.log(newVal, oldVal)
    }, {deep: true});
    // 或
    watch(person.value, (newVal, oldVal) => {
       // 这里监听的是代理对象Proxy，可以直接监听属性值的变化
        console.log(newVal, oldVal)
    });

    return {sum, msg}
}
```

- 2. 监听`reactive()`定义的`整个`数据

  - **vue3的坑，监听值的变化，newVal, oldVal所打印出来的都是新变化的值**

  - **reactive()强制开启的深度监视(deep配置无效)**

```javascript
let person = reactive({
    name: '张三',
    age: 18
})
watch(person, (newVal, oldVal) => {
    console.log(newVal, oldVal)
})
```

- 3. 监听`reactive()`定义的`单个`数据

```javascript
let person = reactive({
    name: '张三',
    age: 18
})
watch(() => person.age, (newVal, oldVal) => {
    console.log(newVal, oldVal)
})
```

- 4. 监听`reactive()`定义的`某些`数据

```javascript
let person = reactive({
    name: '张三',
    age: 18
})
watch([() => person.age, () => person.name], (newVal, oldVal) => {
    console.log(newVal, oldVal)
})
```

- 5. 监听`reactive()`定义的`里面某个对象`数据

  - **如果修改了深层次的属性salary，想要监听外层就能监听到内层数据的变化，就要加上deep:true**

  - **该情况也是，监听值的变化，newVal, oldVal所打印出来的都是新变化的值**

```javascript
<template>
    薪资: {{person.job.j1.salary}}
    <button @click="person.job.j1.salary++">修改薪资</button>
</template>

let person = reactive({
    name: '张三',
    age: 18,
    job: {
      j1: {
        salary: 20
      }
    }
})
watch(() => person.job, (newVal, oldVal) => {
    console.log(newVal, oldVal)
}, {deep:true})
```

#### 6. watchEffect函数

- watch：既要指明监视的属性，也要指明监视的回调。
- watchEffect：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。
- watchEffect有点像computed
  - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
  - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

```javascript
import {watchEffect} from 'vue';

//watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
watchEffect(() => {
  const x1 = sum.value;
  const x2 = person.age;
  console.log('watchEffect配置的回调执行了')
})
```

#### 7. toRef函数

**说明：就是把源对象复制一份出来，当修改新对象的值时候，旧的对象也会跟着改变，并且是响应式对象数据**

- 作用：创建一个ref对象，其value值指向另一个对象中的某个属性。
- 语法：`const name = toRef(person, 'name')`
- 应用：要将响应式对象中的某个属性单独提供给外部使用时。
- 扩展：`toRefs`与`toRef`功能一致，但可以批量创建多个ref对象，语法：`const person = toRefs(person)`

```javascript
<template>
    年龄: {{person.age}}
    姓名: {{name}}
    薪资: {{job.j1.salary}}
</template>

setup(){
    let person = reactive({
        name: '张三',
        age: 18,
        job: {
            j1: {
                salary: 20
            }
        }
    })
    return {
      person,
      name: toRef(person.name),
      ...toRefs(person)  // 返回person下的第一层属性
    }
}
```

#### 8. 自定义hook函数

- 什么是hook? 本质上是一个函数，把setup函数中使用的Composition API进行了封装
- 类似于vue2.x中的mixin。
- 自定义hook的优势：复用代码，让setup中的逻辑更清楚易懂

```javascript
1. 创建新文件夹hooks放hook函数，文件命名通常useXxx.js
import {reactive,onMounted,onBeforeUnmount} from 'vue';
export default function(){
    let ponit = reactive({
        x:0,
        y:0
    })
    function clickFun(event){
        ponit.x = event.pageX;
        ponit.y = event.pageY;
        console.log(event.pageX, event.pageY);
    }
    onMounted(() => {
        window.addEventListener('click', clickFun);
    })
    onBeforeUnmount(() => {
        window.removeEventListener('click', clickFun);
    })
    return ponit;
}

2. 使用
import usePoint(自定义方法名usePoint) from '../hooks/usePoint'
export default {
    setup(){
        let ponit = usePoint();
        return {ponit}
    }
}
```

## 其他 Composition API

#### 1.shallowReactive 与 shallowRef (浅层响应式，但数据有变化，页面不更新)

- shallowReactive: 只处理`对象最外层`属性的响应式（浅响应式）
- shallowRef: 只处理`基本数据类型`的响应式，不进行对象的响应式处理
- 什么时候使用？
  - 如果有一个对象数据，结构比较深，但变化时只是外层属性变化 ===> shallowReactive。
  - 如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

#### 2.readonly 与 shallowReadonly (只读，修改数据不改变，并且页面不更新)

> 当只读的数据发生修改，页面会有警告提示只读

- readonly: 让一个响应式数据变为只读的`（深只读）`
- shallowReadonly: 让一个响应式数据变为只读的`（浅只读，第一层只读）`
- 应用场景: 不希望数据被修改时。(有可能是别人提供的数据，但又不能修改，修改后会有影响，就可以用该api，就算修改了数据也不会改变)
- readonly与shallowReadonly，`接收响应式数据`参数，并且要`重新赋值给原变量`

```javascript
页面数据没有变化有两个情况：
1. 目标数据已经改变，但页面展示没有更新（数据不是响应式）
2. 目标数据没有发生变化。readonly()就是让数据只读，不让修改


let person = reactive({
    name: '张三',
    age: 18,
    job: {
        j1: {
            salary: 20
        }
    }
})
person = readonly(person);
```
 
#### 3.toRaw 与 markRaw

- toRaw:
  - 作用：将一个由`reactive`生成的`响应式对象`转为`普通对象`
  - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。

- markRaw:
  - 作用：标记一个对象，使其永远不会再成为响应式对象。
  - 应用场景：
    - 1.有些值不应该被设置为响应式的，例如复杂的第三方类库等。
    - 2.当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

```javascript
比如在一个已有的响应式对象中再添加一个普通对象
let person = reactive({
  name: '张三'
})

let msg = {age: 18, sex: '男'};
person.msg = markRaw(msg);  // 标记其不会再成为响应式对象
```

#### 4.customRef

- 作用：创建一个自定义的ref，并对其`依赖项跟踪`和`更新触发`进行显示控制。

```javascript
<template>
    <input type="text" v-model="ketWord"/>
    {{ ketWord }}
</template>
  
<script>
import {customRef} from 'vue';
export default {
    name: 'DemoWorld',
    setup(){
        let timer;
        // 自定义一个ref
        function myRef(value, delay) {
            return customRef((track, trigger) => {
                return {
                    get() {
                        track(); // 3. 通知Vue追踪数据的变化，然后更新
                        return value;
                    },
                    set(newValue) {
                        clearTimeout(timer); // 防抖
                        timer = setTimeout(() => {
                            value = newValue; // 1. 把新的值，重新赋值给传进来的value
                            trigger(); // 2. 通知Vue去重新解析模板
                        }, delay)
                    }
                }
            });
        }
        let ketWord = myRef('hello', 500)
        return {ketWord}
    }
}
</script>
```


### reactive对比ref

- 从定义数据角度对比：
  - ref用来定义：`基本类型数据`
  - reactive用来定义：`对象（或数组）类型数据`
  - 备注：ref也可以用来定义`对象（或数组）类型数据`，它内部会自动通过`reactive`转为`代理对象`
- 从原理角度对比：
  - ref通过`Object.defineProperty()`的`get`与`set`来实现响应式（数据劫持）。
  - reactive通过使用`Proxy`来实现响应式（数据劫持），并通过Reflect操作`源对象`内部的数据。
- 从使用角度对比：
  - ref定义的数据：操作数据`需要`.value，读取数据时模板中直接读取`不需要`.value
  - reactive定义的数据：操作数据与读取数据：`均不需要`.value

### setup的两个注意点

- setup执行的时机
  - 在beforeCreate之前执行一次，this是undefined。
- setup的参数
  - props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
  - context：上下文对象
    - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性，相当于`this.$attrs`(在props配置了传过来的参数，则attrs就会没有值；反之在props没配置该属性，则attrs就会有该值)
    - slots: 收到的插槽内容，相当于`this.$slots`
    - emit: 分发自定义时间的函数，相当于`this.$emit`

```vue
  父组件:
  <template>
    <DemoWorld @hello="showMsg" msg="123">
      // vue3使用 v-slot:slotName 方式，vue2也可以slot=""方式
      <template v-slot:slotName>
        <span>具名插槽</span>
      </template>
    </DemoWorld>
  </template>
  <script>
    import DemoWorld from './components/DemoWorld'
    export default {
      name: 'App',
      // 注册组件
      components: {DemoWorld},
      setup() {
        // 绑定事件
        function showMsg(value) { alert(value) }
        return {showMsg}
      }
  </script>

   子组件:
   <template>
    <button @click="test">按钮</button>
    // 展示具名插槽
    <slot name="slotName"/>
   </template>
   <script>
    export default {
        name: 'DemoWorld',
        // 需要获取父组件传过来的参数，不然会警告没有使用这些参数
        props:['msg'],
        // vue3新增的配置项，不加该配置会有警告
        emits:['hello'],
        setup(props, context){
            console.log(props)  // Proxy {msg: '123'}  响应式数据
            // 调用父组件方法
            function test() { context.emit('hello',666) }
            return {test}
        }
    }
   </script>
  ```