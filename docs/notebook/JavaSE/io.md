### IO(input,output）输入和输出

1.IO是我们的程序与外界交换数据的方式。java提供了一种统一的标准的方式与外界交换数据。
2.java将流按照功能划分为读和写，并用不同的方向来表示，其中输入流(外界到程序的方向)用于读取数据输出流用于写出数据

>java将流划分为两大类:节点流，处理流

1.节点流:也称为低级流，是实际链接程序与数据源的"管道"，负责实际搬运数据。读写一定是建立在节点流的基础上进行的。
2.处理流:也称为高级流，不能独立存在，必须链接在其他流上，目的是当数据流经当前流时对这些数据做某些处理，这样可以简化我们对数据的操作

**实际应用中，我们是链接若干高级流，并最终链接低级流，通过低级流读写数据，通过高级流对读写的数据进行某些加工处理，完成一个复杂的读写操作。这个过程称为"流链接"。这也是学习IO的精髓所在**

>文件流

文件流是一对低级流，用于读写文件。就功能而言它们与RandomAccessFile一致。但是底层的读写方式有本质区别。

1.**RAF**是基于指针进行**随机读写**的，可任意读写文件指定位置的数据。可以做到对文件部分数据的编辑操作。
2.**流**是**顺序读写**方式，所以不能做到任意读写指定位置数据，对此也无法做到对文件数据进行编辑的操作。但是配合高级流，可以更轻松的读写。

## 文件输出流 FileOutputStream(String path，bollean append) 向文件写出字节文件

>注意:该构造方法进行写出后再重新写新内容会覆盖前一次的所有内容，如果不想被覆盖，构造方法加上第二个参数bollean append变成**追加写模式**，若指定的文件存在，那么数据全保留，通过该流写出的数据会被追加到文件最后。

例:
```
FileOutputStream fos = new FileOutputStream("fos.txt");
String str="ab";
byte[] data = str.getBytes("UTF-8");
fos.write(data);
System.out.println("写出完毕");
fos.close();
```

## 文件输入流 FileInputStream(String path)

例:
```
FileInputStream fis = new FileInputStream("fos.txt");
byte[] data = new byte[100];
int len = fis.read(data);

//new String()构造方法，还可以读取byte[]指定长度的字节，填入第二跟第三个参数，防止有空白字符串
String str = new String(data,0,len,"UTF-8");
System.out.println(str);
fis.close();
```

## 文件流复制文件操作

>使用文件输入流读取原文件,使用文件输出流向复制文件写数据，利用块读写操作顺序从原文件将数据读取出来写入复制文件完成复制操作

例:
```
FileInputStream fis = new FileInputStream("1.png");
FileOutputStream fos = new FileOutputStream("2.png");

int len=-1;
byte[] data = new byte[1024*10];
while(((len=fis.read(data))!=-1)) {
    fos.write(data,0,len);
}

System.out.println("复制完成");
fos.close();
fis.close();
```

## 缓冲流(BufferedInputStream 和 BufferedOutputStream)

>缓冲流是一对高级流，功能是提高读写效率。链接他们以后，无论我们进行随机读写还是块读写，当经过缓冲流时都会被转换为块读写操作

例:
```
FileInputStream fis = new FileInputStream("1.png");
FileOutputStream fos = new FileOutputStream("2.png");

BufferedInputStream bis = new BufferedInputStream(fis);
BufferedOutputStream bos = new BufferedOutputStream(fos);

int d = -1;
while((d=bis.read())!=-1) {
    bos.write(d);
}
bis.close();
bos.close();
```

>注意:缓冲流的write方法并不是立即将数据写出，而是先将数据存入其内部的数组中，当数组装满时才会做一次真实写操作。(转化为块写操作的过程)

>flush（）方法的意义是强制将缓冲流已经缓存的数据一次性写出。这样做可以让写出的数据有即时性，但是频繁调用会降低写效率，在更关注写出的即时性时应当使用该方法。

>close方法中会调用一次flush方法

## 对象流

>对象流也是一对高级流，提供的功能是读写java中的任何对象

>对象输出流ObjectOutputStream，它可以将给定的java对象转换为一组字节然后通过其连接的流将这些字节写出

>当一个类的实现希望可以被对象流进行读写，那么该类必须实现Serializable接口

例:
```
import java.io.Serializable;
public class Preson implements Serializable{
	private String name;
	private int age;
	private String gender;
	private String[] otherInfo;
    ...get和set方法
    public String toString() {
		return name+","+age+","+gender+","+Arrays.toString(otherInfo);
	}
}


Preson p = new Preson();
p.setName("admin");
p.setAge(18);
p.setGender("男");

String[] otherInfo = {"abc","ccc","rrr","666"};
p.setOtherInfo(otherInfo);

FileOutputStream fis = new FileOutputStream("data.obj");
ObjectOutputStream oos = new ObjectOutputStream(fis);
oos.writeObject(p);
System.out.println("写出完毕");
oos.close();
```

>通过对象流写出对象的这个方法经历了两个步骤:

1.对象流先将给定的对象转换为了一组字节，这组字节包含对象本身保存的数据信息，还包含该对象的结构信息然后将这组字节通过其连接的流写出（对象序列化）
2.经过文件流时，文件流将这组字节写入到了文件中（将数据写入磁盘做长久保存的过程：数据持久化）

>对象输入流(进行对象的反序列化操作)

**使用对象流读取的字节必须时通过对象输出流序列化的一组字节才可以**
例:
```
public static void main(String[] args) throws IOException, ClassNotFoundException{
	FileInputStream fis = new FileInputStream("data.obj");
	ObjectInputStream ois = new ObjectInputStream(fis);
	Preson p = (Preson)ois.readObject();
	System.out.println(p);
	ois.close();
}
```







