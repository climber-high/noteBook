### MVC
1.Modle模型:操作数据部分 DAO
2.View视图:显示相关部分 Thymeleaf
3.Controller控制器 : M和V的中间调度 Servlet

### webapp目录和resources目录区别

1. 写在webapp下的页面可以被用户直接访问
2. Thymeleaf默认访问的路径是resources目录下的页面，此目录的页面编译后会自动添加到WEB-INF目录下 可以达到禁止用户直接访问的目的（工作中很多页面是不允许用户直接访问的比如登录后的页面）

>页面通过Thymeleaf加载后，页面文件中的../代表的不再是文件所在位置往上一层，而是Servlet位置往上一层，所有的Servlet的位置为工程的根路径 