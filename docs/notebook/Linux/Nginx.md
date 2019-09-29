### Nginx

>Nginx是俄罗斯程序员开发的一款高性能的web服务器

1. Nginx可以承受高并发，可以同时承受近100万请求。

2. 利用Nginx和Tomcat(应用服务器)组合搭建反向代理服务器集群，可以解决WEB高并发问题。

## web 服务器

>凡是能处理Http协议的服务器，统称为web服务器

1. Nginx Apache
2. MS IIS 
3. Java Web Server
	- Tomcat
	- JBoss
	- Oracle Weblogic
	- IBM WebSpare

## 安装Nginx

### yum安装

>yum -y install nginx

**启动命令**

```
systemctl start nginx.service
systemctl stop nginx.service
systemctl restart nginx.service
systemctl enable nginx.service
systemctl disable nginx.service
```

#### 配置文件

>/etc/nginx/nginx.conf

**web目录：**
```
/usr/share/nginx/html/index.html
```

检查:
```
ps -A | grep nginx
```

### 编译安装Nginx

>nginx.org

```
1.下载原文件
wget http://nginx.org/download/nginx-1.16.1.tar.gz

2.创建文件夹
mkdir /usr/local/nginx

3.释放解压
tar -xzvf nginx-1.16.1.tar.gz

4.添加nginx启动用户
useradd nginx

5.安装必须模块
yum -y install openssl openssl-devel pcre-devel gcc(可加可不加)
不行就openssl-*

6.进入文件夹(里面有configure配置文件)
cd nginx-1.16.1

7.执行configure文件(配置的参数http://nginx.org/en/docs/configure.html)
./configure --user=nginx --prefix=/usr/local/nginx --with-http_ssl_module

太长可以用"\"续行写

8.编译 安装
make
make install
```

## 启动nginx(80端口)

>nginx -c /usr/local/nginx/conf/nginx.conf

>/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

**编译安装的文件位置**

```
/usr/local/nginx/conf  配置文件位置
/usr/local/nginx/html  web网站位置
```

## 关闭nginx

>nginx -s stop

## Nginx 配置文件

>编译安装配置文件路径:/usr/local/nginx/conf/nginx.conf

>yum安装配置文件路径:/etc/nginx/nginx.conf

#### 通用参数

```
worker_processes 1;(CPU多少核就填多少)  进程

events {
    worker_connections  1024;  (1个cpu，线程池有1024线程) 线程
}

http{
	http协议通用参数
    
    include       mime.types;  //content-Type响应的文件类型
    
    //不知道文件什么类型的默认类型（会不分文件类型，随便的下载）
    default_type  application/octet-stream; 

	keepalive_timeout  65;  //http1.1，等待客户端在65s后没发送请求再断开连接

	#gzip  on; //IE6不支持，把信息压缩给客户端，客户端再解压，可以减少流量

	server{
    	虚拟主机参数
    }
    server{
    	虚拟主机参数
    }
}

```

#### 修改后测试文件是否修改成功，没有报错

```
nginx -t -c /usr/local/nginx/conf/nginx.conf
```

#### 文件修改后热(不停机)加载配置文件

```
nginx -s reload
```

## 虚拟主机

>将一台服务器作为多个web服务器使用，称为虚拟主机












