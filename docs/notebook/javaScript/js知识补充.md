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

## 数组方法

>1.forEach(不返回新数组)

**forEach没有生成一个新的数组，而且无法通过return或break这样的语句跳出本次循环**

```
let arr = [1,2,3,4,5] 
let newarr =arr.forEach((item,index)=>{
    
})
```

>2.map(返回新数组)

**map不改变原数组，他可以返回一个新的数组，当遍历数组时，内部改变原数组的时候，不会改变新的数组，就是说新添加的元素不会进入循环里面**

```
let arr = [1,2,3,4,5] 
let newarr =arr.map(item=>{
    arr.push(10)
    return item*2
    
    //如果return item>2,则返回数组符合条件就true,不符合就false
    //[false, false, true, true, true]
}) 
console.log(arr)  //[1, 2, 3, 4, 5,10,10,10,10,10]
console.log(newarr) //[2, 4, 6, 8, 10]
```

>3.filter(返回符合条件的新数组)

**不会改变原数组，返回符合条件的数组**

```
let arr = [1,2,3,4,5] 
let newArr=arr.filter(item=>{
    return item>1
    
    //如果进行元素操作，则新数组跟就旧数组值一致
})
console.log(arr) //[1, 2, 3, 4, 5]
console.log(newArr)  //[2, 3, 4, 5]
```

>4.every(条件都满足返回true)

**every不会改变原数组，遍历数组的每一项，如果每一项都满足条件，则会返回true**

```
let arr = [1,2,3,4,5] 
let newArr=arr.every(item=>{
    return item>1
})
console.log(arr) //[1, 2, 3, 4, 5]
console.log(newArr) //false
```

>5.some(有一个条件满足则返回true)

**some不会改变原数组，遍历数组的每一项，如果任何一项满足条件，则会返回true**

```
let arr = [1,2,3,4,5] 
let newArr=arr.some(item=>{
    return item>1
})
console.log(arr) //[1, 2, 3, 4, 5]
console.log(newArr) //true
```

>6.find

**符合条件就返回第一个符合条件的数组元素**

```
let arr = [1,2,3,4,5] 
let newarr =arr.find(item=>{
    return item>2
    
    //如果对元素进行操作则只会返回第一个元素
}) 
console.log(newarr) //3
```

>7.reduce(返回数组的总和)

```
let arr = [1,2,3,4,5] 
let newarr =arr.reduce((prev, next)=>{
    return prev+next; //前一个数加后一个数，等于数组总和
}) 
console.log(newarr) //15
```

## 题目:

```
var a = {},b={key:'b'},c={key:'c'};
a[b]=123;
a[c]=456;
console.log(a[b]);

因为键名称只能是字符串，b/c单做键会调用toString得到的都是[object Object],
a[b],a[c]都等价于a["[object Object]"]，那不就是更新[object Object]这个键的值了

等于console.log(a["[object Object]"]);
```