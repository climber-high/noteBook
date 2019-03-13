## Java开发环境

## Java编译运行过程
```
1.编写的java源文件(.java)首先要经过编译(通过javac命令编译原文件)，生成所谓字节码文件(.class);
2.java程序的运行需要JVM的支持。JVM是一个软件，安装在操作系统中为字节码文件提供运行环境;
  JVM 加载 .class 并 运行.class
    跨平台、一次编程到处使用
```

### 名词解释
```
1.JVM:java虚拟机 (加载 .class 并 运行.class)

2.JRE:java运行环境(除了包含JVM以外还包含了运行java程序所必须的环境)
	  JRE=JVM+系统类库

3.JDK:java开发工具包
	  除了包含JRE以外还包含了开发java程序所必须的命令工具
	  JDK=JRE+编译、运行等命令工具
	  
运行java程序的最小环境为JRE
开发java程序的最小环境为JDK

安装JDK地址：https://www.oracle.com/technetwork/java/index.html
```

## 配置环境变量
```
1.JAVA_HOME:指JDK的安装路径
2.CLASSPASS:表示类的搜索路径，一般写为.
3.PATH:执行JDK下的bin目录

在window系统中通过控制面板-系统-高级系统设置的环境变量追加path路径
如：D:\Program Files\Java\ jdk1.6.0_24
设置完成后打开命令行 键入java或javac命令，看见正常提示就表示环境变量配置成功
```

### Eclipse集成开发环境
```
下载地址：www.eclipse.org/downloads

文档下载地址：http://www.oracle.com/technetwork/java/javase/downloads/index.html
```

## 开发java程序的步骤
```
1.新建java项目
2.新建java包
3.新建java类
```

## 基本写法
```
package day;  //声明包day

public class HelloWorld {  //声明类HelloWorld
    //主方法，为程序的入口
	//程序的执行从main开始，main结束则程序结束
	public static void main(String[] args) {
		//严格区分大小写
		//所有符号必须是英文模式
		//每句话必须以分号结尾
		//println(); 输出并换行
		//print(); 输出不换行
		System.out.println("Helloworld");
		System.out.print("123");
	}
}
```