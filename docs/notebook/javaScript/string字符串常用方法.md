# string字符串常用方法
## 一.查找类
```
1.查找字符
①charAt
功能：查找第n个位置的字符
参数：数值，若超出字符串长度，则返回空字符串
返回值类型：字符串string
例子："abc".charAt(0) //"a"
②charCodeAt
功能：查找第n个位置的字符unicode编码
参数：数值，若超出字符串长度，则返回NaN
返回值类型：数字number
例子："刘备".charCodeAt() //"21016"
```
```
2.查找位置
①indexOf
功能：从左往右，查找特定字符首次出现的索引,若查找失败则返回-1
参数：字符串
返回值类型：数字
例子："Miss".indexOf("s") //2
②lastIndexOf
功能：从右往左，查找特定字符首次出现的索引,若查找失败则返回-1
参数：字符串
返回值类型：数字
例子："Miss".lastIndexOf("s") //3
```

## 二.功能类
### ①concat()
```
功能：将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回。
参数：字符串
返回值类型：新的字符串
例子：
var a="hello ",b="world";
var c=a.concat(b);
console.log(c);  //hello world
```
### ②replace()
```
功能：返回一个由替换值替换一些或所有匹配的模式后的新字符串。
参数：字符串或正则表达式
返回值类型：新的字符串
例子：
var g="It's time to Sleep";
var oldword="Sleep";
console.log(g.replace(oldword,"have breakfast"));
```
### ③toLowerCase ()
```
功能：把字符串值转为小写形式
参数：字符串
返回值类型：字符串
例子：
var J="TextAligin";
console.log(J.toLowerCase());
```
### ④toUpperCase ()
```
功能：把字符串值转为大写形式
参数：字符串
返回值类型：字符串
例子：
var k="TextAligin";
console.log(k.toUpperCase());
```
### ⑤match()
```
功能：当一个字符串与一个正则表达式匹配时，检索匹配项。(检索想要的字符串)
参数：字符串，正则表达式
返回值类型：想要检索的字符串
例子：
var x="n&i$123$zh#en*b@a$"
console.log(x.match(/[A-Z,0-9]/ig))
```
### ⑥substr()
```
功能：返回一个字符串中从指定位置开始到指定字符数的字符。(切割想要的字符串)
参数：数值
返回值类型：想要切割的字符串
例子：
var str='ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz';
console.log(str.substr(0,5))   //ABCDE
console.log(str.substr(-10,1)) //q
第一个参数,开始切割的位置
第二个参数,切割的长度
```
### ⑦substring()
```
功能：返回一个字符串在开始索引到结束索引之间的一个子集, 或从开始索引直到字符串的末尾的一个子集。(没有负数)
参数：数值
返回值类型：想要切割的字符串
例子：
var str='ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz';
console.log(str.substring(3,10));
console.log(str.substring(10,3)==str.substring(10,3));
```
### ⑧slice ()
```
功能：提取一个字符串的一部分，并返回一新的字符串。(可以负数)
参数：数值
返回值类型：想要切割的字符串
例子：var str='ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz';
var s1=str.slice(3,10);
var s2=str.slice(10,3);
	s1===s2;
```
### ⑨split ()
```
功能：使用指定的分隔符字符串将一个String对象分割成字符串数组，以将字符串分隔为子字符串，以确定每个拆分的位置。
参数：数值
返回值类型：字符串
例子：var digite="0123456789";
	console.log(digite.split("",5))
	//  ["0", "1", "2", "3", "4"]
```