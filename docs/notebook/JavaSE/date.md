### 日期类(java.util.Date)

>Date的每一个实例用于表示一个具体的时间点，内部维护一个long值，表示UTC时间即:1970-01-01  00: 00:00到当前Date表示的时间之间经过的毫秒。由于Date存在时区和千年虫问题，所以大部分方法在JDK1.1时就被声明为过时的（都被Calendar取代）。

##### Date now = new Date();

>Date默认创建表示当前系统时间

##### long time = now.getTime();

>获取Date内部维护的毫秒

##### void date.setTime(long);

>填入毫秒数输出

例:
```
Date date = new Date();
date.setTime(0);

//Date重写了toString()方法，用一个字符串来描述当前Date对象所表示的时间。
System.out.println(date);
```

## SimpleDateFormat

>java.text.SimpleDateFormat

**该类可以按照一个指定的日期格式将Date与String进行互相转换。**

##### String format(Date data)

>将给定的Date对象表示的日期按照指定的日期格式转换为String

例:
```
Date date = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

//format可以传毫秒数long类型
String str = sdf.format(date);
System.out.println(str);
```

##### Date parse(String line)

>将一个字符串按照指定的日期格式解析为Date对象

例:
```
String line = "2012-07-05 18:09:44";
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
Date date = sdf.parse(line);
```







