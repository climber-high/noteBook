### Object

## toString()

>通常我们定义的类如果需要使用到toString方法时,就应当重写这个方法，
Object提供的输出的是该对象的**句柄**，没有什么实际意义

```
Point p = new Point(1,2);
String str = p.toString();
System.out.println(str);    //Object.Point@7852e922  类名@地址
```

**toString方法会被很多API调用。所以我们定义一个类时，很常见的操作就是重写这个方法。
该方法的意义是将当前对象转换为一个字符串形式。该字符串内容格式没有严格的要求，
原则为包含这个对象的相关属性信息**

例:
```
public class Point {
	//重写toString()方法
	public String toString() {
		return "("+x+","+y+")";
	}
}

main:
	Point p = new Point(1,2);
	System.out.println(p);    //(1,2)

//System.out.println(Object obj);
该方法会将给定对象的toString方法，返回的字符串输出到控制台
```

## equal()

>equals的作用是比较当前对象与参数对象的内容是否一致

例:
```
public class Point {
	//重写equals()方法
	public boolean equals(Object obj) {
		if(obj==null) {
			return false;
		}
		if(this==obj) {
			return true;
		}
		//判断obj是否为Point类型
		if(obj instanceof Point) {
			Point p = (Point)obj;
			return this.x==p.x && this.y==p.y;
		}
		return false;
	}
}

main:
	Point p2 = new Point(1,2);
	System.out.println(p.equals(p2));

```

>我们定义的类如果使用equals，就应当重写这个方法。Object提供的equals方法本身内部就是用"=="进行比较的，没有实际意义
java API提供的类，toString,equals方法都妥善的进行了重写

