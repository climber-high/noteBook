### jsp

>sun公司制订的一种服务器端动态页面技术规范。

```
因为虽然使用servlet也可以生成动态页面，但是过于繁琐(需要使用out.println语句),并且不利于页面的
维护(要修改页面，就得修改java代码)。所以sun才推出了jsp规范。

jsp是一个以".jsp"为后缀的文件，该文件的内容主要是html和少量的java代码。该文件会被容器转换成一
个对应的servlet然后执行。也就是说，jsp的本质就是一个servlet!
```

### 编写jsp文件

>以.jsp为后缀的文件。html,css,js可以直接写，可以加java代码

1. java代码片段

```
	<% java代码 %>
```

2. jsp表达式

```
	<%= java表达式 %>
```

3. jsp声明

```
	<%! 声明变量或方法 %>
	通过jsp声明，可以给对应的serlet添加新的成员变量或者方法。
```

## 隐含对象

>直接可以使用的对象，比如out,request,response,session,applicetion,pagecontext,config,exception,page。容器在生成对应的servlet时，会添加获得这些

	page(了解即可): jsp实例本身，jsp实例指的是jsp对应的那个
				servlet对象。
	pageContext: 容器会为每一个jsp实例创建唯一的一个符合PageContext
				接口要求的对象，该对象会一直存在，除非jsp实例被删除。
				作用1:绑订数据。
				作用2:提供了一些方法，用来获得其它所有的隐含对象。

	config: 即ServletConfig,用来读取初始化参数。   
	exception: 用来获得jsp运行时产生的一些异常信息的。
	session：会话
	application



## 指令

>通过指令，可以告诉容器，在生成servlet代码时，额外做一些处理，比如导包。

**指令的语法**

```
    <%@ 指令名 属性=值 %>
	如果有多个属性，则属性之间用空格隔开。
```

>page指令

```
    import属性：用于指定要导的包名。比如
    <%@ page import="java.util.*,java.text.*"%>
    
    contentType属性：设置response.setContentType方法的内容。
    pageEncoding属性：容器在读取jsp文件的内容时，会按照该属性指定的字符集去解码。
    
    errorPage属性:用于指定一个异常处理页面。
		注：当jsp运行发生了异常，则容器会调用异常处理页面。
		
	isErrorPage属性：缺省值是false, 当值是true时，才可以
		使用exception隐含对象。
		
	session属性：缺省值是true,当值为false时，不能够使用 
		session隐含对象。
```

## jsp执行流程

1. 容器会将jsp转换成一个对应的servlet。

```
	a. html(css,js) ------> 在service方法里,使用out.write输出。
	注意:out.print对于null会输出null, 而out.write会将 null转换成""输出。
    
	b.<%     %>  ------> 在service方法里面，照搬。
	c.<%=    %>  ------> 在service方法里面，使用out.print输出。
```

2. 容器调用该servlet。


	会将该servlet编译、然后实例化、初始化、调用。





