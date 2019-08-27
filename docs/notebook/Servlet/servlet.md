### Servlet

### 什么是服务器

>就是一台高性能的电脑

- web服务器： 就是在电脑上安装了web服务软件 如：Tomcat
- 邮件服务器： 就是在电脑上安装了邮件服务器软件
- 数据库服务器： 就是在电脑上安装了MySQL或Oracle类似的软件

### 什么是Web服务软件(Tomcat)

>负责建立网络连接，读取相关的文件返回给客户端，可以理解成是一个容器，装的是处理各种业务的Servlet

## 在eclipse中配置Tomcat

	1.下载tomcat安装包和源代码
	2.eclipse中 window -> Preferences -> server -> runtime -> add -> 找到自己下载安装包对应的版本 -> 点下一步 -> browse找到安装包路径finish

## 创建web工程

	1.创建maven工程,把jar改成war

	2.解决报错:在最长的文件上右键点击最长的(Generate Deployment Descriptor Stub)，要在Project Explorer中操作

	3.项目右键Properties -> Targeted Runtimes 选取服务确认

	4.在src/main/java右键新建servlet，进到类里面，ctrl鼠标左键找到源代码，没有就Attach Source导入下载的tomcat源代码(配置一次就行)，重写service方法，添加业务
	//设置响应类型
    resp.setContentType("text/html");
    //得到输出对象
    PrintWriter pw = resp.getWriter();
    //输出数据
    pw.println("<h1>HelloServlet2</h1>");
    pw.close();

	5.Window -> Show View -> Servers 点击No servers are available 选择服务类型Tomcat 版本 server完成(配置一次就行)

	6.添加服务，双击服务选择Server Locations中的第二个Use Tomcat installation

	7.运行:项目右键run on servers

##### 设置响应类型和字符集

```
resp.setContentType("text/html;charset=utf-8");
```

## 请求方式

>Get请求和Post请求

### service方法

- 该方法中做了一个判断，如果请求方式为get则调用doGet方法 如果请求方式为post则调用doPost方法.

### 获取同名参数

	form表单多选框多个参数传给后台用getParameterValues获取所有选中值
 	String[] hobbies = request.getParameterValues("hobby");

**加入jdbc代码改成1.7时会报错，在工程上右键->properties -> Project facets -> Java改成1.7**

## 重定向

>response.sendRedirect(request.getContextPath()+"/ListServlet");

- 此方法给浏览器返回一个地址和302状态码，当浏览器接收到302状态码的时候会立即向地址放送请求


## Thymeleaf

>什么是Thymeleaf:Thymeleaf是一个模板引擎，功能是将一个html页面作为一个模板，并根据模板中特定的标记 对模板中的内容进行替换或修改，最后形成一个新的html

>为什么使用Thymeleaf:如果不使用Thymeleaf 我们需要在Servlet里面即需要处理数据也许处理复杂的html标签， 使用Thymeleaf后可以将html标签部分从Servlet中分离出来，仍然在单独的html页面文件中写页面相关内容


配置:
```
    <dependency>
      <groupId>org.thymeleaf</groupId>
      <artifactId>thymeleaf</artifactId>
      <version>3.0.9.RELEASE</version>
    </dependency>
```

例:
```
//创建模板引擎对象
TemplateEngine te = new TemplateEngine();
//创建解析器
ClassLoaderTemplateResolver resolver = new ClassLoaderTemplateResolver();

//设置字符集
resolver.setCharacterEncoding("utf-8");
//把解析器和模板引擎关联
te.setTemplateResolver(resolver);

//创建上下文对象，用于传递参数
Context context = new Context();
context.setVariable("msg", "Hello Thymeleaf");

//把数据加载到页面中
String html = te.process("hello.html", context);

//把html返回给客户端
response.setContentType("text/html;charset=utf-8");
PrintWriter pw = response.getWriter();
pw.println(html);
pw.close();

HTML代码:
<h1 th:text="${msg}">abcd</h1>
```

## Thymeleaf 属性

1. th:text 类似于innerText
例:
```
<div th:text="${user.name}"></div>
```

2. th:utext 类似于innerHTML
例:
```
<div th:utext="${msg}"></div>
```

3. 如果需要给页面传递多个相关参数，可以将多个相关参数封装到一个对象中，这样只需要给页面传递一个对象即可，在页面中通过以下方式获取数据
4.th:remove="all"  当动态加载页面时删除标签
例:
```
<div th:remove="all"></div>
```

5.th:each="e : ${emps}" 遍历集合中的对象 
例:
```
<tr th:each="e : ${emps}">
    <td th:text="${e.empno}"></td>
    <td th:text="${e.ename}"></td>
    <td th:text="${e.sal}"></td>
</tr>
```

6.th:src=@{images/img{n}.jpg(n=${first.articleRandomDouble%20})}

