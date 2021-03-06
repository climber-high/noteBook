### JS基础

## break与continue的区别


| break | continue |
| ----- | ----- |
| 立即终止循环 | 终止当前循环 |

##### 例
```
var i=0
while(i<10){
	i++;
	if(i==5){
		break;/*立即终止循环*/
		continue;//终止的当前循环
	}
	console.log(i);
}
```

## 函数
```
for(var i=0;...){
	具体执行的代码
}

定义函数
function 函数名 (a,b){
	具体执行的代码
}
如果要执行函数里面的代码,就需要调用函数
(就是叫一下他的函数)
函数名()
```

### 函数特点
函数的特点:封装,重用,(多态)
```
function wind(a){}

参数a(形参)
```

### return关键字
	1.return以下的代码都不会执行
	2.函数的返回值可以通过reture 来输出
```
function a(){
	var b=1;
	return b
}
a()
```

### 作用域
	1.全局作用域：在任何地方都可以使用
	2.局部作用域：在函数内的区域


### 变量
1.全局变量：在任何地方都可以使用
2.局部变量：只存在于对它做出声明,仅限于*函数内部*

## 事件冒泡和事件捕获

1. 事件冒泡：当事件到达目标节点后，会沿着捕获阶段的路线原路返回。**所有经过的节点,都会触发对应的事件**

```
如果父子元素 都 注册同一个事件，并且点击子元素，会先执行子元素事件的方法，
再执行父元素事件的方法，如果直接点父元素，只会执行父元素方法。
```

2. 事件捕获：当一个事件触发后,从Window对象触发,不断经过下级节点,直到目标节点。在事件到达目标节点之前的过程就是捕获阶段。**所有经过的节点,都会触发对应的事件**

```
如果父子元素 都 注册同一个事件，并为父元素的第三个参数改为true，
并且点击子元素，会先执行父元素事件的方法，再执行子元素事件的方法
```

## 事件委托(事件代理)

>优点:

1. 提高性能:每一个函数都会占用内存空间，只需添加一个事件处理程序代理所有事件,所占用的内存空间更少。

2. 动态监听:使用事件委托可以自动绑定动态添加的元素,即新增的节点不需要主动添加也可以一样具有和其他元素一样的事件。


```
为父子元素中的父元素添加一个监听事件，其子元素都可以触发这个监听事件

父元素.addEventListener("click",function(e){
	var obj = e.targer||e.srcElement;
	console.log(obj.nodeName);  //打印出当前元素的标签名
},true)
```

## 回调函数

>把一个函数B作为实参传递给另外一个函数A，函数在执行的时候，
可以把传递进来的函数B去执行

```
function each(arr,callBack) {
	for(let i=0;i<arr.length;i++){
		let flag = callBack.call(arr,arr[i], i);
		if(flag===false){ //如果为false结束循环
			break;
		}
	}
}
each([10,20,30,40], function (item, index) {
	//this:arr
	if(index>1){
		return false;
	}
});
```

```
function each(fun,obj){
	var arr = this;
	var newArr = [];
	for(let i = 0;i<arr.length;i++){
		if(isNaN(arr[i])){
			break;
		}
		if(obj === undefined){
			newArr.push(fun(arr[i],i));  //this:window
		}else{
			newArr.push(fun.call(obj,arr[i],i));  //this.obj
		}
	}
	return newArr;
}
Array.prototype.each = each;

let arr = [10,20,30,'AA',40];
var obj = {};
arr = arr.each(function(item,index){
	return item * 10;
},obj)
console.log(arr);
```

例:
```
function say(value){
	console.log(value.name);
}

function execute(someFunction){
	var value={
			name:"123"
	}
	someFunction(value);
}

execute(say);
```

## sort排序

>默认按照UniCode码排序，可以传入一个函数作为排序规则

```
function mySort() {
   var array = [12, 1, 3, 56, 7];
   array.sort(sortFun);
}
   
function sortFun(a,b){
	return a-b;
}
```

## setTimeout/setInterval调用函数问题

>定时器是`结合函数引用`进行调度的

三种形式:

```
1.传函数名，不用加引号，也不用加括号
setTimeout(func,1000); //交给定时器来调用

2.传匿名函数
setTimeout(function(){

},1000)

3.传函数字符串，加引号，也要加括号
setTimeout('func()',1000);
这种方法，会在全局作用域下查找函数，有时候有问题
```

## 闭包

>密闭的容器，类似于set,map容器，存储数据的;是一个对象(key:value)

`形成条件`

```
1.函数嵌套
2.内部函数引用外部函数的局部变量
```

#### 优点

```
延长外部函数局部变量的生命周期
```

#### 缺点

```
容易造成内存泄漏
```

#### 注意
```
1.合理的使用闭包
2.用完闭包要及时清除(销毁)
```

例1:
```
function fun(){
	var count = 1;
	return function(){
		count++;
		console.log(count);
	}
}
var fun2 = fun();
fun2(); //2
fun2(); //3
```

例2:
```
function fun(n,o){
	//var n = 0, o;
	console.log(o);
	return {
		fun: function(m){
			//var m = 1;  n = 0;
			return fun(m, n)
		}
	}
}
var a = fun(0); //undefined
a.fun(1); //0
a.fun(2); //0
a.fun(3); //0  只是不停的调用return里的fun,n值没改变

var b = fun(0).fun(1).fun(2).fun(3)
//undefined,0,1,2  每次都新调用都外部函数fun

var c = fun(0).fun(1)
c.fun(2);
c.fun(3);
//undefined,0,1,1
```