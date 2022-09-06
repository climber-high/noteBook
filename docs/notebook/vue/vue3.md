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

>1. setup

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