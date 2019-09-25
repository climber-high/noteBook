### Spring AOP

>AOP是面向切面(Aspect)的编程。

**AOP并不是Spring的特点，只是Spring能很好的支持AOP，在基于Spring框架的的基础之上，开发人员能够更简单快捷创建并应用切面，实现AOP效果。**

关于处理数据的流程：
```
注册      客户端 ---> Controller ---> Service ---> Mapper
登录      客户端 ---> Controller ---> Service ---> Mapper
改密码 客户端 ---> Controller ---> Service ---> Mapper
改头像 客户端 ---> Controller ---> Service ---> Mapper
```

如果需要处理任何请求时都执行相同的任务，例如统计执行每种业务的耗时，在传统做法中，可以把统计耗时的功能写成一个方法，然后，在处理每种业务时都调用这个方法！这种做法的缺点在于需要修改大量业务，在每个业务中添加对这个方法的调用！

可以假想在数据处理流程中，添加1个切面，在切面中可以配置相关方法，在处理任何一个业务时，都会经过这个切面，并执行切面中的方法！在开发时，只需要考虑切面的创建、切面中需要执行的方法，并不需要修改原有的任何代码！

所以，可以认为：在处理多个业务时都需要执行相同的任务，就可以使用AOP。

假设需要实现：统计每个Service中的业务功能的执行耗时。

首先，添加AOP相关依赖aspectj-tools他aspectj-weaver：
```
<dependency>
    <groupId>aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.5.4</version>
</dependency>

<dependency>
    <groupId>aspectj</groupId>
    <artifactId>aspectj-tools</artifactId>
    <version>1.0.6</version>
</dependency>
```

自定义切面类：
```
@Component
@Aspect
public class TimerAspect {

}
```

>然后，在切面类中添加切面方法，即各个业务都需要执行的方法，关于该方法：

- 权限：应该使用public权限；

- 返回值类型：如果切面方法使用@Before或@After注解进行配置，可以使用void表示无返回值，如果切面方法使用@Around注解进行配置，则需要使用Object类型的返回值；

- 方法名称：自定义；

- 参数列表：如果切面方法使用@Around注解进行配置，可以添加ProceedingJoinPoint类型的参数，用于在切面方法中调用切面对应的方法，如果使用的不是@Around注解，则可以不添加参数；

如果添加了`ProceedingJoinPoint`类型的参数，它表示切面对应的方法的调用者，可以调用该参数对象的`proceed()`方法，即等效于执行了某个业务方法！例如注册过程中执行到了切面，调用该方法时，等效于执行了IUserService所声明的reg()方法，如果是登录过程中执行的，就等效于执行了login()方法。

**注意：调用ProceedingJoinPoint的proceed()方法时需要处理异常，该异常表示的是某业务方法中抛出的异常，例如表示注册时业务方法抛出的UsernameDuplicateException的对象，应该将该异常继续抛出，交由控制器进行处理！**

以统计业务层的方法执行耗时为例，可以编写为：
```
public Object timer(ProceedingJoinPoint pjp) throws Throwable {
    long start = System.currentTimeMillis();

    Object result = pjp.proceed();

    long end = System.currentTimeMillis();

    System.err.println("耗时：" + (end - start) + "毫秒。");

    return result;
}
```
然后，还需要在方法之前添加注解，以配置切面将应用于哪里！

**@Around("execution(* cn.tedu.store.service.impl.*.*(..))")**
除了`@Around`以外，还可以使用`@Before`或`@After`，分别表示在某个执行流程的节点之前或之后创建切面！通常，仅使用@Around即可！
