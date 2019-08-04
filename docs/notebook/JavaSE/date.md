### 日期类(java.util.Date)

>Date的每一个实例用于表示一个具体的时间点，内部维护一个long值，表示UTC时间即:1970-01-01 00:00:00到当前Date表示的时间之间经过的毫秒。由于Date存在时区和千年虫问题，所以大部分方法在JDK1.1时就被声明为过时的（都被Calendar取代）。

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

## Calendar

>java.util.Calendar类用于封装日历信息，其主要作用在于其方法可以对时间分量进行运算,可以调整时间，获取时间信息，计算时间等。

**Calendar是抽象类，其具体子类针对不同国家的日历系统，其中应用最广泛的是GregorianCalendar(通用的阳历)，对应世界上绝大多数国家/地区使用的标准日历系统**

##### Calendar提供了一个方法:int get(int field)

>可以获取当前Calendar指定时间分量对应的值

**不同的时间分量用一个不同的int值表示，Calendar提供了大量的int型常量表示不同的值，直接引用即可。**

例:
```
Calendar calendar = Calendar.getInstance();

//获取年
int year = calendar.get(Calendar.YEAR);

//获取月，从0开始
int mouth = calendar.get(Calendar.MONTH)+1;

//获取日和"天"相关的时间分量
//DAY_OF_WEEK：周中的天，星期几
//DAY_OF_YEAR:年中的第几天
//DAY_OF_MONTH:月中的天，跟DATE一样

int day = calendar.get(Calendar.DATE);
int day = calendar.get(Calendar.DAY_OF_MONTH);

int hour = calendar.get(Calendar.HOUR_OF_DAY); //获取小时，24小时制
int minute =calendar.get(Calendar.MINUTE);  //获取分钟
int second = calendar.get(Calendar.SECOND); //获取秒数

//获取星期几(得到的数是从星期日0开始)
int week = calendar.get(Calendar.DAY_OF_WEEK)-1;

//获取给定的时间分量所允许的最大值getActualMaximum()
int maxDay = calendar.getActualMaximum(Calendar.DAY_OF_YEAR);
//365
```

## Date getTime()

>返回Date类型。

**set方法调用后不是立即调整Calendar的时间分量，只有在getTime时才会更新。但是有些时间分量会影响相同的内容这会出现冲突，后者会替代前者的修改**


## void set(int field , int value)

>调整当前Calendar指定时间分量为给定的值(设置具体的时间)

例:
```
Calendar cale = Calendar.getInstance();
cale.set(Calendar.YEAR, 2010);
```

## void add(int field,int amount)

>对指定的时间分量加上给定的值，若给定的值是负数则是减去操作。(当前再加上的时间)






