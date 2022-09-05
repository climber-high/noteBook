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

>main.js 引入的不再是Vue构造函数，引入的是一个名为createAppDe的工厂函数

>initOnSave:false 在配置文件关闭语法检查

```
import { createApp } from 'vue'
import App from './App.vue'

// 创建应用实例对象(里面的属性和方法比vue2少)
const app = createApp(App)  // vue3写法
// 挂载
app.mount('#app') 

new Vue({
    router: router,
    store: store,
    render: h => h(App)
}).$mount('#app');   // vue2写法
```