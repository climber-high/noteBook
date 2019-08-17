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






