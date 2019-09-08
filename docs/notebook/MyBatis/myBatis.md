### MyBatis

>MyBatis是一种持久层框架，用于处理持久化存储数据，即将数据存入到数据库中，后续还可以从数据库中取出所需的数据，或对这些数据进行修改或删除！

**作用:**之所以会出现持久层框架，是因为传统的JDBC技术使用过于繁琐，且操作过程相对固定，无论是做增删改查中的哪种操作，都是先建立连接(Connection)，设计好SQL语句，就获取Statement/PreparedStatement对象，然后执行，执行完毕后，将处理结果，如果是增加或查询操作，可能需要处理ResultSet，如果是增删改操作，还会得到受影响的行数，以此判断操作成功与否，处理完毕之后，还需要关闭或归原连接，以释放资源。

==常见的持久层框架有MyBatis、Hibernate，其主要作用都是简化持久层开发。以MyBatis为例，只需要开发人员编写操作所对应的抽象方法及SQL语句即可，无需关注实现过程。==

## 使用MyBatis+Spring的项目

>在pom.xml添加依赖

```
<!-- MyBatis框架 -->
<!-- 备选版本号：3.5.0，3.4.0~3.4.6 -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.1</version>
</dependency>

<!-- MyBatis整合Spring -->
<!-- 备选版本号：2.0.0，1.3.0~1.3.2 -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.1</version>
</dependency>

<!-- MySQL -->
<!-- 备选版本号：8.0.12~8.0.15 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.16</version>
</dependency>

<!-- DBCP -->
<dependency>
    <groupId>commons-dbcp</groupId>
    <artifactId>commons-dbcp</artifactId>
    <version>1.4</version>
</dependency>

<!-- 单元测试 -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>

<!-- Spring框架，其实并不一定需要完整的WebMVC -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>4.3.8.RELEASE</version>
</dependency>

<!-- Spring JDBC -->
<!-- 注意：需要使用与spring-webmvc相同的版本 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>4.3.8.RELEASE</version>
</dependency>
```

> MyBatis框架是可以独立使用的，如果结合Spring一起使用，可以简化大量的配置，目前使用Spring框架开发项目已经是行业的标准，所以，通常会将这2个框架一起使用，而极少单独使用MyBatis却不使用Spring。

#### 1.在**src/main/resources**下创建**db.properties**文件，用于配置数据库连接相关信息：

    url=jdbc:mysql://localhost:3306/climber_ums?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Chongqing
	driver=com.mysql.cj.jdbc.Driver
	username=root
	password=root
	initialSize=2
	maxActive=5

#### 2.尝试获取数据库连接对象

>首先，应该在**spring-dao.xml**中加载**db.properties**中的配置：

```
<!-- 读取db.properties中的配置 -->
<util:properties id="config" location="classpath:db.properties" />
```

>在`commons-dbcp`中，定义了`BasicDataSource`数据源类，是实现了`DataSource`数据源接口的，需要把以上读取到的配置信息都注入到数据源类中，才可以利用数据源类去连接数据库，从中获取数据库的连接对象！所以，在**spring-dao.xml**中添加配置：

	<!-- 配置数据源 -->
	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
		<property name="url" value="#{config.url}" />
		<property name="driverClassName" value="#{config.driver}" />
		<property name="username" value="#{config.username}" />
		<property name="password" value="#{config.password}" />
		<property name="initialSize" value="#{config.initialSize}" />
		<property name="maxActive" value="#{config.maxActive}" />
	</bean>

>完成后，可以添加单元测试，尝试通过以上配置的类的对象获取数据库连接对象，以检验数据库连接信息是否正确：

	@Test
	public void getConnection() throws SQLException {
		ClassPathXmlApplicationContext ctx
			= new ClassPathXmlApplicationContext("spring-dao.xml");

		BasicDataSource ds = ctx.getBean(
			"dataSource", BasicDataSource.class);
		Connection conn = ds.getConnection();
		System.out.println(conn);
		ac.close();
	}

#### 3.抽象方法

>先创建`cn.climber.mybatis.User`类，在该类中设计与数据表中相同的6个属性：

例:
```
public class User {
	如果用int类型接收，如果查到的字段没有数据则返回0，不确定是真的是0还是没有值
    private Integer id;
    private String username;
    private String password;
    private Integer age;
    private String phone;
    private String email;
    // 生成SET/GET/toString()
}
```

>在MyBatis中，要求访问数据的抽象方法都定义在接口中，所以，必须先创建`cn.climber.mybatis.UserMapper`持久层接口，并在接口中定义“增加用户数据”的抽象方法，在MyBatis中抽象方法的设计原则是：

- 如果执行的SQL语句将是增、删、改类型的操作，应该将返回值声明为`Integer`类型，表示受影响的行数；

- 方法名称可以自定义，但是，不允许重载；

- 参数列表根据执行的SQL语句中需要多少个参数，就设计多少个参数。

例:
```
public interface UserMapper {
    public abstract Integer addnew(User user);  //增加新用户方法
    Integer deleteById(Integer id);          //删除对应id用户方法
}
```

>完成后，还需要添加配置，使得MyBatis框架知道接口文件在哪里，需要配置的类是`MapperScannerConfigurer`，则在**spring-dao.xml**中添加配置：

```
<!-- 配置MapperScannerConfigurer -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <!-- 接口文件在哪里 -->
    <property name="basePackage" value="cn.climber.mybatis" />
</bean>
```

#### 4.配置SQL语句

>配置mapper.xml文件

```
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper PUBLIC "-//ibatis.apache.org//DTD Mapper 3.0//EN"      
 "http://ibatis.apache.org/dtd/ibatis-3-mapper.dtd">
```

>在配置该文件时，首先添加根节点，并配置当前文件对应的接口文件：

```
<!-- namespace：当前XML对应接口文件是哪个 -->
<mapper namespace="cn.climber.mybatis.UserMapper">

</mapper>
```

>然后，根据将要执行的SQL语句的类型选择子节点，例如此次需要执行的是“增加用户数据”的操作，将执行`INSERT`语句，所以，需要选择`<insert>`节点，节点中的`id`属性表示对应的抽象方法名称：

    <!-- namespace：当前XML对应接口文件是哪个 -->
    <mapper namespace="cn.climber.mybatis.UserMapper">
        <!-- id：对应的抽象方法的名称 -->
        <insert id="addnew"></insert>
    </mapper>

>然后，在`<insert>`节点内部配置需要执行的SQL语句，参数部分使用`#{}`作为占位符，括号内是`User`类中的属性名

	<insert id="addnew">
        INSERT INTO t_user (
            username, password, 
            age, phone, 
            email
        ) VALUES (
            #{username}, #{password}, 
            #{age}, #{phone}, 
            #{email}
        )
    </insert>

>完成后，还需要继续添加配置，使得MyBatis框架知道这些XML文件在哪里，并配置使用哪个数据源去连接数据库，执行配置的SQL语句，需要配置的类是`SqlSessionFactoryBean`，则在**spring-dao.xml**中添加配置：

	<!-- 配置SqlSessionFactoryBean -->
	<bean class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 使用哪个数据源 -->
		<property name="dataSource" ref="dataSource" />
		<!-- XML文件在哪里 -->
		<property name="mapperLocations" value="classpath:mappers/*.xml" />
	</bean>

>完成后，可以编写并执行单元测试：

public class Tests {
    public ClassPathXmlApplicationContext ac;
    @Before
    public void doBefore() {
        ac = new ClassPathXmlApplicationContext("spring-dao.xml");
    }

    @Test
    public void addnew() {
    	//接口名字小写userMapper
        UserMapper userMapper = ac.getBean("userMapper", UserMapper.class);
        User user = new User();
        user.setUsername("Alex");
        user.setPassword("8888");
        Integer rows = userMapper.addnew(user);
        System.out.println("rows=" + rows);
    }

    @After
    public void doAfter() {
        ac.close();
    }
}

## MyBatis配置查询时的返回值

>在配置XML时，如果使用了`<select>`节点，必须在该节点配置`resultType`或`resultMap`其中的1个属性，以指定返回值类型！

例:
```
<select id="countAll" resultType="java.lang.Integer">
    SELECT COUNT(*) FROM t_user
</select>
```

## 使用多个参数的问题

>MyBatis框架在处理抽象方法的参数时，只能识别1个参数！当在抽象方法中添加多个参数(超过1个)时，默认情况下，会出现BindingException

例如：
```
org.mybatis.spring.MyBatisSystemException: nested exception is org.apache.ibatis.binding.BindingException: Parameter 'email' not found. Available parameters are [arg1, arg0, param1, param2]
```

**如果某个方法必须要使用多个参数，可以在每一个参数之前添加@Param注解，并在注解中定义参数的名称**

例如：
```
public interface UserMapper {
    Integer updateEmailById(@Param("id") Integer id,@Param("email") String email);
}

MyBatis框架在运行时，会将调用方法时的多个参数值结合声明时注解中的名称封装为1个Map，然后再执行！
```

**总结:**
```
当设计的抽象方法的参数超过1个时，必须为每一个参数之前添加@Param注解，在注解中指定名称，并且，在配置XML中的映射时，使用的#{}占位符中的也是注解中配置的名称！
```

## 动态SQL--foreach

>动态SQL是MyBatis框架的特性之一。动态SQL指的是在配置XML中的SQL语句时，可以在其中添加一些特殊的标签，使用之具有逻辑判断或循环的功能，最终，在执行时，根据参数的不同，生成的SQL语句会是不一样的！

假设需要实现批量删除功能，例如根据多个id一次性删除多条数据，则，抽象方法可以设计为：

```
Integer deleteByIds(Integer[] ids);
```

需要执行的SQL语句大致例如：

```
DELETE FROM t_user WHERE id IN (2,4,9);
```

其中的2,4,9是需要根据调用方法的参数来决定的，如果调用方法时，给出的数组是{1,3,5,7,9}，则生成的就应该是1,3,5,7,9，即由5个数字和4个逗号共同组成的，如果数组的值包括12、18、25，则生成的应该是12,18,25，所以，最终生成的SQL语句中的部分应该是根据数组进行遍历，取出数组中的每一个元素，并且，保证各元素值之间使用逗号进行分隔！则在配置XML的映射时，需要使用<foreach>节点对数组进行遍历：

```
<delete id="deleteByIds">
    DELETE FROM 
        t_user 
    WHERE 
        id IN (
        <foreach collection="array"
            item="id" separator=",">
            #{id}
        </foreach>
        )
</delete>
```

>关于<foreach>节点的配置：

- collection：当匹配的抽象方法只有1个参数时，如果该参数是List集合类型时，该属性取值为list，当参数是数组类型时，该属性取值为array，另外，当抽象方法的参数超过1个时，肯定必须配置@Param注解，此处需要配置的属性值就是@Param注解中配置的名称；

- item：遍历过程中，获取到的元素的变量名，是自定义的名称；

- separator：遍历过程中，生成的SQL语句中各元素之间的分隔符号；

- open / close：遍历生成的SQL语句部分的最左侧字符串和最右侧字符串。

## 动态SQL--if

动态SQL的特性是可以根据参数不同，生成不同的SQL语句，<if>主要是用于判断，基于这样的特性，可以设计一个单表查询的通用查询功能：

	List<User> find(String where, String orderBy, Integer offset, Integer count);

在配置XML映射时，可以：

	<select id="find" resultType="xx.xx.xx.Xxx">
        select * from t_user where #{where} order by #{orderBy} limit #{offset},#{count}
    </select>

为了保证调用方法时没有给出某些参数SQL语句也不会出现语法错误，则应该在其中添加一些<if>标签，以对这些参数进行基本的判断：

	<select id="find" resultType="cn.climber.mybatis.User">
        SELECT 
            id,username,password,age,phone,email 
        FROM 
            t_user 
        <if test="where != null">
        WHERE
            ${where}
        </if>
        <if test="orderBy != null">
        ORDER BY 
            ${orderBy}
        </if>
        <if test="offset != null">
        LIMIT #{offset}, #{count}
        </if>
    </select>

**使用<if>时，其中的test就是判断的表达式，如果在表达式需要使用参数，直接写参数名即可，无需添加特殊符号或占位符。**

>在动态SQL中的`<if>`并没有匹配的`else`，如果一定要形成`if...else...`的结构，需要使用`<choose>`系列节点，其基本结构是：

    <choose>
        <when test=""></when>
        <otherwise></otherwise>
    </choose>

这种使用方式其实并不直观，也可以通过如下方式来实现：

    <if test="where != null">
        ...
    </if>
    <if test="where == null">
        ...
    </if>
    使用2个<if>时需要执行2次判断，效率略低。

## #{}和${}占位符

>在配置MyBatis中的XML映射时，可以使用#{}或${}格式的占位符。

使用#{}格式的占位符用于表示某个值，在实际处理时，是预编译的，也就是MyBatis会先把#{}格式的占位符使用问号?表示，并直接编译该SQL语句，编译完成后，执行时，再把问号?对应的值填入并执行SQL语句！而使用${}格式的占位符可以表示SQL语句中的任何部分，在实际处理时，不是预编译的，MyBatis会先将占位符对应的值与原本配置的SQL语句进行拼接，然后再编译，最终执行！

**能够在SQL语句使用?表示的部分，都应该使用#{}格式的占位符，表示的是某个值，是会被预编译的，而不能通过?表示的部分，都必须使用${}格式的占位符，会先根据占位符的值拼接出SQL语句再编译！**

## 查询时字段名称类属性名不一致的解决方案

在MyBatis执行查询时，默认情况下，会把查询结果中列名与返回结果类型中的属性名对应的值进行封装，例如查询结果中存在username这一列，就会把这一列的数据封装到User类中名为username的属性中！也就是说，如果希望能够自动封装查询结果，需要查询结果的列名与类的属性名完全一致！

在分析查询相关问题，必须严格区分3个名词：字段(Field)、列(Column)、属性(Property)。在设计数据表时，表中需要存入哪些数据，就设计了哪些字段；在查询时，查询结果类似一个表格，每一竖排就是一列；类中的全局变量是属性。

>在设计SQL语句时，为名称不一致的列定义别名，使之与类中的属性名保持一致即可：

```
<select id="findAll"
    resultType="cn.climber.mybatis.User">
    SELECT 
        id,username,
        password,age,
        phone,email,
        is_delete AS isDelete
    FROM 
        t_user 
    ORDER BY 
        id ASC
</select>
```

>另外，还可以配置<resultMap>来解决名称不一致的问题！该配置的作用是指导MyBatis如何将查询结果封装到类的对象中

```
<!-- id：为当前resultMap配置取名 -->
<!-- type：封装查询结果的数据类型 -->
<resultMap id="UserMap"
    type="cn.climber.mybatis.User">
    <id column="id" property="id"/>
    <result column="username" property="username"/>
</resultMap>

在配置<resultMap>时，其子级节点可以全部使用<result>进行配置，但是，对于主键的配置应该使用<id>节点，利用MyBatis缓存查询结果。
```

当定义了`<resultMap>`之后，在配置`<select>`节点时，就可以不必再配置其中的resultType属性，而是配置resultMap属性，属性值就是以上<resultMap>节点中配置的id，例如：

```
<select id="findById" resultMap="UserMap">
    ...
</select>
```

## 多表联合查询

>由于User或Department这种类都是与数据表结构相对应的，这种类通常称之为实体类，实体类必然不适用多表联合查询的！所以，涉及多表联合查询时，需要为查询结果自定义VO(Value Object)类，该类中的属性设计可以完全参考查询的需求，例如：

VO类与实体类只是定位不同，实体类是与数据表相对应的，而VO类是与查询结果相对应的，除此以外，在编写代码方面没有任何区别。

所以，抽象方法：
```
UserVO findVOById(Integer id);
```

配置映射:
```
<select id="findVOById"
    resultType="cn.climber.mybatis.UserVO">
    SELECT
        t_user.id, username,
        password, age,
        phone, email,
        is_delete AS isDelete,
        department_id AS departmentId,
        name AS departmentName
    FROM
        t_user
    LEFT JOIN
        t_department
    ON
        t_user.department_id=t_department.id
    WHERE
        t_user.id=#{id}
</select>
```

## 在MyBatis中的1对多查询

**根据部门的id，查询部门的信息，并且，显示出该部门所有用户的基本信息。**

1. 在设计抽象方法之前，还需为本次的查询结果设计对应的VO类：
```
	public class DepartmentVO {
		private Integer id;
		private String name;
		private List<User> users;
	}
```

2. 然后，先创建`cn.climber.mybatis.DepartmentMapper`接口，并添加抽象方法：
```
	DepartmentVO findVOById(Integer id);
```

3. 配置mybatis的xml,添加根节点`<mapper>`的`namespace`属性，对应以上新创建的`DepartmentMapper`接口，然后，添加`<select>`节点，配置以上抽象方法的映射：
```
	<select id="findVOById" 
        resultType="cn.climber.mybatis.DepartmentVO">
        SELECT
            *
        FROM
            t_department
        LEFT JOIN
            t_user
        ON
            t_department.id=t_user.department_id
        WHERE
            t_department.id=#{id}
    </select>
```

4. 进行单元测试
```
    public class DepartmentTests {

        public ClassPathXmlApplicationContext ac;
        public DepartmentMapper departmentMapper;

        @Before
        public void doBefore() {
            ac = new ClassPathXmlApplicationContext("spring-dao.xml");
            departmentMapper = ac.getBean("departmentMapper", DepartmentMapper.class);
        }

        @Test
        public void findVOById() {
            Integer id = 1;
            DepartmentVO result = departmentMapper.findVOById(id);
            System.out.println(result);
        }

        @After
        public void doAfter() {
            ac.close();
        }
    }
```

5. 如果直接运行，可能会报告错误(如果凑巧查询的部门只有1个用户或没有用户暂时不会报错)，因为MyBatis不知道如何将多条查询结果封装到1个对象中，所以，需要配置`<resultMap>`，在配置之前，还应该调整查询时的SQL语句，将字段名相同的定义别名，以保证查询结果的列表都不相同，否则，MyBatis只能识别多个相同列名的第1个列！所以，关于查询的SQL语句需要调整为：
```
<select id="findVOById" resultMap="DepartmentVOMap">
    SELECT
        t_department.id AS did,
        name,
        username,
        t_user.id AS uid,
        password, age,
        phone, email,
        is_delete, department_id
    FROM
        t_department
    LEFT JOIN
        t_user
    ON
        t_department.id=t_user.department_id
    WHERE
        t_department.id=#{id}
</select>
```

6. 配置`<resultMap>`，此次的重点是需要使用`<collection>`节点配置那些1对多的数据
```
<resultMap id="DepartmentVOMap" type="cn.climber.mybatis.DepartmentVO">
    <id column="did" property="id"/>
    <result column="name" property="name"/>
    <!-- collection节点：用于配置1对多关系的数据，也可以理解为“配置List集合类型的数据” -->
    <!-- ofType：List集合中的元素的类型 -->
    <collection property="users"
        ofType="cn.climber.mybatis.User">
        <id column="uid" property="id"/>
        <result column="username" property="username"/>
        <result column="password" property="password"/>
        <result column="age" property="age"/>
        <result column="phone" property="phone"/>
        <result column="email" property="email"/>
        <result column="is_delete" property="isDelete"/>
        <result column="department_id" property="departmentId"/>
    </collection>
</resultMap>
```

### 关于MyBatis提示Invalid bound statement (not found)的解决方案

>出现这样的问题的主要原因在于MyBatis无法将接口中的抽象方法与XML中配置的节点绑定！可能的原因有：

1. 没有指定接口文件在哪个包中，或者，配置出错；

2. 没有指定XML文件在哪里，或者，配置出错；

3. 在XML中配置节点时，id值不是抽象方法的名称；

4. (概率较低)框架所需的jar包文件损坏，导致无法正常工作。













