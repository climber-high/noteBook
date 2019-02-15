## Sass语言-预处理CSS

### WHAT - 他是个什么东西
sass这个语言的诞生，主要用于更好的编写和维护我们的css代码。可以理解为超级css(它的文件后缀是`.scss`)。同类型的还有less,stylus

### WHEN - 什么时候用它
当你要写css的时候

### WHY 为什么要用它
因为它具备很多编程语言的特点在里面，例如代码可以封装为模块，进行重用，也可以
对我们的结构进行有条不紊的编程，这些都是原来的css所做不到的，并且，面对日益复杂的网站和需求，多人协作已经是一个趋势，让我们用sass可以更好的去协同多人代码的分工和合并。

### 安装
1. ruby安装，安装好用`gem -v`检验是否成功
2. sass安装，安装好用`scss -v`检验是否成功

### 使用
新建目录后：
1.scss scss/web.scss css/web.css
2.scss --watch scss/web.scss:css/web.css


重新打开使用时：(开启监听)
scss --watch scss:css
        (监听目录):(输出目录)
代码生成风格：`--stylt expanded`：没有缩进的、扩展的css代码。

有不懂看[文档] (http://www.sass.hk/docs/)

### 语法
1.变量
	```
	//设置变量，方便后续重复使用，并方便统一修改
	$h1-size:22px;
	$h2-size:18px;
	div .title{
	    font-size: $h1-size;
	}
	p .title{
	    font-size: $h1-size;
	}
	```
2.嵌套
	```
	.web{
		width:1000px;
		margin:0 auto;
		header{
			//这里就可以写关于.web里面的子元素header的相关样式
		}
	}
	```
	> ps:这样的写法可以方便我们管理元素的层级关系，更好的维护代码

	如果要写伪类选择器和伪对象选择器，可以用"&"符号来代替自身，例如
	```
	div{
		&:hover{
			background:skybule;
		}
		&::after{
			content:"";
		}
		~p{
			color:pink
		}
	}
3.导入scss文件
	可以把多次重用的样式写到单独一个scss文件里，在需要的时候
	导入进来
	```
	//导入其他SCSS文件
	@import "common/reset"
	@import "common/common"
	
	div{
	    color: white;
	}
	```
	> 如果你不希望被导入的文件，被编译成css文件，可以在它的
	名字前面加一个**_(下划线)**

4.重用代码块
定义了一个重用代码块，名字叫title,当我们需要的时候，就喊一下这个名字
```
	@mixin title{
	    width: 100%;
	    height: 50px;
	    line-height: 50px;
	    font-size: 26px;
	    border: 1px solid blue;
	    border-radius: 15px;
	    text-indent: 2em;
	}
```