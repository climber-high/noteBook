## Java开发环境

### Java编译运行过程
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

### 配置环境变量
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
```