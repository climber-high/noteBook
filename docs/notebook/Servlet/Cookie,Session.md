### Cookie,Session

>用于解决无状态通讯协议下的状态保持问题。

### 无状态通讯协议

```
1.HTTP协议是无持续状态的通讯协议，每次通讯建立连接，传输数据，断开连接。
2.断开后，客户端和服务端没有任何通讯联系。
3.HTTP无连接通讯的优点：充分复用服务器资源，可以为更多的用户提供通讯服务。
4.HTTP无连接协议不能记住客户端状态(Cookie 就是HTTP通讯协议中的会员卡)
```

## cookie

1.在用户第一次访问服务器时候，通过response响应头部下发到浏览器。
2.浏览器负责保存Cookie
- 如果不设置过期时间，保存在浏览器中，完全关闭浏览器失效
- 可以按照秒设置超时时间，会将Cookie保存到硬盘上，直到超时过期。如果在response中覆盖时间为0，则删除Cookie。
- Cookie是按照网站域名保存的，不同网站域名Cookie不会混用。

3.每次访问相同域名时候，浏览器会自动将Cookie通过request的头部带回到服务器。

#### 使用Cookie

1.下发Cookie:通过Response下发Cookie

例:
```
//下发Cookie到浏览器客户端
//Cookie 是在HTTP协议头中传输的，空格和中文都是特殊字符，必须编码传输

Cookie c1 = new Cookie("msg",URLEncoder.encode("Hello World!","UTF-8"));
Cookie c2 = new Cookie("test",URLEncoder.encode("你吃了么","UTF-8"));
c2.setMaxAge(60*60*24*30);  //设置过期时间
//将Cookie通过response下发到浏览器
response.addCookie(c1);
response.addCookie(c2);
```

2.接收Cookie:通过Request对象接收Cookie

例:
```
//接收request中的Cookie
Cookie[] cookies = request.getCookies();
//当没有任何Cookie时候，返回null
if(cookies!=null) {
    for(Cookie cookie: cookies) {
        System.out.println(cookie.getName()+":"+URLDecoder.decode(cookie.getValue(),"UTF-8")); //解码
    }
    //显示一些回馈信息
    response.setContentType("text/html;charset=UTF-8");
    response.getWriter().print("ok");
}
```

## Session

>Cookie 可以被串改，有安全风险，Session采用银行卡机制避免的安全问题。

1.用户第一次来的时候，将Session ID通过Cookie下发到浏览器，同时在服务器上建立Session ID对应的Session对象(帐页)
- 1.Session ID是一个号码，Session对象是储存数据的对象
- 2.一个Session ID对应一个Session对象
- 3.Cookie中存储的是Session ID

2.每次浏览器访问服务器时候，会将Session ID通过带回。
3.服务器会自动根据SessionID匹配找到Session对象。
4.Session可以储存会话相关的信息。
5.如果浏览器没有提供Session ID,则认为是新用户，下发新的Session ID
6.服务器上的Session对象，在超时以后自动删除。一般是30分钟，可以通过web.xml设置

##### request.getSession()

1. request.getSession() 会自动维护Session对象和SessionID。
2. 如果SessionID没有或者服务器上的Session对象已经过去，则自动创建Session，自动分配SessionID, 自动在Cookie下发SessionID。
3. 返回Session对象，相当于保存在服务器端的账本，可以存储任意数据。 
4. Session对象的ID与Cookie中的SessionID一致。

例:
```
HttpSession session = request.getSession();
String id = session.getId();
```

## 使用Cookie和Session

1. 由于Cookie中信息是明码保存，可以被仿冒涂改。 不宜使用Cookie存储敏感信息。 
2. Cookie容量一般不超过4K，只能存储少量信息
3. Cookie经典用途是：用户行为追踪，记住用户名，记住登录状态。
4. Session对象是存储服务器端，其安全性有保证，可以存储敏感信息。
5. Session存储不受限制，数量很多时候会自动缓冲到硬盘。
6. Session经常用于缓存 用户会话相关信息，比如登录用户信息。可以在多个Servlet之间共享数据。

## Filter

过滤器：可以拦截处理任何的web请求，可以在不改变Servlet情况下，为一组Servlet增加功能。

例:
```
1.创建类实现Filter接口
2.重写Filter接口的三个方法doFilter(主要用),init,destroy

public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
	throws IOException, ServletException {
        HttpServletRequest req = (HttpServletRequest) request;  //强转成HttpServletRequest
        HttpServletResponse res = (HttpServletResponse) response;

        HttpSession session = req.getSession();  //调用getSession方法
        User user = (User) session.getAttribute("loginUser");  //获取session里面的loginUser的值
        System.out.println("user:"+user);
        if(user==null){
            String path = req.getContextPath()+"/login.html";
            res.sendRedirect(path);     //没登录就重定向到登录页面
            System.out.println("重定向login");
            return;
        }
        //如果登录就执行后续链条
        System.out.println("执行后续链条");
        chain.doFilter(req, res);
    }
    
3.配置web.xml文件(filter-name对应相同)
<filter>
    <filter-name>access</filter-name>
    <filter-class>cn.climber.web.AccessFilter</filter-class>  //过滤器的完全限定名
</filter>

<filter-mapping>
    <filter-name>access</filter-name>
    <url-pattern>/ListServlet</url-pattern>  //每个需要先经过过滤器的Servlet
</filter-mapping>
<filter-mapping>
    <filter-name>access</filter-name>
    <url-pattern>/DeleteServlet</url-pattern>
</filter-mapping>
```

#### url-pattern

>匹配规则:适合Servlet 和 Filter

1.后缀匹配规则 *.jpg
2.路径匹配规则 /photos/*
3.全匹配 *.*

**注意:**后缀匹配和路径匹配不能联合使用











