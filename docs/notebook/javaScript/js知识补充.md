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

## 深度克隆

>浅拷贝数据

```
基本数据类型:
拷贝后会生成一份新的数据，修改拷贝以后的数据不会影响原数据

对象/数组
拷贝后不会生成新的数据，而是拷贝是引用，修改拷贝以后的数据会影响原来的数据
```

例:

```
var obj1 = {
	name:"zs",
	age:"18",
	eat:function(){
		console.log(1);
	},
	car:[1,2,3]
}

var obj2 = {};

function extend(a,b){
	for(var key in a){
		b[key]=a[key];
	}
}

extend(obj1,obj2);
```

>拷贝数据的方法

```
1.直接赋值给一个变量  //浅拷贝，拷贝数据基本数据类型也会修改
2.Object.assign()  
3.Array.prototype.concat() //数据里的基本数据类型原数据不会被修改，只有对象数组会修改
4.Array.prototype.slice()	//数据里的基本数据类型原数据不会被修改，只有对象数组会修改

5.JSON.parse(JSON.stringify()) //数据中不能有函数，函数不能拷贝
//深拷贝(因为先转成字符串，基本数据类型修改后不会影响原数据)
```

>深拷贝数据

```
拷贝的时候生成新数据，修改拷贝以后的数据不会影响原数据
```

>typeof返回的数据类型

```
String,Number,Boolean,Undefined,Object,Function
```

例:

```
var obj1 = {
	car:[1,2,3]
}
var obj2 = {};
function extend(a,b){
	for(var key in a){
		var item = a[key];
//		console.log(Object.prototype.toString.call(item))
//		console.log(item)
		if(item instanceof Array){
			b[key]=[];
			extend(item,b[key]);
		}else{
			b[key]=item;
		}
		
	}
}
extend(obj1,obj2);


//定义检测数据类型的功能函数
function checkedType(target){
	return Object.prototype.toString.call(target).slice(8,-1)
}
//实现深度克隆(对象/数组)
function clone(target){
	//判断拷贝的数据类型
	//初始化变量result 成为最终克隆的数据
	let result,targetType = checkedType(target);
	if(targetType === 'Object'){
		result = {};
	}else if(targetType === 'Array'){
		result = [];
	}else{
		return target;
	}
	//遍历目标数据
	for(let i in target){
		//获取遍历数据结构的每一项值
		let value = target[i];
		//判断目标结构里的每一值是否存在对象/数组
		if(checkedType(value) === 'Object' || checkedType(value) === 'Array'){
			//继续遍历获取到的value的值
			result[i] = clone(value);
		}else{  //获取到的value值是基本的数据类型或者是函数
			result[i] = value;
		}
	}
	return result;
}
```

## new操作符

>new操作符具体干了什么呢

```
1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
2、属性和方法被加入到 this 引用的对象中。
3、新创建的对象由 this 所引用，并且最后隐式的返回 this。
```

## 防抖和节流

>防抖:当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次

```
function debounce(fn, wait) {    
    var timeout = null;    
    return function() {        
        if(timeout !== null)   clearTimeout(timeout);    //!== null可省略
        timeout = setTimeout(fn, wait);    
    }
}
// 处理函数
function handle() {    
    console.log(Math.random()); 
}
// 滚动事件
window.addEventListener('scroll', debounce(handle, 1000));
```

>节流:当持续触发事件时，保证一定时间段内只调用一次事件处理函数。

```
时间戳:

var throttle = function(func, delay) {            
　　var prev = Date.now();            
　　return function() {                
　　　　var context = this;                
　　　　var args = arguments;                
　　　　var now = Date.now();                
　　　　if (now - prev >= delay) {                    
　　　　　　func.apply(context, args);                    
　　　　　　prev = Date.now();                
　　　　}            
　　}        
}        
function handle() {            
　　console.log(Math.random());        
}        
window.addEventListener('scroll', throttle(handle, 1000));
```

```
定时器:

var throttle = function(func, delay) {            
    var timer = null;            
    return function() {                
        var context = this;               
        var args = arguments;                
        if (!timer) {                    
            timer = setTimeout(function() {                        
                func.apply(context, args);                        
                timer = null;                    
            }, delay);                
        }            
    }        
}        
function handle() {            
    console.log(Math.random());        
}        
window.addEventListener('scroll', throttle(handle, 1000));
```

```
时间戳+定时器：

更精确地，可以用时间戳+定时器，当第一次触发事件时马上执行事件处理函数，
最后一次触发事件后也还会执行一次事件处理函数

var throttle = function(func, delay) {     
    var timer = null;     
    var startTime = Date.now();     
    return function() {             
        var curTime = Date.now();             
        var remaining = delay - (curTime - startTime);             
        var context = this;             
        var args = arguments;             
        clearTimeout(timer);              
        if (remaining <= 0) {                    
            func.apply(context, args);                    
            startTime = Date.now();              
        } else {                    
            timer = setTimeout(func, remaining);              
        }      
    }
}
function handle() {      
    console.log(Math.random());
} 
window.addEventListener('scroll', throttle(handle, 1000));
```

## 不改变原数组跟改变原数组的方法

>不改变原数组方法

```
concat()
join()
slice()

indexOf()
lastindexOf()
```

>改变原数组方法

```
shift()
unshift()
pop()
push()
sort()
splice()
reverse()
```
