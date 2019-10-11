>事务是保障同一个业务中的多条增、删、改**要么全部成功，要么全部失败**的机制！

在执行过程中，万一出现意外，导致第1条Update语句执行成功，第2条却没有执行或执行失败，则会导致数据安全问题。使用事务，可以保证这2条SQL语句要么都执行成功，要么都执行失败！则数据是安全的！

在执行事务过程中，如果出错，可以回滚**(rollback)**，则相当于撤消已经执行的操作，如果全部成功，可以提交**(commit)**，则会完成对硬盘中数据库文件的写入！

>事务特点:

- 在 MySQL 中只有使用了 Innodb 数据库引擎的数据库或表才支持事务。
- 事务处理可以用来维护数据库的完整性，保证成批的 SQL 语句要么全部执行，要么全部不执行。
- 事务用来管理 insert,update,delete 语句

>事务是必须满足4个条件（ACID）：：原子性（Atomicity，或称不可分割性）、一致性（Consistency）、隔离性（Isolation，又称独立性）、持久性（Durability）。

1. 原子性：一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。

2. 一致性：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。(一致性要依赖于原子性和隔离性)

3. 隔离性：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。

4. 持久性：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。


在业务方法之前添加**@Transactional**注解，就可以保证该方法是以事务的方式来运行的，如果执行过程中出错，就会自动回滚，如果全部执行结束也没有出错，则会自动提交！

>在框架中，关于事务的执行机制大致是：

```
try {
    调用业务方法
    提交(commit)
} catch (RuntimeException e) {
    回滚(rollback)
}
```

所以，为了保证事务机制能够生效，必须：

1. 所有增删改操作后，必须及时判断受影响的行数，如果受影响的行数不是预期值，应该抛出RuntimeException或其子孙类异常；

2. 如果某个业务涉及2次或更多次增删改(例如2次Update，或1次Delete加1次Update，或1次Insert加1次Delete)，必须在业务方法之前添加@Transactional注解！

**另外，该注解也可以添加在业务类的声明之前，则表示该类中所有业务方法都是以事务的方式来运行的！一般没有这个必要，所以，在需要使用事务的方法之前添加该注解即可！**

## MYSQL 事务处理主要有两种方法：

1. 用 BEGIN, ROLLBACK, COMMIT来实现
```
BEGIN 开始一个事务
ROLLBACK 事务回滚
COMMIT 事务确认
```

2. 直接用 SET 来改变 MySQL 的自动提交模式:
```
SET AUTOCOMMIT=0 禁止自动提交
SET AUTOCOMMIT=1 开启自动提交
```

#### select @@tx_isolation;

>查询当前会话隔离级别(默认REPEATABLE-READ)

#### 设置当前会话隔离级别

>set session transaction isolation level read uncommitted

```
1.read uncommitted : 读未提交(对方未提交但自己仍然看到对方修改后的数据)

2.read committed : 读已提交,不可重复读取 (对方未提交,自己看不到对方修改后的数据，对方commit后才会看到修改数据)

3.repeatable read : 可重复读取(对方提交后,自己也看不到对方修改后的数据，要自己commit后才会看到修改数据)

4.serializable : 序列化(隔离级别最高)
```

## 脏读

>一个事务读到了另外一个事务尚未提交的数据

```
解决脏读:可以将事务的隔离级别设置为read committed就可以解决脏读的问题
```

## 不可重复读取

>在一个事务当中，对于同一条记录，两次读取的数据不一样

```
可以将事务的隔离级别设置为repeatable read就可以解决不可重复读取问题，
另外，该隔离级别也解决了脏读问题。
```

## 虚读(幻影读取)

>在一个事务(A事务)当中，读取到了一些记录，此时，另外一个事务(B事务)添加了一些符合A事务查询条件的记录。这个时候，A事务再一次查询，发现多个一些记录

```
可以将事务的隔离级别设置为serializable(序列化)，该隔离级别最高，该隔离级别也解决
```

## 视图

>在已有的表或者视图上创建的虚拟表

#### 如何创建视图

>create view 视图名 as select ...

```
1. create view v_staff as select * from staff;

2. create view v_staff2(name,age) as select name,age from staff;

3.create view v_staff3(sname,salary,age,dname) as  //因为两个表都有一个name，所以要进行区分，也可以取别名
    select s.name,salary,age,d.name from staff s join dept d on s.dept_id=d.id;
```

#### 向视图当中插入数据

>原有的表也会收到影响

```
insert into staff values();
```




