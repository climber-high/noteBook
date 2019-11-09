### js知识补充

## Object.keys()和Object.values()

>返回一个表示给定对象的所有可枚举`属性`或`值`的`字符串数组`

```
const obj = {
		a:1,
		b:2,
		c:3
	}

console.log(Object.keys(obj))  //["a", "b", "c"]
```

## for in和for of区别

>for ... in 循环返回的值都是数据结构的`键名`(适合遍历对象)

**遍历对象返回的对象的key值,遍历数组返回的数组的下标(key)**

```
const obj = {
		a:1,
		b:2,
		c:3
	}

	for(let i in obj){
		console.log(i)  //a,b,c
	}
```

>for of循环用来获取一对键值对中的`值`,而 for in 获取的是`键名`

**1.一个数据结构只要部署了 Symbol.iterator 属性, 就被视为具有 iterator接口, 就可以使用 for of循环**

**2.对象中没有 Symbol.iterator这个属性,所以使用 for of会报 obj is not iterable**

**3.它可以与 break、continue和return 配合使用,也就是说 for of 循环可以随时退出循环**

```
只要有 iterator 接口的数据结构,都可以使用 for of循环。

数组 Array
Map
Set
String
arguments对象
Nodelist对象, 就是获取的dom列表集合
```

>让对象也可以用for of遍历

```
const obj = {
		a:1,
		b:2,
		c:3
	}

	//Object.keys(obj) 返回对象的键名以数组形式['a','b','c']
	for(let i of Object.keys(obj)){
		console.log(i)  //a,b,c
		console.log(obj[i])  //1,2,3
	}
```