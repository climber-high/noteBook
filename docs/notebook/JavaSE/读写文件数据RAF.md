### 读写文件数据RandomAccessFile

>java.io.RandomAccessFile

**RAF是专门用来读写文件数据的API。其基于指针对文件数据进行读写操作，可以灵活的编辑文件数据内容**

**创建RAF时可以指定对该文件的权限，常用有两种: 1. r:只读模式  2. rw:读写模式**

**RAF创建时含有写权限的情况下，当指定的文件不存在时会自动将其创建出来**

```
import java.io.RandomAccessFile;
RandomAccessFile raf = new RandomAccessFile("raf.dat","rw");
//第一个参数可以是文件路径也可以传File实例化对象
```

## void write()(写文件)

>向文件中写入1字节，写的是给定int值对应二进制的"低八位"

```
RandomAccessFile raf = new RandomAccessFile("raf.dat","rw");
raf.write(97);
System.out.println("写出完毕");
raf.close();      //读或写完毕要执行close();
```

## int read()

>读取一个字节，并以int形式返回，若返回值为-1则表示读取到了文件末尾

```
RandomAccessFile raf = new RandomAccessFile("raf.dat","rw");
int d = raf.read();
System.out.println(d);  //97  只会返回0-255的数，如果raf.write(256)，则raf.read()为0

d = raf.read();        
System.out.println(d);  //如果整个文件只有一个字节，再进行读取下一个字节不存在的话返回-1
raf.close();
```

## 文件复制

>创建2个RAF,一个用来读原文件，一个用来往复制文件中写。顺序从原文件读取每个字节写入到复制文件中

例:
```
RandomAccessFile raf = new RandomAccessFile("test.txt","r");  //原文件
RandomAccessFile copy = new RandomAccessFile("testCopy.txt","rw");  //新建的文件

int d = -1; //记录每次读取到的字节
while((d=raf.read())!=-1) {
	copy.write(d);
}

System.out.println("复制完毕");
raf.close();
copy.close();
```

>若希望提高读写效率，可以通过提高每次实际读写的数据量，减少实际发生的读写操作来做到。

	单字节读写:随机读写
	一组字节读写:块读写
	机械硬盘(磁盘)的块读写效率还是比较好的，但是随机读写效率极差

## 块读写

>int read(byte[ ] data)

**一次性读取给定的字节数组长度的字节量并存入到该数组中。返回值为实际读取到的字节量，若返回值为-1，表示本次读取是文件末尾(没读到任何字节)**

>void write(byte[ ] data)

**一次性写出给定字节数组中的所有字节**

```
//记录每次实际读取到的字面量
int len=-1;
//每次要求读取的字节量
//10kb
byte[] data=new byte[1204*10];
while((len=txt.read(data))!=-1) {
	txt2.write(data,0,len);
}
```

### 新建文件并写入字符串

例:
```
RandomAccessFile raf = new RandomAccessFile("dat.txt","rw");
String str = "123";
raf.write(str.getBytes("UTF-8"));
raf.close();
System.out.println("写入完成");
```


### RAF读写基本类型数据，以及基于指针的操作

>RAF总是在指针当前位置或读或写，并且读写完毕后指针会自动向后移动，以备下次读写操作。

### 位移运算>>>,(将给定数字二进制整体向右移动指定位)

>在文件写入一个int最大值

例:
```
RandomAccessFile raf = new RandomAccessFile("raf.dat","rw");
int max = Integer.MAX_VALUE;
raf.write(max>>>24);
raf.write(max>>>16);
raf.write(max>>>8);
raf.write(max);
raf.close();
```
### void writeInt(int i)

>RAF提供了方便写出基本类型数据的方法(连续写出4个字节，将int的值写入文件)

其他基本类型方法也有:
1.void writeLong(long l)
2.void writeDouble(double d)

例:
```
RandomAccessFile raf = new RandomAccessFile("raf.dat","rw");
int max = Integer.MAX_VALUE;
raf.writeInt(max);
```

### int readInt()

>连续读取4个字节，将int值返回

**如果读的字节比文件末尾的字节数大，那么会报错java.io.EOFException（end of file）**

例:
```
RandomAccessFile raf = new RandomAccessFile("raf.dat","rw");
raf.writInt(1);   //连续写4个字节,写完再读要用void seek(指定位置)，不然有可能报错
raf.seek(0);
raf.readInt();    //1
```

### void seek(long pos)

>将指针移动到指定位置

例:
```
RandomAccessFile raf = new RandomAccessFile("raf.dat","rw");
raf.seek(0);
raf.writInt(1);
```

### long getFilePointer()

>返回当前RAF的指针位置

例:
```
RandomAccessFile raf = new RandomAccessFile("raf.dat","rw");
long a = raf.getFilePointer();
```