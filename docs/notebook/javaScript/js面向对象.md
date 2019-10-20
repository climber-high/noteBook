### js面向对象

## 创建对象的三种方式

```
1.字面量方式:
var obj = {
    name : 'zs',
    age : 18
}

2.调用系统的构造函数
var obj = new Object();
obj.name = 'zs';

3.自定义构造函数方式(首字母大写)
function Person(name){
    this.name=name;
    this.eat=function () {
        console.log("吃饭");
    }
}
var per = new Person("张三");
```

## 工厂模式

```
function createObject(name){
    var obj = new Object();
    obj.name = name;
    return obj;
}
var obj = createObject('张三')
```

**工厂模式跟自定义构造函数区别**

```
共同点:都是函数，都可以创建对象，都可以传入参数

工厂模式:
函数名是小写，有返回值，函数里有new，new之后的对象是当前的对象，直接调用函数

自定义构造函数:
函数名大写，函数里没有new跟返回值，this是当前的对象，通过new的方式来创建对象
```

## 构造函数与实例对象的关系

```
function Person(name,age,sex){
    this.name=name;
    this.age=age;
    this.sex=sex;
    this.eat=function () {
        console.log("吃饭");
    }
}
var per = new Person("张三",18,"男");

判断这个对象是否这种数据类型:
1.
  console.log(per.__proto__.constructor == Person);
  console.log(per.constructor == Person);

2.
  console.log(per instanceof Person)
```

**关系**

1. 实例对象是通过构造函数来创建的--创建的过程叫实例化(通过构造函数实例化对象)
2. 判断对象是否这个数据类型(2种方式)
    - 通过构造器的方式 (实例对象.构造器==构造函数名字)
    - 对象 instanceof 构造函数名字(通常用这种方式判断)

## 原型

>作用

```
1.通过原型来添加方法，数据共享，节省内存空间
```

>概念

```
1. 实例对象中有 __proto__ 这个属性，叫原型，也是一个对象，这个属性是给浏览器使用，
不是标准的属性, __proto__ 可以叫原型对象
2.构造函数中有 prototype 这个属性，叫原型，也是一个对象，这个属性是给程序员使用，
是标准的属性, prototype 可以叫原型对象

* 实例对象的__proto__和构造函数中的prototype相等
var p1 = new Person("zs",18);
console.log(p1.__proto__==Person.prototype); //true

* 又因为实例对象是通过构造函数来创建的，构造函数中有原型对象prototype
* 实例对象的__proto__指向了构造函数的原型对象prototype
```