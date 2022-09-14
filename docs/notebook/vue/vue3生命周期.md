### vue3生命周期

> Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有两个被更名：

- `beforeDestroy`改名为`beforeUnmount`

- `destroyed`改名为`unmounted`

> Vue3.0和Vue2.x生命周期对比

- `Vue2.x`中new Vue(options)之后就执行beforeCreate()和created(), 之后判断`是否有el配置项`，没有的话再调$mount('#app')方法；有el配置项之后，就判断`是否有template配置项`，有的话就编译模板到渲染函数，没有的话编译el的innerHtml至模板

- `Vue3.0`首先创建app = createApp(App)并且挂载app.mount('#app')，之后再执行beforeCreate()和created()，(Vue3.0生命周期少了判断是否有el配置项)，之后就判断`是否有template配置项`。

> Vue3.0也提供了Composition API形式的生命周期钩子，与Vue2.x中钩子对应关系如下

- `beforeCreate` ===> `setup()`
- `created` ===> `setup()`
- `beforeMount` ===> `onBeforeMount`
- `mounted` ===> `onMounted`
- `beforeUpdate` ===> `onBeforeUpdate`
- `updated` ===> `onUpdated`
- `beforeUnmount` ===> `onBeforeUnmount`
- `unmounted` ===> `onUnmounted`

```javascript
import { onBeforeMount } from 'vue';
setup() {
    onBeforeMount(()=> {
        // 函数体
    })
}
```

>Vue3.0配置项生命周期钩子 和 Composition API形式的生命周期钩子执行顺序

```
1. 不管什么时候都先走setup()
2. beforeCreate()
3. created()
4. 之后先走组合式api的生命周期钩子onBeforeMount()
5. 再走配置项生命周期钩子beforeMount()
...
```