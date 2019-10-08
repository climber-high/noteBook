### Redis

#### 数据库分类:

```
1.关系型数据库:存储表，使用sql操作，也称为结构化数据库
	常见产品:MySQL Oracle SQLServer DB2

2.非关系型数据库(非结构化数据库,NoSQL)
	- 泛指不使用SQL操作的数据库
    - 文档数据库:存储文档 MongoDB 用于存储JSON文档
    - key_Value 数据库，核心原理就是 散列表 ，查询性能奇高,为提升查询性能而生!
      经常作为关系型数据库的缓存使用，常见产品:MemoryCache Redis
```

>Redis是一种非关系型KV数据库

1. 是内存型数据库，同时提供了磁盘持久存储
2. Redis 采用散列表技术，查询性能高，可以达到千万级并发。
3. Redis 经常作为 关系型数据库的缓存使用
4. Redis支持5种数据类型。

#### Redis 提供例5种数据类型

>Redis 为5种数据类型提供的对应的一组操作命令

1. Sring类型命令
2. Hash类型命令
3. List类型命令
4. Set类型命令
5. Sorted Set类型命令

## Redis的安装

>yum安装

```
yum -y install redis

启动:systemctl start redis.service
检查:ps -A|grep redis
```

>编译安装(redis.io/download)

```
1.下载: wget 路径
2.解压redis文件到文件夹中 tar -xzf redis-3.0.0.tar.gz
3.进入解压好的redis-3.0.0文件
4.创建文件夹：mkdir /usr/local/redis
4.make (进行编译)
5.make PREFIX=/usr/local/redis install (进行安装)
6.文件夹中就多了个bin文件夹
7.复制配置文件 cp redis.conf /usr/local/redis
```

>添加到环境变量里(哪里都可以使用redis)

```
export PATH=/usr/local/redis/bin:$PATH
echo $PATH
which redis-server
```

>启动redis服务器

```
cd (回到主目录)
redis-server &

&符号的作用是将程序设置为后台执行
```

>启动客户端连接到Redis(TCP端口:6379)

```
redis-cli
```

>关闭redis服务器

```
redis-cli shutdown
```

## Redis命令

>命令参考redisdoc.com

**key的类型永远是字符串，存储到数据库中的Value的类型就以下5种**

#### Sring类型命令(key value)

```
1. set message “hello” EX 10   
(EX 10 为过10秒后过期)(set方法只能1个key_value)

2. get message  (获取键的值)

3. append message "java" (在hello添加java)

4. strlen message (获取字符串的长度)

5. mset message2 "m2" message3 "m3"
(mset可以一次加入多个key_value)

6. mget message2  message3
(mget可以一次抓取多个key返回多个value)

7. incr score（自增1，要求值为字符串的数值）
(如果set score 59的value的是数字，可以incr score自增1返回自增1的值，get score就是加1后的值)

8. incrby score 10 (可以加大于1的数)

9. decr score (自减1)

10. decrby score 5 (减大于1的值)

11. set price 12.56  ; incrbyfloat price 1.9
(可以增加或减少(-10.5)浮点数的值)

12. keys * (查找所有的键名)

13. del message (删除某个)
```

#### Hash类型命令

```
key为字符串
value为Hash，Hash又有一个key-value

1.hset loginUser name tom  (添加)
  hset loginUser password 123 (添加)
  
2.hget loginUser name （获取单个）

3.hmget loginUser name password （获取多个）

4.hkeys loginUser (获取hash里的key)
```


#### List类型命令(队列)

>List类型本质上一个缓存队列

```
lpush queue 123
lpush queue 456

rpop queue (返回"123")
rpop queue (返回"456")

LLEN queue (队列的长度)

BRPOP queue 60 (等待60s是否有数据进入)
lpush queue "HI" (一执行BRPOP也会立刻执行)
```

**相关命令:**
```
LPUSH(从左侧开始放一个值)
LPUSHX(从左侧开始放多个值)
LPOP（从左侧取一个值）
LLEN (队列的长度)
LINSERT （从中间插入）
LSET （更改）
BLPOP，BRPOP (阻塞，如果队列没数据就等待，直到超时)
```

#### Set类型命令

>是一个集合，不重复的，无序的

```
SADD (增加数据)
SPOP (删除数据)
```

#### Sorted Set类型命令

>有序集合

## Jedis

>Java Redis Client API

**java连接redis**

```
创建maven项目，导包:
<dependency>
    <groupId>com.redislabs</groupId>
    <artifactId>jedis</artifactId>
    <version>3.0.0-m1</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>

使用:
Jedis jedis = new Jedis("localhost");  //ip
jedis.set("foo", "bar");
String value = jedis.get("foo");
System.out.println(value);
jedis.close();
```

## 多个tomcat服务器使用redis共享session

>1.把三个jar包放进tomcat服务器的TOMCAT_BASE/lib文件夹中

```
tomcat-redis-session-manager-VERSION.jar
jedis-2.5.2.jar
commons-pool2-2.2.jar
```

>2.修改tomcat里面的conf/context.xml

```
<Valve className="com.orangefunction.tomcat.redissessions.RedisSessionHandlerValve" />
<Manager className="com.orangefunction.tomcat.redissessions.RedisSessionManager"
         host="localhost" <!-- redis服务器位置 -->
         port="6379"
         database="0"
         maxInactiveInterval="60" <!--redis失效时间-->
/>
```

>3.写jsp文件进行测试

```
在tomcat的 webapps/ROOT/ 下创建jsp文件
```



