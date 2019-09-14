### SpringBoot处理异常

>一、先创建`cn.climber.store.util.JsonResult`响应结果类型，并在其中声明需要它可以给客户的数据的属性：

```
public class JsonResult<T> {

    private Integer state;
    private String message;
    private T data;

    public JsonResult() {
        super();
    }

    public JsonResult(Integer state) {
        super();
        this.state = state;
    }
}
要返回data数据的类型不确定，先定义一个泛型
```

“注册”时，业务层的“注册”功能可能抛出UsernameDuplicateException或InsertException，这2种异常其实也不只是“注册”才会抛出，其它的某些功能也可能抛出同样的异常，SpringMVC框架提供了统一处理异常的机制，在编写控制器中处理请求的代码时，就不必再关注异常的问题，等同于控制器中处理请求时将异常抛出，由SpringMVC框架再去捕获相关异常，并进行处理即可！

>二、可以在控制器中添加一个专门处理异常的方法，关于方法的设计原则：

1. 权限：应该使用public权限；

2. 返回值：使用与处理请求的方法相同的原则；

3. 方法名称：自定义；

4. 参数列表：至少包括1个异常类型的参数，表示将捕获并处理的异常，另外，根据需要，可以添加HttpServletRequest等参数，但是，并不能像处理请求的方法那样随意添加；

5. 必须添加@ExceptionHandler注解。

**处理异常的方法:**

```
@ExceptionHandler
public JsonResult<Void> handleException(Throwable ex) {

}
```

**以上方法必须在控制器类中，也只能作用于当前控制器类中处理请求时抛出的异常，为了使得所有控制器类抛出的异常都可以被处理，应该将以上处理异常的代码添加在控制器类的基类中：**

```
/**
 * 控制器类的基类
 */
public abstract class BaseController {

    @ExceptionHandler(ServiceException.class)
    public JsonResult<Void> handleException(Throwable ex) {
        JsonResult<Void> jsonResult = new JsonResult<>();

        if (ex instanceof UsernameDuplicateException) {
            jsonResult.setState(2);
        } else if (ex instanceof InsertException) {
            jsonResult.setState(3);
        }
        return jsonResult;
    }
}
```

如果不通过基类处理异常，也可以自定义某个类，在类之前添加`@ControllerAdvice`或`@RestControllerAdvice`也可以使得整个项目的所有控制器都应用该处理异常的做法！

>三、设计所需要处理的请求

规划客户端需要向服务器端的哪个URL提交请求，提交请求时，使用哪种请求方式(GET/POST)，需要提交哪些请求参数，当服务器端的控制器接收到并处理完成后，应该给予什么样的响应结果。

```
请求路径：/users/reg
请求参数：User user
请求类型：POST
响应结果：JsonResult > {"state":1} / {"state":2,"message":"您尝试注册的用户名已经被占用"}
```

>四、处理请求

创建`cn.tedu.store.controller.UserController`控制器类，在类之前添加`@RestController和@RequestMapping("users")`这2个注解，并声明`@Autowired private IUserService userService;`业务层对象，然后，添加处理请求的方法：

```
@RestController
@RequestMapping("users")
public class UserController {

    @Autowired
    private IUserService userService;

    @RequestMapping("reg")
    public JsonResult<Void> reg(User user) {
        userService.reg(user);
        return new JsonResult<>(1);
    }
}
```





