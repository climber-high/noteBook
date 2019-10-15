### Promise

>Promise是一个构造函数，可以new Promise()得到Promise的实例

### Promise有两个函数

```
resolve(成功后的回调函数)

reject(失败后的回调函数)
```

1. 在Promise构造函数的Prototype属性上有一个then方法
2. Promise表示一个异步操作，每当new 一个 Promise的实例，就表示一个具体的异步操作
3. 异步操作只有两种结果，执行**成功**或执行**失败**(需要在内部调用成功或失败的回调函数，把结果返回给调用者)
4. 因为是异步操作，**无法使用return**把操作的结果返回给调用者，只能使用**回调函数的形式返回**

例:
```
会立即调用Promise构造函数传递的方法

var promise = new Promise(function(){
    内部写具体的异步操作
})
```