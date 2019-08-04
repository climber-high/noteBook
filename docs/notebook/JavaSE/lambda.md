### lambda表达式

>JDK8之后推出的一个新特性，可以让java编码"以函数式编程"，最直观的感受就是可以用一个类似函数的形式创建匿名内部类

语法:
```
(参数)->{
	方法体
}

```

**注意:lambda不是所有的匿名内部类创建都可以使用，仅在创建接口中只有一个抽象方法时可用。**

例:(无参数无返回值)
```
//常规写法:
Runnable r1 = new Runnable() {
    public void run() {
        System.out.println("hello");
    }
};

//lambda写法1
Runnable r2 = ()->{
    System.out.println("hello");
};

//lambda 只有一行代码写法
Runnable r3 = ()->System.out.println("hello");
```

例:（有参数和返回值）
```
List<String> list = new ArrayList<>();
list.add("ab");
list.add("a");
list.add("abc");

Collections.sort(list,(o1 , o2)->o1.length()-o2.length());
```



