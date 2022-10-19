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

#### 4. 存取器

```javascript
  class Person {
    firstName: string = 'A'
    lastName: string = 'B'
    get fullName() {
      return this.firstName + '-' + this.lastName
    }
    set fullName(value) {
      const names = value.split('-')
      this.firstName = names[0]
      this.lastName = names[1]
    }
  }
  
  const p = new Person()
  console.log(p.fullName) // A-B
  
  p.firstName = 'C'
  p.lastName = 'D'
  console.log(p.fullName) // C-D
  
  p.fullName = 'E-F'
  console.log(p.firstName, p.lastName) // E-F
```

#### 5. 静态属性

> 静态成员:在类中通过static修饰的属性或者方法，就是静态属性和静态方法

**静态成员在使用的时候通过 `类名.`的这种语法来调用的**

```javascript
/*
静态属性, 是类对象的属性
非静态属性, 是类的实例对象的属性
*/

class Person {
  name1: string = 'A'
  // 静态属性
  static name2: string = 'B'

  // 构造函数是不能用static修饰的
  constructor(name: string) {
    this.name1 = name;
    // name2是静态属性，不能通过实例对象来调用
    Person.name2 = name;
  }

  // 静态方法
  static sayHi() {console.log('你好')}
}
// name2是静态属性，不能通过实例对象来调用
console.log(Person.name2)  // 'B'
console.log(new Person('张三').name1)  // '张三'

// 调用静态方法
Person.sayHi();
```

#### 6. 抽象类

> 抽象类做为其它派生类的基类使用。它们不能被实例化，为了让子类实例化及实现内部的抽象方法

> 抽象类: 抽象方法一般没有任何具体的内容实现，也可以包含实例方法

**`abstract` 关键字是用于定义抽象类和在抽象类内部定义抽象方法。**

```javascript
/*
抽象类
  不能创建实例对象, 只有实现类才能创建实例
  可以包含未实现的抽象方法
*/

abstract class Animal {
  abstract name: string
  // 抽象方法
  abstract cry()

  // 实例方法
  run() {
    console.log('run()')
  }
}

class Dog extends Animal {
  name: string = '张三' 
  // 重新实现抽象类中的方法
  cry() {
    console.log(' Dog cry()')
  }
}

const dog = new Dog()
dog.cry()
dog.run()
```

## 四、函数

#### 1. 函数类型

```javascript
// 函数声明式，命名函数
function add(x: number, y: number): number {
  return x + y
}

// 函数表达式，匿名函数
let myAdd = function(x: number, y: number): number {
  return x + y
}

// 完整写法
// let myAdd2: (x: number, y: number) => number = function(x: number, y: number): number {
//   return x + y
// }
```

#### 2. 默认参数和可选参数

```javascript
// 1. 参数列表直接赋值 为默认参数
// 2. 参数?: 为可选参数

function buildName(firstName: string = 'A', lastName?: string): string {
  if (lastName) {
    return firstName + '-' + lastName
  } else {
    return firstName
  }
}
console.log(buildName('C', 'D'))
console.log(buildName('C'))
console.log(buildName())  // 函数firstName参数有默认值，所以可以不传；lastName为可选参数，可以不传
```

#### 3. 剩余参数(rest参数)

> rest参数需要放在形参的最后

```
function info(x: string, ...args: string[]) {
  console.log(x, args)
}
info('abc', 'c', 'b', 'a')
```

#### 4. 函数重载

> 函数名相同, 而形参不同，形参个数不同的多个函数

```
// 重载函数声明
// 1. 如果不写该声明，调用函数时参数没有对应上，也会编译通过，但定义的函数有可能返回undefined
// 2. 如果写了该声明，调用函数时参数没有对应上，那么会编译不通过，提示: 没有与此调用匹配的重载的错误

function add(x: string, y: string): string
function add(x: number, y: number): number

// 定义函数实现
function add(x: string | number, y: string | number): string | number {
  // 在实现上我们要注意严格判断两个参数的类型是否相等，而不能简单的写一个 x + y
  if (typeof x === 'string' && typeof y === 'string') {
    return x + y
  } else if (typeof x === 'number' && typeof y === 'number') {
    return x + y
  }
}

console.log(add(1, 2))
console.log(add('a', 'b'))
// console.log(add(1, 'a')) // error
```

## 五、泛型

> 指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定具体类型的一种特性

**泛型写法可以在编写代码的时候，会有提示信息判断代码是否正确，并提示该值类型**

**如果填写any类型，代码在编写的时候就算有错误或调用不存在的方法，都不会有错误提示**

#### 1. 函数泛型

```javascript
function getArr<T>(value: T, count: number) : T[] {
  const arr: Array<T> = [];
  for(let i=0; i<count; i++) {
    arr.push(value);
  }
  return arr;
}

const arr1 = getArr<number>(123,3);
console.log(arr1[0].toFixed(2))
// console.log(arr1[0].split(""))  error

const arr2 = getArr<string>('123',3);
// console.log(arr2[0].toFixed(2))  error
console.log(arr2[0].split(""))
```

> 多个泛型参数的函数

```javascript
function swap<K, V>(a: K, b: V): [K, V] {
  return [a, b]
}
const result = swap<string, number>('abc', 123)
console.log(result[0].length, result[1].toFixed())
```

#### 2. 泛型接口

> 在定义接口时, 为接口中的属性或方法定义泛型类型，在使用接口时, 再指定具体的泛型类型

```javascript
  // 定义一个泛型接口
  interface IBaseCRUD<T> {
    data: Array<T>
    add: (t: T) => T 
    getUserById: (id: number) => T
  }

  // 定义一个用户信息的类
  class User {
    id?: number
    name: string
    age: number
    constructor(name: string, age: number) {
      this.name = name
      this.age = age
    }
  }

  class UserCRUD implements IBaseCRUD<User> {
    data: Array<User> = []
    // 重新实现接口方法
    // 添加用户方法
    add(user: User): User {
      user.id = Date.now() + Math.random()
      this.data.push(user);
      return user
    } 
    // 根据id获取用户信息
    getUserById(id: number): User {
      return this.data.find(user => user.id === id);
    }
  }
  const userCRUD: UserCRUD = new UserCRUD();
  userCRUD.add(new User('张三', 18));
  const {id} = userCRUD.add(new User('李四', 20));
  console.log(userCRUD.data);
  console.log(userCRUD.getUserById(id));
```

#### 3. 泛型类

> 在定义类时, 为类中的属性或方法定义泛型类型 在创建类的实例时, 再指定特定的泛型类型

```javascript
  // 定义一个类，类中的属性值类型不确定，方法中的参数及返回值的类型也不确定
  class GenericNumber<T> {
    defaultValue: T
    add: (x: T, y: T) => T
  }
  // 在实例化类的对象的时候，再确定泛型的类型
  const genericNumber: GenericNumber<number> = new GenericNumber<number>();
  genericNumber.defaultValue = 123;
  genericNumber.add = function(x, y) {
    return x + y;
  }
  console.log(genericNumber.add(genericNumber.defaultValue, 20))
```

#### 4. 泛型约束

> 如果我们直接对一个泛型参数取 length 属性, 会报错, 因为这个泛型根本就不知道它有这个属性

```javascript
// 没有泛型约束
function fn<T>(x: T): void {
  // console.log(x.length)  // error
}
```

> 可以定义一个接口来`泛型约束`

```javascript
// 用来约束将来的某个类型中必须要有length属性
interface Lengthwise {
  // 接口中有一个属性length
  length: number
}

// 指定泛型约束
function fn2<T extends Lengthwise>(x: T): void {
  console.log(x.length)
}

// 传入符合约束类型的值，必须包含必须 length 属性：
fn2('abc')
// fn2(123) // error  number没有length属性，传入的值需要有length属性，才满足Lengthwise接口约束
```

## 六、声明文件

> 当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能

**declare var 并没有真的定义一个变量，只是定义了全局变量 jQuery 的类型，仅仅会用于编译时的检查，在编译结果中会被删除**

> 1. 声明语句: 如果需要ts对新的语法进行检查, 需要要加载了对应的类型说明代码

```
declare var jQuery: (selector: string) => any;
```

> 2. 声明文件

```
1. 把声明语句放到一个单独的文件（jQuery.d.ts）中, ts会自动解析到项目中所有声明文件

2. 下载声明文件: npm install @types/jquery --save-dev
```

> 很多的第三方库都定义了`对应的声明文件库`, 库文件名一般为 @types/xxx, 可以在 https://www.npmjs.com/package/package 进行搜索

> 有的第三库在下载时就会自动下载对应的声明文件库(比如: webpack),有的可能需要单独下载(比如 jQuery/react)

## 七、内置对象

> 内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。

> 1. ECMAScript 的内置对象

```javascript
/* 1. ECMAScript 的内置对象 */
let b: Boolean = new Boolean(1)
let n: Number = new Number(true)  // 1，传入false或null为0，传入undefined为NaN
let s: String = new String('abc')
let d: Date = new Date()
let r: RegExp = /^1/
let e: Error = new Error('error message')
b = true
// let bb: boolean = new Boolean(2)  // error，改变了的类型不是基本数据类型
```

> 2. BOM 和 DOM 的内置对象 

`Window，Document，HTMLElement，DocumentFragment，Event，NodeList`

```javascript
const div: HTMLElement = document.getElementById('test')
const divs: NodeList = document.querySelectorAll('div')
document.addEventListener('click', (event: MouseEvent) => {
  console.dir(event.target)
})
const fragment: DocumentFragment = document.createDocumentFragment()
```