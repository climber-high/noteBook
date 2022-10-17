### 常用语法

## 一、基础类型

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

## 二、接口

> TypeScript 的核心原则之一是对值所具有的结构进行类型检查。我们使用接口（Interfaces）来定义对象的类型

**接口是对象的状态(属性)和行为(方法)的抽象(描述)**

`类型检查器会查看对象内部的属性是否与 IPerson 接口描述一致, 如果不一致就会提示类型错误`

- 接口类型的对象:
  - 多了或者少了属性是不允许的
  - 可选属性: ?
  - 只读属性: readonly

```
// 定义人的接口, 限定或者约束该对象中的属性数据
interface IPerson {
  readonly id: number   // 1. 只读类型
  name: string
  age: number
  sex?: string    // 2. 可选属性
}

const person1: IPerson = {
  id: 1,
  name: 'tom',
  age: 20,
  // sex: '男'  // 可以没有
  // xxx: 12 // error 没有在接口中定义, 不能有
}
// person2.id = 2 // error，只读不能修改
```

#### 1. 函数类型

> 接口可以描述函数类型(参数的类型与返回的类型)

> 为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

```typescript
interface SearchFunc {
  (source: string, subString: string): boolean
}

const mySearch: SearchFunc = function(source: string, sub: string): boolean {
  return source.search(sub) > -1   // search()字符串内置方法，但没有indexOf(sub, 1)可以指定起始查找位置灵活
}

// 创建函数的 参数列表类型 和 返回值类型，可以省略
// const mySearch: SearchFunc = function(source, sub) {
//  return source.search(sub) > -1
// }

console.log(mySearch('abcd', 'bc'))
```

#### 2. 类 类型

> 一个`类`可以`实现多个`接口

```
interface Alarm {
  alert(): any
}

interface Light {
  lightOn(): void
  lightOff(): void
}

class Car implements Alarm, Light {
  // 实现接口，如果接口有方法，则类必须实现该接口方法
  alert() {
    console.log('Car alert')
  }
  lightOn(){}
  lightOff(){}
}
```

>一个`接口`可以`继承多个`接口，能够从一个接口里复制成员到另一个接口里

```
interface LightableAlarm extends Alarm, Light {}
```

## 三、类

```javascript
/*
类的基本定义与使用
*/

class Greeter {
  // 声明属性
  message: string

  // 构造方法，为了实例化对象的时候，可以直接对属性的值进行初始化
  constructor(message: string) {
    this.message = message
  }

  // 一般方法
  greet(): string {
    return 'Hello ' + this.message
  }
}

// 创建类的实例
const greeter = new Greeter('world')
// 调用实例的方法
console.log(greeter.greet())
```

#### 1. 继承，多态

> 类从基类中继承了属性和方法，派生类通常被称作子类，基类通常被称作超类。

> 类和类的继承关系，需要用到extends关键字

```
class Animal {
  run(distance: number) {
    console.log(`Animal run ${distance}m`)
  }
}

class Dog extends Animal {
  cry() {
    console.log('wang! wang!')
  }
}

const dog = new Dog()
dog.cry()
dog.run(100) // 可以调用从父中继承得到的方法
```

> 子类中可以调用父类中的构造函数，使用super关键字(也可以用super调父类实例方法)

```javascript
class Animal {
  name: string
  constructor(name: string) {
    this.name = name;
  }
  run(distance: number) {
    console.log(`Animal run ${distance}m`)
  }
}

class Dog extends Animal {
  constructor(name: string) {
    // 实例化对象的时候调用父类构造方法。子类的构造函数必须在访问this下的属性前，调用super()函数
    super(name); 
    super.run(2);
  }
  run(distance: number) {   // 子类 重写 父类方法
    console.log(666);
    super.run(distance);   // 子类 调用 父类run方法
  }
}
const dog:Dog = new Dog('小狗');   // 1. Animal run 2m
dog.run(3)    // 2. 666; 3. Animal run 3m

/* 如果子类型有扩展的方法, 不能让子类型引用指向父类型的实例 */
// const dog:Dog = new Animal('小狗');  error

// 对象的多态
function showRun(ant: Animal) {
  ant.run(3);
}
showRun(dog);
```

#### 2. 公共，私有与受保护的修饰符

> 用来描述类内部的属性/方法的可访问性

- public: `默认值`, 公开的外部也可以访问
- private: 只能`类内部`可以访问 `(实例化后获取不到类的内部属性)`
- protected: `类内部和子类`可以访问 `(实例化后获取不到类的内部属性)`

```javascript
class Animal {
  public name: string

  public constructor(name: string) {
    this.name = name
  }

  public run(distance: number = 0) {
    console.log(`${this.name} run ${distance}m`)
  }
}

class Person extends Animal {
  private age: number = 18
  protected sex: string = '男'

  run(distance: number = 5) {
    console.log('Person jumping...')
    super.run(distance)
  }
}

class Student extends Person {
  run(distance: number = 6) {
    console.log('Student jumping...')

    console.log(this.sex) // 子类能看到父类中受保护的成员
    // console.log(this.age) //  子类看不到父类中私有的成员

    super.run(distance)
  }
}

console.log(new Person('abc').name) // 公开的可见
// console.log(new Person('abc').sex) // 受保护的不可见
// console.log(new Person('abc').age) //  私有的不可见
```

#### 3. readonly修饰符

> 你可以使用`readonly`关键字将属性设置为只读的。 只读属性必须在声明时或构造函数里被初始化。

```
class Person {
  readonly name: string = 'abc'
  constructor(name: string) {
    this.name = name
    // 构造函数中，可以对只读的属性成员的数据进行修改
    this.name = '123'
  }
}

let john = new Person('John')
// john.name = 'peter' // error
```

> `参数属性`可以方便地让我们在一个地方定义并初始化一个成员

**构造函数中参数列表的参数,使用readonly修饰后，该参数name可以叫参数属性,该类就自动创建一个属性成员name，外部无法修改，但是能访问**

**readonly, public, private, protected进行修饰，都会自动添加该属性**

```javascript
class Person2 {
  // name: string = 'abc' 参数列表添加readonly修饰符，声明变量可以省略
  constructor(readonly name: string) {
    // this.name = name  也不需要手动初始化，外部也能够访问
  }
}

const p = new Person2('jack')
console.log(p.name)
```