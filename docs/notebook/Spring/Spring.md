### Spring

>Spring是开源组织。Spring组织提供了很多开源组件

**两大核心功能:**IOC/DI 和 AOP

## IOC 控制反转

>指对象的创建和管理，提供例非常复杂的JavaBean对象管理功能

1.主动控制:由应用程序创建和管理对象，称为主动控制
  - 适合创建简单对象

2.控制反转:应用程序将对象的创建和管理交给环境处理，应用程序只是被动的使用对象，称为控制反转。
  - 适合创建和管理复杂对象

#### 使用Spring的IOC功能

```
1.准备一个被IOC管理的类
2.利用Maven导入Spring API
3.创建一个Spring配置文件，将被IOC管理的类，告诉Spring
4.利用配置文件初始化Spring,Spring启动时自动根据配置文件找到类，并且创建对象。
5.从Spring获得(IOC)创建的对象，测试是否成功创建了对象。


导入Spring的jar包
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>4.3.9.RELEASE</version>
</dependency>

//在创建ctx(Spring IOC容器)对象时候，Spring会根据xml配置文件，创建xml文件中登记的对象
ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
//从ctx对象中获取已经创建好的对象
DemoBean bean = (DemoBean) ctx.getBean("demo");
//检查对象是否创建成功
System.out.println(bean);
```

## JavaBean

>JavaBean是指其类型满足一定规则的Java对象。由属性，方法，事件3部分组成

1.必须有包(package)

2.有无参数构造器，默认构造器就是无参数构造器。-方便派生子类

3.实现序列化接口。方便Java底层自动进行对象序列化。

4.可以包含Bean属性。Bean属性就是setXxx、getXxx方法。
- 当有方法getName setName时，就称为包含Bean属性 name
- boolean 类型的Bean属性方法 getXxx 或 isXxx

例:
```
class User{
	private int age; //对象属性，实例变量
    private String name;  //对象属性，实例变量
    private boolean married; //对象属性，实例变量

    public int getAge(){  //bean 属性，属性名age
        return age;
    }
    public void setAge(int age){  //bean 属性，属性名age
        this.age = age;
    }
    public boolean isMarried(){  //bean 属性，属性名married
        return married;
    }
}
```

>Spring IOC 是管理JavaBean对象的容器

1.Spring建议被管理的对象采用JavaBean规范。
- Spring 可以宽泛的支持Java对象，即便没有遵守JavaBean规范。

2.JavaBean规范不是强制性规范，但是被所有企业作为编程规范。

3.可以简单理解JavaBean就是普通的Java对象。

4.些软件称为pojo对象。 

**宽泛的理解:**普通java对象 >= pojo对象 >= JavaBean对象

### Spring提供了重载的getBean

	DemoBean bean = ctx.getBean("demo",DemoBean.class);

### Spring Bean 的别名功能alias

例:
```
<!-- 通知Spring创建哪个类的实例 -->
<bean id="demo" class="day01.DemoBean"></bean>
<!-- alias别名 -->
<alias name="demo" alias="bean1"/>
```

## Junit 

>Java 单元测试工具。

配置，导入jar包:
```
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
</dependency>
```

使用:
```
1.在src/test/java创建测试类

2.写junit语句，运行直接调用方法，不用main方法
@Test
public void testHello() {
    System.out.println("Hello World!");
}
注意:选中要执行的方法就只会执行这个方法，不选就所有方法都会执行

@Before //在测试案例之前执行的方法，可以把下面方法公共代码放到这里先执行

@After  //在测试案例之后执行的方法
```

##### Spring 中默认情况下Bean是单例的

单例： 在软件运行期间，对象个体始终唯一的现象。

表现：多次调用getBean获得的引用都引用了同一个对象。 

例:
```
ClassPathXmlApplicationContext ctx = new
           ClassPathXmlApplicationContext("applicationContext.xml");
DemoBean b1 = ctx.getBean("demo",DemoBean.class);
DemoBean b2 = ctx.getBean("demo",DemoBean.class);

System.out.println(b1==b2);  //true
```

##### scope="prototype" 属性

>利用scope属性 可以创建多个对象实例

```
applicationContext.xml配置
<bean id="demo1" class="day01.DemoBean" scope="prototype"></bean>

DemoBean b1 = ctx.getBean("demo1",DemoBean.class);
DemoBean b2 = ctx.getBean("demo1",DemoBean.class);
System.out.println(b1==b2);   //false
```



