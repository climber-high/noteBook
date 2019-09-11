### SpringBoot框架

>SpringBoot框架是一个默认已经添加了许多依赖(例如spring相关的依赖、单元测试的依赖、jackson的依赖等)的框架，同时，已经完成了许多常规的配置(例如各项需要设置编码的位置、组件扫描、注解驱动等)，其核心思想是约定大于配置，即：大家都遵守共同的约定来编程即可，而无须关心相关配置！

## 创建Spring项目

1. 推荐打开`https://start.spring.io`网站，在页面中输入所创建的项目的相关参数，根据需要勾选所需的依赖，点击按钮即可下载项目。
2. 推荐将下载的项目解压到Workspace中，并通过Eclipse的Import > Existing Maven Projects方式导入项目。
3. 导入后，项目应该开始自动更新，如果没有开始自动更新，也可以对项目点右键，选择Maven > Update Project，在弹出的对话框中勾选Force Update ...选项再开始更新！

**注意：如果使用的是Mars(4.5)版本的Eclipse，在pom.xml文件中可能会提示Maven相关的错误，可以无视该错误提示，并不影响程序运行！如果需要解决错误，更换使用Oxygen(4.7)或以上版本的Eclipse即可！**

**注意：SpringBoot默认会创建根包，是根据在网站上填写的group和artifact决定的，后续创建的各个组件类(例如控制器类)都必须放在这个包，或其子包下，因为SpringBoot默认配置的组件扫描就是扫这个包！**

**注意：在根包下，默认有xxxApplication.java类，该类的名字是根据创建项目时指定的artifact决定的，该类中有main()方法，用于启动整个项目，SpringBoot项目内置Tomcat，启动项目时，就会启动内置的Tomcat，并将整个项目部署到该Tomcat中！如果Tomcat所需的端口被占用，会导致启动失败！**

## 通过SpringBoot项目接收请求并响应

>向服务器发送请求，服务器响应正文。

1. 创建cn.climber.springboot.controller.HelloController控制器类：

```
@RestController
public class HelloController {

}
```

>以上使用的@RestController相当于@Controller和@ResponseBody的组合！使用该注解后，当前类中所有处理请求的方法都相当于添加了@ResponseBody，即每个处理请求的方法都是响应正文的！如果某个处理请求的方法仍需要转发或重定向，必须不使用@RestController注解，或者，在使用@RestController注解的情况下，将处理请求的方法的返回值类型声明为ModelAndView也是可以的！该注解并不是SpringBoot框架所特有的，而是SpringMVC框架就已经定义的注解，只不过在SpringMVC框架中使用时还需要添加配置而已。

2. 然后，在类中添加处理请求的方法：

```
@RequestMapping("hello")
public String hello() {
    return "hello, springboot!!!";
}
```

**注意：SpringBoot项目默认已经配置好了DispatcherServlet，且映射的路径是`/*`，即所有请求，所以，在配置控制器中的路径时，并不需要添加 .do 的后缀！**

最后，打开浏览器，通过http://localhost:8080/hello进行访问即可！

**注意：SpringBoot项目启动时，会把项目部署到内置的Tomcat中，且使用的Context Path是 ''，即没有指定Context Path，所以，在通过URL进行访问时，路径中并不需要包括项目的名称！**

### 在SpringBoot项目中添加静态页面

>SpringBoot项目的src/main/resources下默认存在static文件夹，静态资源(html/css/js/图片等)都应该放在这个文件夹中！

**SpringBoot项目将index.html设置为默认页，当没有指定需要访问的资源时，会视为访问index.html页面！**

## springBoot数据库配置

>因为SpringBoot框架的开发者并不能确定大家用哪一款数据库，或者使用什么框架处理持久层，所以，与数据库相关的依赖默认都是没有添加的！需要在创建项目时就另行勾选，或创建完项目之后，在pom.xml中补充添加！

使用MySQL和MyBatis需要添加的依赖：

```
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.0/1.3.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

**注意：当添加了以上数据库相关依赖后，如果直接运行，SpringBoot项目会自动读取数据库连接相关配置！在没有进行配置之前，项目会启动失败！**

**注意：在src/main/resources下的application.properties就是SpringBoot项目的配置文件！在有些SpringBoot项目中，配置文件也可能是使用yml作为扩展名的，即配置文件名为application.yml。**

>需要在application.properties中添加数据库连接相关的配置：

```
spring.datasource.url=jdbc:mysql://localhost:3306/climber_ums?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
spring.datasource.username=root
spring.datasource.password=root
```

**注意：SpringBoot项目启动时只加载配置信息，并不执行数据库连接，所以，以上配置只要没有语法格式错误，就能正常启动项目，如果配置的值有误，会在后续连接 数据库时报告错误！**

**注意：在src/test/java下默认已经存在单元测试类，且类中已经存在1个空的测试方法！直接执行该测试方法可以用于检验环境大致有没有问题！**

**注意：SpringBoot项目的单元测试类必须放在src/test/java的根包下，且测试类本身必须添加@RunWith(SpringRunner.class)和@SpringBootTest注解！**

>可以在单元测试中添加测试方式，以测试数据库连接是否正确，测试过程中，需要使用DataSource对象，可以通过自动装配的方式获取该对象

```
@Autowired
private DataSource dataSource;

@Test
public void getConnection() throws SQLException {
    Connection conn = dataSource.getConnection();
    System.err.println(conn);
}
```

**注意：在传统的项目中通过getBean()获取的对象，在SpringBoot项目中，都可以通过自动装配获取值！**

## 基于SpringBoot+MyBatis的数据库编程

1. 先创建与数据表结构对应的cn.climber.springboot.entity.User实体类：

```
public class User {
    private Integer id;
    private String username;
    private String password;
    private Integer age;
    private String phone;
    private String email;
    private Integer isDelete;
    private Integer departmentId;
    
    get/set和toString方法
}
```

2. 在使用MyBatis实现持久层开发时，必须先有接口和抽象方法！所以，先创建cn.climber.springboot.mapper.UserMapper接口文件，然后在接口中添加抽象方法

```
public interface UserMapper {
    Integer addnew(User user);
}
```

3. 接下来，还应该使得MyBatis框架知道接口文件在哪里！可以在以上接口的声明之前添加@Mapper注解，以表示这个接口是MyBatis使用的接口，也可以选择在启动类(即：SpringbootApplication类)之前添加@MapperScan注解，并在注解中配置接口文件所在的包

```
@MapperScan("cn.climber.springboot.mapper")
@SpringBootApplication
public class SpringbootApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootApplication.class, args);
    }

}
```

4. 接下来，应该配置抽象方法对应的SQL语句，可以直接在抽象方法之前使用`@Insert`注解来配置SQL语句，如果后续的抽象方法涉及其它类型的SQL语句，也可以使用`@Update`、`@Delete`、`@Select`注解进行配置！这种做法并不是SpringBoot特有的做法，而是MyBatis框架本身就允许的！但是，需要添加一些配置，这些配置SpringBoot已经完成，所以，无需开发人员再配置！这种做法非常直观的表现了抽象方法与SQL语句的对应关系，但是，却不便于管理SQL语句，所以，一般还是推荐使用XML进行配置！

所以，还是在**src/main/resources**下创建名为**mappers**的文件夹，向文件夹中粘贴得到mybatis的xml配置文件，然后配置抽象方法映射的SQL语句

```
<mapper namespace="cn.climber.springboot.mapper.UserMapper">
    <insert id="addnew">
        INSERT INTO t_user (
            username, password,
            age, phone,
            email, is_delete,
            department_id
        ) VALUES (
            #{username}, #{password},
            #{age}, #{phone},
            #{email}, #{isDelete},
            #{departmentId}
        )
    </insert>
</mapper>
```

5. 完成后，还应该配置XML文件的位置，则需要在application.properties中添加配置：

```
mybatis.mapper-locations=classpath:mappers/*.xml
```

6. 最后，应该执行单元测试，以检测操作是否正确！则在src/test/java中的包下创建测试类，在类之前添加2项必须的注解，然后编写并执行单元测试

```
@RunWith(SpringRunner.class)
@SpringBootTest
public class UserMapperTests {

    @Autowired
    private UserMapper mapper;

    @Test
    public void addnew() {
        User user = new User();
        user.setUsername("springboot");
        user.setPassword("456456");
        Integer rows = mapper.addnew(user);
        System.err.println("rows=" + rows);
    }

}
```

## 在控制器处理用户注册的请求

1. 先创建表示响应JSON结果cn.climber.springboot.util.JsonResult的类

```
public class JsonResult {
    private Integer state;
}
```

2. 创建cn.climber.springboot.controller.UserController控制器类，在类之前添加@RestController注解，并在类中添加处理请求的方法：

```
@RestController
public class UserController {
    @RequestMapping("reg")
    public JsonResult reg(User user) {
        return null;
    }
}
```

3. 在具体处理过程中，需要使用持久层已经开发的功能，则应该先添加持久层对象作为全局变量：

```
@Autowired
private UserMapper userMapper;
```

4. 然后，在reg()方法中实现注册，过程大致是：先根据用户名执行查询，如果查询到结果，表示用户名已经被注册，则直接返回“失败”，如果查询不到结果，表示用户名还没有被注册，则执行注册，并返回“成功”。所以，关于处理请求的过程：

```
@RequestMapping("reg")
public JsonResult reg(User user) {
    JsonResult jsonResult = new JsonResult();
    User result = userMapper.findByUsername(user.getUsername());
    if (result == null) {
        userMapper.addnew(user);
        jsonResult.setState(1);
    } else {
        jsonResult.setState(2);
    }
    return jsonResult;
}
```

针对响应的JSON字符串中包括许多为null的属性，可以在对应的类的属性之前添加`@JsonInclude(value=Include.NON_NULL)`注解配置，则当属性为null时，该属性将不会被序列化到JSON字符串中，也可以将该注解添加在类的声明之前，则该类中所有属性都有这样的特点！

**如果希望当前项目中，所有序列化为JSON的过程都有这样的效果，可以在application.properties中添加配置：**

```
spring.jackson.default-property-inclusion=NON_NULL
```


