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

### 目录结构

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

### 常用 Composition API

>1. setup函数

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

>2. ref函数

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

>3. reactive函数

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
    - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性，相当于`this.$attrs`
    - slots: 收到的插槽内容，相当于`this.$slots`
    - emit: 分发自定义时间的函数，相当于`this.$emit`