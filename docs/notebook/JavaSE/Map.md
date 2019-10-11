### java.util.Map 查找表

>Map是一个接口，常用的实现类:java.util.HashMap 散列表（由散列算法实现）

1.Map体现的结构是一个多行两列的表格，其中左列称为key，右列称为value。
2.Map总是成对保存数据的，并且总是根据key来获取对应的value。
3.因此我们可以将数据以value保存，查询条件以key形式保存

例:
```
Map<String,Integer> map = new HashMap<>();

//将给定的键值对存入到Map中。
map.put("语文", 99);
map.put("数学", 98);
System.out.println(map);

Integer i = map.put("英语", 97);  //返回值为null。
Integer y = map.put("语文", 77);  //返回值为99.

//获取键名的值
//V get(Object k)根据给定的key获取对应的value，如果给定的key不存在，则返回值为null。
Integer num = map.get("数学");    //返回值为98

int size = map.size();    //元素个数

//是否包含给定的key
boolean ck = map.containsKey("语文");

//是否包含给定的value
boolean cv = map.containsValue("97");

//将给定的key所对应的键值对删除。返回值为该key对应的value,key不存在就返回null
Integer num = map.remove("英语");
```

**注意**:Map有一个要求,key不允许重复(equals)。如果使用已有的key存放value,则会做替换value操作,此时返回值为原value.若key不存在,则返回值为null。

## Map的遍历

>遍历Map有三种方式:

**1：遍历所有的key。 Set keySet()**

将当前Map中所有的key以一个Set集合形式返回。
遍历这个集合就等于遍历了所有key。

```
Set<String> keySet = map.keySet();

for(String key : keySet) {
    System.out.println(key);
}
```

**2.遍历每一组键值对。  Set<Entry> entrySet()**

将当前Map中每一组键值对(若干Entry实例)以一个Set集合形式返回。

java.util.Map.Entry
Entry的每一个实例用于表示Map中的一组键值对其提供的getKey和getValue方法用于获取这组键值对中的key,value值。

```
Set<Entry<String,Integer>> entrySet = map.entrySet();
for(Entry<String,Integer> entry : entrySet) {
    String key = entry.getKey();
    Integer value = entry.getValue();
}
```

**3.遍历所有的value(相对不常用)  Collection calues()**

将当前Map中所有的value以一个集合形式返回

```
Collection<Integer> value = map.values();
for(Integer val : value) {
    System.out.println(val);
}
```

#### HashMap,Hashtable,ConcurrentHashMap的区别:

1. HashMap是线程不安全的，性能好。

**注：可以使用Collections.synchronizedMap方法将HashMap转换成线程安全的HashMap。**

2. Hashtable是线程安全的，性能不好。

3. ConcurrentHashMap是线程安全的，并且性能良好。

**结论：在多线程环境下，应该使用ConcurrentHashMap,否则使用HashMap。**




