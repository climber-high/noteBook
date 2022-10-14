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
let s: number = null | undefined (严格模式下会报错)
```

#### 5. 数组

```
1. 第一种: number[]
    let list1: number[] = [1, 2, 3]
    let list2: (number|string)[] = [1, 2, '3']

2. 第二种方式是使用数组泛型，Array<元素类型>：
    let list3: Array<number> = [1, 2, 3]
```

#### 6. 元组 Tuple

> 元组类型允许表示一个已知元素数量和类型的数组，`各元素的类型不必相同`

**如果某个值为其他数据类型，或比如应该数字类型的值，但使用字符串类型下的方法，则会报错**

```
let t1: [string, number] = ['hello', 10];

console.log(t1[1].substring(1)) // Error, 'number' 不存在 'substring' 方法
```

#### 7. 枚举

> 1. 枚举数值`默认从0开始`依次递增, 根据特定的名称得到对应的枚举数值

**枚举的元素可以为中文，不推荐**

```
enum Color {
  Red,
  Green,
  Blue,
  中文
}
let myColor: Color = Color.Green
console.log(myColor, Color.中文) // 1, 3
```

> 2. 默认情况下，从`0开始`为元素编号。 你也可以`手动的指定`成员的数值，没设置值的从上一个设置了值的开始自增

```
enum Color {
  Red = 1,
  Green,
  Blue
}
console.log(Color.Green) // 2
```

> 3. 当枚举的值为数字，可以通过枚举的值，可以获取枚举所对应的元素名(枚举的值为`字符串不可以获取`其元素名)

```
enum Color {
  Red = 1,
  Green,
  Blue
}
let colorName: string = Color[2]
console.log(colorName) // 'Green'
```

> 4. 如果前一个枚举的`类型为字符串`，则下一个的枚举`必须定义值`，不然会报错

```
enum Color {
    Red = '123',
    Green = 1,
    Blue = 2
}
```

> 5. 枚举实现的原理

```
enum Color {
    Red,
    Green,
    Blue
}

var Color;
(function (Color) {
    Color[Color["Red"] = 0] = "Red";
    Color[Color["Green"] = 1] = "Green";
    Color[Color["Blue"] = 2] = "Blue";
})(Color || (Color = {}));
```

#### 8. any

> 不清楚类型的变量指定一个类型

```
let notSure: any = 1;

// 当一个数组中要存多个数据，个数和类型不确定，可以使用
let list: any[] = [1, true, 'free']
```

#### 9. void

> `void`类型像是与`any类型相反`，它表示`没有任何类型`，当一个函数没有返回值时，通常其返回值类型是void

```
/* 表示没有任何类型, 一般用来说明函数的返回值不能是undefined和null之外的值 */
function fn(): void {
  console.log('fn()')
  // return undefined
  // return null  非严格模式下，可以返回null
  // return 1 // error
}
```

> 声明一个 void 类型的变量没有什么用，因为你只能为它赋予 `undefined` 和 `null`

```
let unusable: void = undefined;
```

#### 10. object

> object表示非原始类型，也就是除number，string，boolean之外的类型。

```
function fn2(obj: object): object {
  console.log('fn2()', obj)
  return {}
  // return undefined
  // return null
}
console.log(fn2(new String('abc')))
console.log(fn2(String))

// console.log(fn2('abc') // error
```

#### 11. 联合类型（Union Types）

> 表示取值可以为多种类型中的一种

```
function toString2(x: number | string): string {
  return x.toString()
}
```

#### 12. 类型断言(Type Assertion)

> 可以用来手动指定一个值的类型

- 语法:
 - 方式一: <类型>值
 - 方式二: 值 as 类型 ，tsx中只能用这种方式

 ```
function getLength(x: number | string) {
// 直接x.length会报错是因为，传入x不清楚是数字或者字符串(字符串才有长度属性)
  if ((<string>x).length) { // 所以指定x的字符串类型
    // 证明为字符串
    return (x as string).length
  } else {
    // 证明为数字
    return x.toString().length
  }
}
console.log(getLength('abcd'), getLength(1234))
 ```

#### 类型推断

> TS 会在没有明确的指定类型的时候推测出一个类型

**1.定义变量时赋值了, 推断为对应的类型**

```
let b9 = 123 // number
// b9 = 'abc' // error
```

**2.定义变量时没有赋值, 推断为any类型**

```
let b10 // any类型
b10 = 123
b10 = 'abc'
```