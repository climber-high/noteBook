#### mysql -u root -p
>连接本机数据库服务器

>1.linux版:直接回车(因为没有，密码)
2.window:输入 密码root再回车(当然你的密码不是root是用自己的密码)

#### SHOW DATABASES; 

>(查看所有数据库)

#### DROP DATABASE IF EXISTS db1;

>删除库db1

#### CREATE DATABASE DB1 CHARSET utf8;

>创建数据库（window数据库库名不区分大小写，而linux系统区分大小写）

#### USE DB1;

>选中一个数据库使用

#### 创建一个表(建表之前先选中一个数据库 USE 数据库名)
例:
```
CREATE TABLE stu(
 id(字段名) INT(数据类型),
 name VARCHAR(9), #VARCHAR是可变字长
 GENDER CHAR(9),  -- CHAR是字长不可变
 birthday DATE
);
```

#### mysql中的注释有
```
/*注释*/
-- 两个横线之后要带个空格()单行注释
# 单行注释
```

#### 查看表的结构
```
语法: DESC 表名;
例: DESC stu;
```

#### 查看所有的表
```
语法:SHOW TABLES;
```

#### 删除表
```
DROP TABLE stu;
```

www.bilibili.com/video/av5354244/?p=1




