### JDBC

>Java DataBase Connectivity:java数据库连接，实际上JDBC是sun公司提供一套连接数据库的API（Application Programma Interface应用程序编程接口） 
	
**为什么使用JDBC**:因为Java编程语言有可能需要连接多种数据库，如果没有JDBC，Java程序员每一种数据库都需要学习一条对应方法调用，换数据库时代码也需要做相应的改动，为了避免Java程序员学习多套操作数据的方法，Sun公司提供JDBC接口，让各个数据库厂商根据此接口写实现类（驱动），这样的话Java程序员只需要掌握JDBC接口中方法的调用，就可以访问任何数据，而且换数据库时代码不需要做任何的改动。

### 使用JDBC

```
1.新建Maven项目
2.导入jar包,在pom.xml添加
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.6</version>
    </dependency>
</dependencies>
```

例:
```
//1.注册驱动
Class.forName("com.mysql.jdbc.Driver");

//2.获取连接对象
Connection conn =DriverManager.getConnection("jdbc:mysql://localhost:3306/newdb3","root","");

//3.创建sql执行对象
Statement stat = conn.createStatement();

//4.执行sql语句
String sql = "create table jdbct1(id int,name varchar(10))";
stat.execute(sql);

//5.关闭资源
conn.close();
```

## statement执行SQL语句的对象

##### 1.execute(sql):当执行数据库相关和表相关的SQL语句时使用该方法，返回值是布尔值类型，代表是否有结果集；

例:
```
//创建表
String sql = "create table jdbct1(id int,name varchar(10))";
stat.execute(sql);
```

##### 2.executeUpdate(sql):执行增删改相关SQL使用此赋哪个发，返回值为int类型,代表生效的行数

例:
```
//插入sql语句
String sql = "insert into emp(empno,ename) values(101,'Tom')";
stat.executeUpdate(sql);
System.out.println("插入完成!");
```

##### 3.executeQuery(sql) 执行查询的sql语句时使用此方法，返回值类型ResultSet,里面装着查询到的所有数据，然后遍历数据

例:
```
String sql = "select * from emp";
ResultSet rs = stat.executeQuery(sql);
//遍历结果
while(rs.next()) {
    String name = rs.getString("ename");
    double sal = rs.getDouble("sal");
    System.out.println(name+"="+sal);
}
```

## ResultSet获取数据的两种方式
1. rs.getString("字段名");
2. rs.getString(字段的位置);

#### 配置数据库文件*.properties

例:获取配置文件的值
```
//创建属性对象
Properties p = new Properties();
//获取文件流                        //类加载器          //获取资源文件
InputStream ips = DBUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
//把流加载到属性对象中
p.load(ips);
//从属性对象中获取数据
String driver = p.getProperty("driver");
String url = p.getProperty("url");
String username = p.getProperty("username");
String password = p.getProperty("password");
```

## 数据库连接池 DBCP(DataBaseConnectionPool)

>为什么要用:如果不使用连接池，一万次业务请求需要和数据库服务器建立一万次连接，需要一万次的开关连接，这样会非常浪费资源，通过数据库连接池可以将连接重用，避免资源的浪费

**如何使用:**
```
1.在maven项目pom.xml引入DBCP相关jar包

<dependency>
  <groupId>commons-dbcp</groupId>
  <artifactId>commons-dbcp</artifactId>
  <version>1.4</version>
</dependency>
```

例:
```
//创建连接池对象
BasicDataSource ds = new BasicDataSource();
//设置连接信息
ds.setDriverClassName("com.mysql.jdbc.Driver");
ds.setUrl("jdbc:mysql://localhost:3306/newdb3");
ds.setUsername("root");
ds.setPassword("");
//设置初始连接数量
ds.setInitialSize(3);
//设置最大连接数量
ds.setMaxActive(5);
//获取连接
Connection conn = ds.getConnection();
System.out.println(conn);
```
## SQL注入

>往本来应该传进去值的位置 添加进去了SQL语句,这种行为称为SQL注入

例:
```
select count(*) from jdbcuser where username='asdfasdf' and password='' or '1'='1'

注入的内容:or '1'='1'
```

## PreparedStatement(预编译的SQL)

>预编译的SQL执行对象,编译时将SQL语句的逻辑锁死，不会出现SQL的风险

**好处：**代码结构整洁，不易出错（拼接字符串易出错），可以避免SQL注入
**什么时候使用:**当SQL语句中有变量时使用PreparedStatement，反之则使用Statement

例:
```
Class.forName("com.mysql.jdbc.Driver");
Connection conn = DriverManager.getConnection(url,username,password);

String sql = "select count(*) from user where username=? and password=?";
//创建预编译的SQL执行对象
PreparedStatement ps = conn.prepareStatement(sql);
//替换掉SQL语句中的?
ps.setString(1, username);
ps.setString(2, password);
//执行SQL语句
ResultSet rs = ps.executeQuery();
```

## 批量执行(addBatch,executeBatch)

>将多条SQL语句的多次数据传输，合并成一次数据传输，从而提高执行效率。

例:
```
String sql="insert into emp(empno,ename) values(?,?)";
PreparedStatement ps = conn.prepareStatement(sql);

for(int i=1;i<=100;i++) {
    ps.setInt(1, 20000+i);
    ps.setString(2, "name"+i);
    //添加到批量操作
    ps.addBatch();
    //为了避免内存溢出 每20次执行一次
    if(i%20==0) {
        //执行批量操作
        ps.executeBatch();
    }
}
//执行批量操作
ps.executeBatch();
```

## 分页查询

例:
```
String sql = "select empno,ename from emp limit ?,?";  //跳过多少条，查询多少条
PreparedStatement ps = conn.prepareStatement(sql);
ps.setInt(1, (page-1)*count);
ps.setInt(2, count);
ResultSet rs = ps.executeQuery();
while(rs.next()) {
    int eno = rs.getInt("empno");
    String ename = rs.getString("ename");
    System.out.println(eno+":"+ename);
}
```

## 获取自增主键的值

例:
```
创建球队表时把id当成主键，当成球员一个字段的外键来建立关系。获取球队表球队名所对应的id，放到
球员表的另一个字段中

String sql = "insert into team values(null,?)";
PreparedStatement ps = conn.prepareStatement(sql,Statement.RETURN_GENERATED_KEYS);//返回生成的主键

//替换内容
ps.setString(1, teamName);
ps.executeUpdate();
//获取自增的球队id
ResultSet rs = ps.getGeneratedKeys();
while(rs.next()) {
    int teamId = rs.getInt(1);
    //保存球员
    String sql2 = "insert into player values(null,?,?)";
    PreparedStatement ps2 = conn.prepareStatement(sql2);
//				把球员名字和球队id替换进去
    ps2.setString(1, playerName);
    ps2.setInt(2, teamId);
    //执行SQL
    ps2.executeUpdate();
}
```

## 元数据

1.数据库元数据：数据库相关的各种信息(DatabaseMetaData)

例:
```
DatabaseMetaData dbmd = conn.getMetaData();
System.out.println("数据库名:"+dbmd.getDatabaseProductName());
System.out.println("驱动版本:"+dbmd.getDriverVersion());
System.out.println("用户名:"+dbmd.getUserName());
```


2.表的元数据： 表相关的各种信息(ResultSetMetaData)

例:
```
//获取表的元数据
Statement stat = conn.createStatement();
String sql = "select * from emp";
ResultSet rs = stat.executeQuery(sql);
//得到表元数据对象
ResultSetMetaData rsmd = rs.getMetaData();
//获取表字段数量
int count = rsmd.getColumnCount();
//获取表字段名称和类型
for (int i = 0; i < count; i++) {
    String name = rsmd.getColumnName(i+1);
    String type = rsmd.getColumnTypeName(i+1);
    System.out.println(name+":"+type);
}
```


