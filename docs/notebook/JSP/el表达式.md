### JSP

## el表达式

>是一套简单的计算规则，用于给jsp标签的属性赋值。

	注:el表达式也可以脱离jsp标签，直接使用。

### el表达式的使用
1.读取bean的属性:

	方式一 ${user.username}
		a1.执行过程: 容器会依次从pageContext-->request-->session
			-->application中查找绑订名为"user"的对象,找到之后，会
			调用该对象的"getUsername"方法，然后输出。
		a2.优点
			比直接使用java代码更简洁。
			对于null，会转换成""输出。
			如果找不到对应的对象，不会报空指针异常,会输出""。
		a3.指定查找范围。
			可以使用pageScope、requestScope、sessionScope、
			applicationScope来指定查找的范围，比如
			${sessionScope.user.username}。

	方式二  ${user['username']}
		 a1.等价于${user.username}。
		 a2.[]里面可以使用绑订名。
		 a3.[]里面可以使用从0开始的下标，用于访问数组中的指定
			下标的元素。

2.做一些简单的运算:

	b1.算术运算: +,-,*,/,%(注: "+"只能求和。)
	
	b2.关系运算: >,>=,<,<=,==,!=
			
	b3.逻辑运算: &&,||,!

	b4.empty运算: empty
				注：用于判断是否是一个空的集合或者是一个空字符串，
			如果是，返回true。

c.读取请求参数值
	
	${param.username} 等价于 String request.getParameter("username");

	${paramValues.interest} 等价于 String[] request.getParameterValues("interest");
	
	当有多个请求参数名相同时，使用getParameterValues方法。


## jstl标签

>apache开发的一套jsp标签，后来捐献给了sun,sun将其全名为jstl(jsp standard tag)。

### 使用jstl?

1. 导入jstl相关的jar包。

	<dependency>
		<groupId>jstl</groupId>
		<artifactId>jstl</artifactId>
		<version>1.2</version>
	</dependency>

2.使用taglib指令导入要使用的标签。

	<%@ taglib uri="" prefix=""%> 
	uri属性：用于指定jsp标签所属的命名空间的。
	prefix属性：指定命名空间的别名。
 	 
	命名空间:为了区分同名的元素而在元素前添加的一段说明。
	为了避免命名空间也冲突，经常使用域名来作为命名空间。

### 几个核心标签

1. if标签

	<c:if test="">
		标签体   
	</c:if>	
	
	当test属性值为true时，执行标签体的内容。
	注：
		test属性	可以使用el表达式来计算。

2.choose标签
	
	<c:choose>
		<c:when test="">
		</c:when>
		<c:otherwise>
		</c:otherwise>
	</c:choose>	

	when可以出现1次或者多次，表示一个分支(相当于一个if语句)。
	otherwise可以出现0次或者1次，表示例外(相当于最后那个else)。
	当test属性值为true时，执行标签体的内容。

3.forEach标签

	<c:forEach items="" var="" varStatus="">
	</c:forEach>
	items属性用于指定要遍历的集合或者数组，可以使用el表达式来赋值。
	var属性用于指定一个绑订名：
		注：
			该标签每次从集合或者数组中取一个元素，然后将该元素
			绑订到pageContext上，绑订名由var属性来指定。
	varStatus属性用于指定一个绑订名:
		注:
			绑订值是一个特殊的对象，由该标签创建。该对象提供了
			一些方法用于获得当前遍历的状态：
			getIndex(): 获得当前正在被遍历的元素的下标(从零开始)。
			getCount(): 获得这是第几次遍历（从1开始)。