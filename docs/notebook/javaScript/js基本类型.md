# js基本数据类型

## 字符串
	1.字符串由多个或者零个字符组成的,字符包括字母,数字,标点符号,空格
	2.字符串必须放在""或者是''里面
```
var str = "123";
```

## 数值
```
var num =123;
```
数组是最基本的数据结构，访问对应的属性就可以通过访问其内部属性名即可
```
var arr =["名字",18,180];
console.log(arr[0]);
```

## 布尔值Boolean
```
var bool=true;
```

## undefined
```
var un  (未定义)
```

## null

## 鉴别基本数据类型 typeof
	typeof 可以辨别基本数据类型
	(String,Number,Boolean,undefined,Object),但是不能够准确地分辨Array[]和Object{}
```
分辨方法：Object.prototype.toString.call()
```

## 字符串转换
字符串转数字
```
var str1="123";
var str2="12.3";
var str3="中文";

整形 parseInt
str1=parseInt(str1);


浮点数（带小数点的）parseFloat
str2=parseFloat(str2);

非数字 NaN  Not a Number
str3=parseFloat(str3);
```





