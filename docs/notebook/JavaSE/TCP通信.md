### Socket

>java.net.Socket (套接字) 用于客户端连接服务器

封装例TCP协议的通讯细节，使得我们利用TCP通讯以流的读写操作形式完成

**Socket socket = new Socket("localhost",8088);**

客户端实例化Socket时需要传入两个参数,需要**异常处理**

1.服务端的地址信心(IP地址)
2.服务端打开的端口号

	这里实例化Socket的过程就是通过给定地址链接服务端的过程，若链接不成功构造方法会抛出异常。
	通过IP地址我们可以找到网络上服务端所在的机器，通过端口就可以找到运行在服务端计算机上的服务端应用程序了。
    
例:
```
import java.net.Socket; //引入类

private Socket socket;  

public Client() {
    try {
        System.out.println("正在连接服务器");
        socket = new Socket("localhost",8088);
        System.out.println("已连接服务器");

    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
**OutputStream getOutputStream()** Socket提供的方法
通过返回的字节输出流写出的字节会通过网络发送给远端计算机。

	OutputStream out = socket.getOutputStream();
    
	OutputStreamWriter osw = new OutputStreamWriter(out,"UTF-8");
    BufferedWriter bw = new BufferedWriter(osw);
    PrintWriter pw = new PrintWriter(bw,true);
    while(true) {
        System.out.println("请输入内容");
        String str = scanner.nextLine();
        if("exit".equals(str)) {
            socket.close();
            break;
        }
        pw.println(str);
    }

>java.net.ServerSocket  运行在服务端

主要有两个作用:
1.向系统申请服务端口，客户端就是通过这个端口与服务端建立连接的
2.监听该端口，这样当客户端建立连接时，ServerSocket就会自动实例化一个Socket，用于与该客户端进行数据交互。

**实例化ServerSocket时传入打开的服务端口。如果该端口已经被其他应用程序使用则实例化过程会抛出异常**

例:
```
import java.net.ServerSocket;

private ServerSocket server;

public Server() {
    try {
        System.out.println("正在启动服务器");
        server = new ServerSocket(8088);
        System.out.println("服务器启动完毕");

    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
**InputStream getInputStream()**通过该输入流读取到的内容是远端计算机发送过来的数据

	InputStream in = socket.getInputStream();
    
	InputStreamReader isr = new InputStreamReader(in);
	BufferedReader br = new BufferedReader(isr);
	String line = null;
    
       客户端由于操作系统不同，当客户端断开连接时服务端这里的表现也不同:通常windows客户端断开连接时，
       readLine方法这里会直接抛出异常.而linux的客户端断开连接时，readLine方法会返回为null。
    
    while((line=br.readLine())!=null) {
        System.out.println("客户端说："+line);
    }
    socket.close();
    System.out.println("服务端断开");
    

## ServerSocket提供的方法:Socket accept()

>该方法是一个阻塞方法，调用后开始等待客户端连接，当一个客户端连接后该方法会返回一个Socket实例，通过这个Socket实例即可与该客户端交互。

注意:多次调用accept方法可以接受多个客户端的连接

例:
```
import java.net.Socket;

Socket socket = server.accept();
```

## linux系统查本机ip地址
>/sbin/ifconfig













