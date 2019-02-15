## Date日期对象
### 1.getFullYear()
```
功能：获取当前完整年份
返回值：对应于给定日期的4位年份数字
例子：var myDate = new Date();
		document.write(myDate.getFullYear());
```
### 2.getMonth()
```
功能：获取当前月份
返回值：返回一个0 到 11的整数值： 0 代表一月份，1 代表二月分， 2 代表三月份，依次类推。
例子：var myDate = new Date();
		document.write(myDate.getMonth());
```
### 3.getDate()
```
功能：获取当前日
返回值：1-31
例子:var myDate = new Date();
		document.write(myDate.getDate());
```
### 4.getDay()
```
功能：获取当前星期几
返回值：0-6,0代表星期天
例子:var myDate = new Date();
		document.write(myDate.getDay());
```
### 5.getTime()
```
功能：获取当前时间
返回值：从1970.1.1开始的毫秒数
例子:var myDate = new Date();
		document.write(myDate.getTime());
```
### 6.getHours()
```
功能：获取当前小时数
返回值：0-23
例子:var myDate = new Date();
		document.write(myDate.getHours());
```
### 7.getMinutes()
```
功能：获取当前分钟数
返回值：0-59
例子:var myDate = new Date();
		document.write(myDate.getMinutes());
```
### 8.getSeconds()
```
功能：获取当前秒数
返回值：0-59
例子:var myDate = new Date();
		document.write(myDate.getSeconds());
```
### 9.getMilliseconds()
```
功能：获取当前毫秒数
返回值：0-999
例子:var myDate = new Date();
		document.write(myDate.getMilliseconds());
```
### 10.toLocaleDateString()
```
功能：获取当前日期
返回值：当前年月日
例子:var myDate = new Date();
		document.write(myDate.toLocaleDateString());
```
### 11.toLocaleTimeString()()
```
功能：获取当前时间
返回值：上/下午 加时间
例子:var myDate = new Date();
		document.write(myDate.toLocaleTimeString()());
```
### 12.toLocaleString()
```
功能：获取日期与时间
返回值：当前年月日  上/下午 时间
例子:var myDate = new Date();
		document.write(myDate.toLocaleString());
```

### Date注意事项
```
Date对象一定要通过`new`关键字来创建,
	否则会返回字符串,后续无法访问Date对象
	里面的方法或属性
	var date=new Date()
	var date1=Date()
	console.log(date.getFullYear())
	console.log(date1.getFullYear()) //x
	console.log(typeof date)   //object
	console.log(typeof date1)	//string
```