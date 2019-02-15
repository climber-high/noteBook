## css引入方式

### css引入方式有3种:

#### 第一种引入方式：
```
行内引入（只能控制当前元素）
注意：不推荐的写法，因为结构混乱，且不利于重用
```
##### 例：
   ```
 <div style="width: 500px;height: 500px;
  background-color:darkorange;">
        我是一个饼
		</div>
   ```

#### 第二种引入方式：
```
页内引用
注意：该写法通常不推荐使用，因为只能控制当前页，不能重用到其他页面
```
##### 例：
```
<style type="text/css">
div{
border-radius:50% ;
}
</style>
```


#### 第三种引入方式：
```
页外引用（可以针对1个或多个HTML文件）
注意：是一个独立的css文件，注意语法的写法。可以有效的重用样式
```
##### 例
```
<head>
<meta charset="utf-8" />
<title></title>
<link rel="stylesheet" type="text/css" href="css/web.css"/>
</head>
```