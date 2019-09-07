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
class User implements Serializable{
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

## Spring Bean管理策略

### 对象生命周期管理方法

>对象的生命周期：一个对象从创建到最后销毁的过程称为对象的生命周期

**Spring提供了调用对象生命周期管理方法**

1. 在对象创建以后会自动执行对象的初始化方法
2. 在单例对象销毁前会自动执行对象的销毁方法
	- 多例（原型）对象不会自动执行销毁方法！

例:
```
public class ExampleDemo {
	public void init() {
		System.out.println("初始化");
	}
	public void work() {
		System.out.println("执行");
	}
	public void destroy() {
		System.out.println("销毁");
	}
}

<bean id="example" class="pack.ExampleDemo" init-method="init" destroy-method="destroy"></bean>

public class TestCase {
	ClassPathXmlApplicationContext ctx;
	@Before 
	public void init() {
		System.out.println("开始运行");
		ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
	}
	@After
	public void after() {
		ctx.close();
		System.out.println("运行结束");
	}
	@Test
	public void test() {
	 ExampleDemo e = ctx.getBean("example",ExampleDemo.class);
	 e.work();
	}
}
开始运行，初始化，执行，销毁，运行结束
```
>init-method="init" 用于设定对象生命周期的初始化方法，Spring会在创建对象以后自动执行这个指定的init的方法

>destroy-method="destroy" 用于设定对象生命周期的销毁方法，单例对象时候Spring会在关闭容器，销毁对象之前自动执行这个destroy方法

>非单例对象不能执行其对象销毁方法 scope="prototype"

### Spring 对象创建时机

1.单例对象:
- 默认情况下，在创建容器时候，立即初始化单例对象
- 在设定懒惰初始化后，单例对象会在第一次使用时候创建对象，再次使用时候，复用第一次创建的对象，不会再次创建对象。

2.多例对象:在使用对象时候创建对象，每次使用都会创建新对象。

>设置lazy-init="true"以后，容器启动时候不会立即创建DemoBean对象，在第一个实验DemoBean对象时候，会自动创建，创建以后，多次时候重用同一个bean对象。懒惰初始化，也称为延迟加载现象

例:
```
<bean id="demoBean" class="pack.DemoBean" lazy-init="true"></bean>
```

DemoBean类:
```
public class DemoBean {
	public DemoBean() {
		System.out.println("创建DemoBean对象");
	}
}
```

## DI 依赖注入

>Spring在初始化Bean的时候，按照配置约定的规则将Bean组件依赖的Bean注入到Bean属性中；被依赖的对象获得方式，通过运行环境自动注入，称为依赖注入。

**依赖:**一个功能(方法)在执行期间需要用到一个对象，称为依赖于对象

==Spring 提供了 DI功能，可以将Bean组件注入到需要的对象中==

```
<!-- 利用property标签注入Bean属性，name是被注入的Bean属性(setTool的tool)，ref是引用已经创建好的Bean ID，表示把Bean ID引用的Bean对象注入到目标的set方法中 -->

<bean id="Saw" class="day02.Saw"></bean>
<bean id="qiang" class="day02.Worker"> 
    <property name="tool" ref="Saw"></property>
</bean>
```

### Spring DI，set方法注入，Bean属性注入

1. 可以注入Bean对象
2. 注入基本值：基本类型+字符串
3. 注入集合
	- 基本值集合
	- 对象集合
4. 注入数组
	- 基本值数组
	- 对象数组

例:
```
<!-- 注入基本值集合 -->
<property name="names">
    <list>
        <value>Tom</value>
        <value>Jerry</value>
        <value>Andy</value>
    </list>		
</property>

<!-- 注入对象集合 -->
<property name="tools">
    <list>
        <ref bean="Saw"/>
        <ref bean="Axe"/>
        <bean class="day02.Axe"></bean>
    </list>
</property>

<!-- 注入Map集合 -->
<property name="map">
    <map>
        <entry key="东" value="范"></entry>
        <entry key="南" value="田"></entry>
        <entry key="西" value="河"></entry>
        <entry key="北" value="刘"></entry>
    </map>
</property>

<!-- 注入数组 -->
<property name="values">
    <array>
        <value>3.14</value>
        <value>0.618</value>
        <value>2.17</value>
    </array>
</property>

<!-- 注入对象数组 -->
<property name="arr">
    <array>
        <ref bean="Axe"/>
        <bean class="day02.Saw"></bean>
    </array>
</property>
```

### 利用IOC、DI创建数据库连接池对象

例:
```
<bean id="ds" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/newdb3"></property>
    <property name="username" value="root"></property>
    <property name="password" value=""></property>
    <property name="maxActive" value="10"></property>
    <property name="initialSize" value="2"></property>
</bean>
```

## Spring表达式

>Spring表达式可以读取JavaBean的属性，底层采用 OGNL 表达式解析。

1. 读取bean属性
	
		beanid.name
		beanid.age

2. 读取 数组元素

		arr[1] 
		arr[2]

3. 读取 list

		list[1] 

4. 读取map
		
		map['key']
		map.key 

5. 读取propeties

		cfg['key']
		cfg.key

6. 以上情况的符合使用

		bean.list[1].names[1]

### Spring读取properties

>利用Spring表达式和Properties配合，可以读取Properties文件，利用Properties文件参数注入基本值

>util:properties 标签可以读取Properties文件，返回的Bean类型是Properties。

例:
```
<!-- 读取jdbc.properties -->
<util:properties id="jdbc" location="classpath:jdbc.properties" />

Properties cfg=ctx.getBean("jdbc", Properties.class);
```

## 自动注入 autowire

>Spring 会自动根据Bean ID 或者Bean的类型自动完成Bean属性注入。 可以大幅减少编码提高编程效率。 由于采用默认规则，理解起来有点费劲。

**1. 按照BeanID自动注入**
	- Spring中声明的Bean组件有Bean ID
	- Bean 属性有名字
	- 如果指定了 autowire="byName" 则Spring会自动的将 Bean ID和Bean属性名进行比对，如果一样就进行注入。 无需编码完成。 

例:
```
<bean id="tool" class="day02.Axe"></bean>
<bean id="saw" class="day02.Saw"></bean>
<bean id="qiang" autowire="byName" class="day02.Worker"></bean>

<!-- qiang 包含Bean属性"tool" 设定autowire="byName"以后Spring会自动将Bean属性与Bean ID进行匹配，如果一样就自动注入-->
```

**2. 按照Bean类型自动注入**
	- Spring 容器中声明了Bean组件
	- Bean属性有类型
	- 如果指定了 autowire="byType" 则Spring会自动的将Bean属性类型与Spring内部的Bean组件逐一匹配类型，如果类型一致或者兼容（子类型）则自动注入Bean属性。 如果类型相同的Bean组件有多个，就是抛异常！

例:
```
<!-- 按照类型自动注入 ，与对象ID无关-->
<bean class="day02.Axe"></bean>

<!-- Worker组件包含一个Bean属性tool其类型是Tool类型。设定 autowire="byType"以后Spring会自动寻找与Tool类型兼容的Bean组件，找到后，就自动将组件注入到Bean组件中。找不到不进行注入，找到重复的组件，就会报错误 -->
<bean id="qiang" class="day02.Worker" autowire="byType"></bean>
```

## Spring 注解

**特点：**

1. 注解可以被编译检查，可以避免错误
2. Spring注解采用默认规则配置，可以减少编码量。
	- 不直观，抽象，不便于理解。

### 使用组件注解

>组件注解： 在类上声明“组件Componment”注解，Spring在执行期间会自动将类创建为SPring容器中的bean组件，并且会自动命名。 命名规则是类名首字母小写。

例:
```
@Componment
public class Foo{

}
```

**需要在xml配置文件中，开启组件扫描功能。 组件扫描功能是告诉Spring扫描特定包中的class，扫描到 @Componment 就创建Bean对象。**

例:
```
<!-- 开启Spring组件扫描功能，扫描day03包，Spring在启动时候，会自动扫描day03包中的类，如果扫描到标注在类上的@Component组件，就会自动创建该类的对象，并且自动设定BeanID为类名首字母小写。 -->

<context:component-scan base-package="day03"></context:component-scan>
```

案例步骤：

1. 创建Maven项目，导入Spring
2. 创建一个类 Foo 标注 @Componment 
3. 创建 Spring 配置文件，开启组件扫描功能，扫描 Foo 的包。
4. 创建测试案例，初始化Spring
5. 从Spring中获得 foo 对象。

## 声明Bean的注解(IOC)

>如下注解都可以声明Bean组件，作用的对等的!

```
1. @component 通用组件
2. @Service 业务层组件
3. @Controller 控制器组件
4. @Repository 持久层组件
```

#### 自定义组件ID

例:
```
@Component("myKoo")
```

## 属性注入

注解版本属性注入:

1. 注解版本属性注入支持 对象属性和Bean属性注入。
	1. 可以直接给对象的实例变量注入属性
	2. 可以向set方法注入属性值
2. 注解版属性注入时候默认就是 自动注入，并且会自动的先后按照byName，byType注入。 优点是节省代码！！！
3. 具体注解：自动属性注入
	- @Autowired 工作时候，会先按照类型在spring匹配组件，如果找到重复的组件，在尝试按照名字匹配。 
	- @Resource 工作时候，会先按照名字匹配Bean组件，如果名没有匹配上，就按照类型匹配，如果类型匹配失败，就报错误。


#### Spring 支持 Oracle 提供的Java EE注解

pom.xml配置:
```
<dependency>
    <groupId>javax.annotation</groupId>
    <artifactId>javax.annotation-api</artifactId>
    <version>1.3.2</version>
</dependency>
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
```

#### Scope 注解

>默认情况下，Spring按照单例管理Bean组件，注解版本的Bean组件也是单例的。使用Scope注解可以声明多例对象。

例:
```
@Component
@Scope("prototype")
class Goo{

}
```

#### XML 配置的Bean组件和注解配置的bean组件

1. xml 配置的组件和注解配置组件具有相等的作用。
2. 注解版本的组件配置，适合于自己写的代码。
3. xml版本配置适合配置第三方组件
4. xml配置的组件和注解配置的组件和相互调用，相互注入。
5. 注解不能脱离容器使用！！



