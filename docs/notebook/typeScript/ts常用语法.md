### 常用语法

## 基础类型

> ts中变量一开始是什么类型，那么后期`赋值的时候`，只能用`这个类型的数据`，是不允许用其他类型的数据赋值给当前的这个变量

#### 1. 布尔值

```
let isDone: boolean = false
```

#### 2. 数字

```
let a1: number = 10 // 十进制
let a2: number = 0b1010 // 二进制
let a3: number = 0o12 // 八进制
let a4: number = 0xa // 十六进制
```

#### 3. 字符串

```
let name: string = 'tom'
```

#### 4. undefined 和 null

> 默认情况下 `null` 和 `undefined` 是所有类型的子类型。 就是说你可以把 null 和 undefined 赋值给其他类型的变量。

```
let u: undefined = undefined
let n: null = null
let s: number = null | undefined
```

#### 5. 数组

```
1. 第一种: number[]
    let list1: number[] = [1, 2, 3]

2. 第二种方式是使用数组泛型，Array<元素类型>：
    let list2: Array<number> = [1, 2, 3]
```

#### 6. 元组 Tuple

> 元组类型允许表示一个已知元素数量和类型的数组，`各元素的类型不必相同`

**如果某个值为其他数据类型，或比如应该数字类型的值，但使用字符串类型下的方法，则会报错**

```
let t1: [string, number] = ['hello', 10];

console.log(t1[1].substring(1)) // Error, 'number' 不存在 'substring' 方法
```