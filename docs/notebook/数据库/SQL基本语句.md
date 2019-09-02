### 数据库

## DBMS(DataBaseManagementSystem)

>数据库管理系统(数据库软件)

- 常见的DBMS:MySQL、Oracle、DB2、SQLserver等

|MySQL|Oracle公司，08年被sun公司收购，09年sun被Oracle收购，开源项目，MariaDB(MySQL分支)，市场排名第一|
| :--: | :-- |
|Oracle|Oracle公司，闭源，价格最高性能最高|
|SQLserver|微软公司 市场占有率排第三|
|DB2|IBM公司 闭源产品|
|SQLite|轻量级数据库 应用在移动设备或嵌入式设备中|


##### 开源和闭源

1.开源:开放源代码，免费,通过卖服务盈利
2.闭源:靠产品卖钱

## 连接MySQL

1.Linux系统:桌面右键 中端  mysql -uroot -p
  **-u后面时用户名 -p后面秘密**
2.windows系统:开启菜单 - 所有程序 - MySQL/MariaDB - MySQL client - 输入密码回车

- 退出数据库:exit或\q

## SQL

>Structured结构化 Query查询 Language语言，结构化查询语言，用于程序员和DBMS(数据库软件)之间的交流

### 数据库相关SQL

>想要在数据库软件中保存数据，需要先建库再建表,最后往表中保存数据

1.查询所有数据库
- 格式：show databases

2.创建数据库
- 格式:create database 数据库名 character set utf8/gbk;
- 举例:create database db1 character set utf8;

3.查看数据库详情
- 格式:show create database 数据库名;
- 举例:show create database db1;

4.删除数据库
- 格式:drop database 数据库名;
- 举例:drop database db1;

5.使用数据库
- 格式:use 数据库名;
- 举例:use db1;

### 表相关SQL

1.创建表
- 格式:create table 表名(字段名 类型，字段名 类型);
- 举例:create table person(name varchar(10),age int(3));

2.查看所有表
- 格式:show tables;

3.查看表详情
- 格式:show create table 表名;
- 举例:show create table person;

4.创建表制定字符集
- 格式:create table 表名(字段名 类型，字段名 类型) charset=utf8/gbk;

5.查看表字段
- 格式:desc 表名;
- 举例:desc student;

6.删除表
- 格式:drop talbe 表名;
- 举例:drop talbe person;

7.修改表名
- 格式:rename table 原名 to 新名

8.添加表字段
- 最后面添加:alter table 表名 add 字段名 类型;
- 最前面添加:alter table 表名 add 字段名 类型 first;
- 某个字段后面:alter table 表名 add 字段名 类型 after xxx;

9.删除表字段
- 格式：alter table 表名 drop 字段名;
- 举例:alter table p1 drop id;

10.修改字段名和类名
- 格式:alter table 表名 change 原名 新名 新类型;
- 举例:alter table p1 change name username varchar(10);

### 数据相关SQL

1.插入数据
- 全表插入格式：insert into 表名 values(值1，值2，值3);
- 指定字段插入格式:insert into 表名 (字段名1，字段名2) values(值1，'值2');

- 批量插入格式:
  1.insert into 表名 values(值1，值2，值3),(值1，值2，值3);
  2.insert into 表名 (字段名1，字段名2) values(值1，'值2'),(值1，'值2');

- 插入中文
  insert into emp values(7,'刘备',500);
  **如果执行以上代码报错，执行一下代码后再次执行上面代码**
  set names gbk;
  
2.查询数据
- 格式: select 字段信息 from 表名 where 条件;
	select * from emp;      //查询全部
    select name from emp;   //查询全部名字
    select name,sal from emp where id<5;  //查询id小于5的名字和工资

3.修改数据
- 格式:update 表名 set 字段1名=值，字段2名=值 where条件;
	update emp set name="ABC" where id=5;

4.删除数据
- 格式:delete from 表名 where 条件;
	delete from emp where id=5;

## 数据库中常见的数据类型

1.整数:int(m) 和 bigint(等效java中的long)

    m代表显示长度 需要结合zerofill使用
    create table t_int(id int,age int(10) zerofill);
    insert into t_int values(1,18);
    select * from t_int;

2.浮点数:double(m,d) m代表总长度 d代表小数长度

	decimal(m,d)超高精度浮点数，涉及超高精度运算时使用decimal

3.字符串
	
    1.char(m):不可变长度的字符串 m=5 abc 占5，执行效率略高，最大长度255。
    2.varchar(m):可变长度 m=5 abc 占3 节省内存，最大长度65535 但是超过255建议使用text
    3.text(m):可变长度 最大长度也是65535

4.日期
	
    date:保存年月日
    time:保存时分秒
    datetime:年月日时分秒 默认值为null 最大值9999-12-31
    timestamp:年月日时分秒 默认值为当前系统时间 最大值2038-01-19 03：14：07

#### Varchar和Char的区别

>Varchar是变长（长度可变）的，Char是定长(长度固定)的，在使用是，都需要显式的指定长度，否则，创建数据表的SQL语句就是错的

**以长度10为例(例如设置为varchar(10)或者char(10)，如果存入的数据是abc，在varchar中就只存入了abc,而在Char中，由于字符串的长度只有3，不足10个，会补7个空格达到10位!)**

```
使用char存入数据时，占用的实际空间就是字符乘以数量占用的空间，而使用Varchar时，由于存入的数据长度是不固定的，
MySQL还会另外再使用1个字节存储"实际字符数"，例如存入abc时，其实MySQL另外还使用1个字节存了数字3，表示实际存入
了3个字符！1个字节能表示的数字上限是255，如果在Varchar中存入的字符的数量超过255，MySQL会自动扩容使用2个字节
来记录“实际字符数”。
```




