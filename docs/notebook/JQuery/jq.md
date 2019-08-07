### jQuery
- 什么是jQuery： 是一个通过js语言缩写的js框架，用于简化原生js代码，可以让程序员写的更少但是实现的更多
- jQuery优势：
1. 简化js代码
2. 可以像css一样获取标签   
	css: #abc    js:var d = document.getElementById("abc");  
	jq:$("#abc")
3. 可以像css一样批量给多个标签设置样式
	css: .c1{color:red;}   
	js:   var arr = document.getElementsByClassName("c1");
			for(var i=0;i<arr.length;i++){
				arr[i].style.color="red";
			}
	jq: $(".c1").css("color","red");
4. 可以解决部分兼容性问题

### 如何引入jQuery

- 由于jQuery就是js语言所写的框架，其实jq就是一个外部的js文件
- 和引入普通的js文件一样，通过script标签的src属性引入

### js对象和jq对象互相转换

- js->jq:    var jq = $(js);
- jq->js:    var js = jq[0];


### 选择器

1. 基础选择器
- 标签名    $("div")
- id选择器  $("#id")
- 类选择器  $(".class")
- 分组选择器  $("div,#id,.class")
- 任意元素选择器  $("*")

2. 层级选择器

- $("div span") 匹配div里面所有的span
- $("div>span") 匹配div里面所有的span子元素
- $("div+span") 匹配div的弟弟元素span
- $("div~span") 匹配div的弟弟们元素span
- 层级相关方法：
	1. $("#abc").siblings() 获取id为abc元素的所有兄弟元素
	2. $("#abc").prev()获取id为abc元素的哥哥元素
	3. $("#abc").prevAll() 获取id为abc元素的哥哥们
	4. $("#abc").next()获取id为abc元素的弟弟元素
	5. $("#abc").nextAll()获取id为abc元素的弟弟们
	6. $("#abc").parent() 获取id为abc元素的父元素
	7. $("#abc").children() 获取id为abc元素的所有子元素
	```
		<span>s1</span>
		<div id="abc">d1</div>
		<span>s2</span>
		<span>s3</span>
		<div id="abc">d2</div>
    ```
3. 过滤选择器
- $("div:first") 匹配所有div中的第一个
- $("div:last") 匹配所有div中的最后一个
- $("div:even") 匹配所有div中下标为偶数的div
- $("div:odd") 匹配所有div中下标为奇数的div
- $("div:lt(n)") 匹配所有div中下标小于n的div
- $("div:gt(n)") 匹配所有div中下标大于n的div
- $("div:eq(n)") 匹配所有div中下标等于n的div
- $("div:not(.abc)") 匹配所有div中class值不等于abc的div
4. 可见选择器
- $("div:hidden") 匹配所有隐藏的div
- $("div:visible") 匹配所有显示的div
- 隐藏显示相关的方法
	1. $("#abc").show() 显示
	2. $("#abc").hide() 隐藏
	3. $("#abc").toggle() 隐藏显示切换
5. 内容选择器
- $("div:has(p)") 匹配包含p子元素的div
- $("div:empty") 匹配空的div
- $("div:parent") 匹配非空的div
- $("div:contains('xxx')") 匹配包含xxx文本的div

6. 属性选择器
- $("div[id]") 匹配包含id属性的div
- $("div[id='xxx']") 匹配id属性值等于xxx的div
- $("div[id!='xxx']") 匹配id属性值不等于xxx的div
7. 子元素选择器
- $("div:first-child") 匹配是子元素，并且是div，并且是第一个
- $("div:last-child") 匹配是子元素,并且是div，并且是最后一个
- $("div:nth-child(n)") 匹配是子元素，并且是div，并且是第n个 n从1开始
8. 表单选择器
- $(":input") 匹配表单中的任意控件
- $(":password") 匹配密码框
- $(":radio") 匹配单选
- $(":checkbox") 匹配多选
- $(":checked") 匹配选中的单选、多选、下拉选 
- $("input:checked") 匹配选中的单选和多选 
- $(":selected") 匹配选中的下拉选

### 创建标签

```
- js:var d = document.createElement("div");
- jq: var d = $("<div id='abc'>xxxx</div>");
```

### 添加标签

js: document.body.appendChild(d);
- 最前面添加: $("body").prepend(d);
- 最后面添加: $("body").append(d);

### 插入标签

- 插入到兄弟标签的前面：  $("#abc").before(d);
- 插入到兄弟标签的后面:  $("#abc").after(d);


### 删除标签

- js:document.body.removeChild(被删除的标签对象)
- jq: $("#abc").remove(); 


### 获取和修改元素的文本内容 等效 innerText

- .text() 获取值
- .text("xxx") 赋值


### 获取和修改元素的html内容 等效 innerHTML

```
- .html()
- .html("<h1></h1>")
```

### 获取和修改元素的css样式

- .css("样式名")  获取某样式的值
- .css("样式名","值") 赋值
- .css({"样式1名":"值","样式2名":"值"})  批量赋值


### 获取和修改元素的属性

- .attr("属性名")  获取某个属性的值
- .attr("属性名","值") 
	$("#abc").attr("class","c1");


### 事件模拟

- 通过以下代码触发标签的事件
	标签对象.trigger("事件名");

### hover方法

- 将鼠标移入和移出合并到一起的方法
```
	//给div添加移入移出事件 
	$("div").hover(function(){//鼠标移入时执行
		$(this).css("color","red");
	},function(){//鼠标移出时执行
		$(this).css("color","green");
	});
``` 
   
### 动画相关

- .hide(3000,function(){动画做完后的事情})  隐藏
- .show()  显示
- .fadeOut()  淡出
- .fadeIn()  淡入
- .slideUp()  上滑
- .slideDown()  下滑

- .animate({"top":"100px"},1000)
- .animate({rotate:"180"},{
	step:function(rotate){
    .css({"transform":"rotate("+rotate+"deg)"})
    },duration:1500
})




