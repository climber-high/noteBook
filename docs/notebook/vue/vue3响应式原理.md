## vue3.0的响应式原理

### vue2.x的响应式

- 实现原理
    - 对象类型：通过Object.defineProperty()对属性的读取，修改进行拦截（数据劫持）
    - 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）
- 存在问题(`vue3的reactive()不会有这些问题`)：
    - `新增对象`里没有的属性`(this.obj.key = value)`、`删除`属性`(delete this.obj.key)`，界面`不会更新`。
    ```
        解决方法
        1. 新增：this.$set(this.obj,'key','value') / Vue.set()
        2. 删除：this.$delete(this.obj,'key','value') / Vue.delete()
    ```
    - 直接通过下标修改数组`(this.obj.arr[0] = value)`，界面`不会自动更新`
    ```
        解决方法
        1. this.$set(this.obj.arr, 0, 'value')
        2. this.obj.arr.splice(0, 1, 'value')
    ```

### vue3.0的响应式

- 实现原理：
    - 通过Proxy(代理)：拦截对象中任意属性的变化，包括：属性值的读写、属性的添加、属性的删除等
    - 通过Reflect(反射)：对被代理对象(源对象)的属性进行操作

```javascript
let person = {
    name: '张三',
    age: 18
}

// p代理对象，person源对象
const p = new Proxy(person, {
    get(target, propName) {
        // 读取数据时调用
        // return target[propName];
        return Reflect.get(target, propName)

    },
    set(target, propName, value) {
        // 监听新增或修改，然后修改属性
        // target[propName] = value;
        return Reflect.set(target, propName, value)
    },
    deleteProperty(target, propName) {
        // 监听删除，然后删除
        // return delete target[propName];
        return Reflect.deleteProperty(target, propName)
    }
})
```