## 购买云服务器

```
1.地域:国内配置域名时候必须备案，国外服务器无需备案
2.配置:x86服务器，1CPU 1G 内存
3.镜像:公共镜像 CentOS 7.4
4.网络:1M
5.安全组(防火墙):开放端口 80，443，22
  - 注意:没有开放 8080 需要以后配置

6.设置root密码
7.设置服务器名
```

## 服务器环境初始化

>下载相关www.oracle.com

```
1.安装配置JDK
2.安装配置Tomcat
3.安装配置MySQL
4.将本地MySQL 迁移到远程服务器
5.将Java WEB 应用部署到远程服务器
6.为服务器配置域名，Nginx
```

**服务器中默认又yum**

#### 配置Java环境
	
1. 从Oracle网站下载Linux x64 JDK (.tar.gz)
2. 将jdk释放到/usr/local/java

**在Oracle下载jdk不要用wget 下载地址下载，解压时会失败**


	su(输入管理员密码)
    mkdir /usr/local/java
    cd /usr/local/java
    tar -xzvf /home/soft01/jdk1.8.0_144.tar.gz
    在/usr/local/java文件夹中解压/home/soft01/中的jdk
    
3. 配置Java环境变量(以后开机后就会自动配置)
    
    
    su   (切换管理员)
    cd etc/
    cp profile profile.2019  (备份)
    vim profile （修改profile文件）
    
    在最后添加下面三句:
    export PATH=/usr/local/java/jdk1.8.0_144/bin:$PATH
	export JAVA_HOME=/usr/local/java/jdk1.8.0_144
    export CLASSPATH=.

	1.source profile(加载资源)

	2.javac -version
      java -version

	3.echo $PATH

	4.which javac 看有没有配置成功再重启电脑
    
    5.reboot (重新启动电脑)
    
4. 执行java文件

	
    javac HelloWorld.java  (编译成.class文件)
    java HelloWorld (加载主类)
    
    
#### 安装配置Tomcat

**Tomcat有两种安装方式**

1. yum 安装

```
yum -y install tomcat

默认tomcat安装到的位置:/usr/share/tomcat

启动和关闭:
启动命令: systemctl start tomcat.service
停止命令: systemctl stop tomcat.service
重新启动: systemctl restart tomcat.service
设置开机启动: systemctl enable tomcat.service
关闭开机启动: systemctl disable tomcat.service
```

2. 使用原厂的包下载Tomcat。(tomcat.apache.org)
	
```
可以安装yum -y install wget
wget 加上tomcat下载地址
省去把tomcat下载下来再放到服务器
```

#### 解压zip文件并配置Tomcat

>安装 yum -y install unzip

1. 解压zip文件:

```
unzip apache-tomcat-7.0.67.zip
```

2. 将tomcat安装到 /usr/local

```
mv apache-tomcat-7.0.67 /usr/local
```

3. 设置启动脚本的执行权限

```
cd /usr/local/apache-tomcat-7.0.67/bin
chmod +x *.sh   //为.sh脚本添加执行权限
```

4. 启动和关闭Tomcat

```
./startup.sh
./shutdown.sh
```

5. 检查是否启动

```
ps -A|grep java
```

6. 开放防火墙8080端口

```
开启防火墙 systemctl start firewalld
关闭防火墙 systemctl stop firewalld
查看防火墙状态 systemctl status firewalld

firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload
```

7. 测试

```
http://ip:8080
```

8. 云服务器开启8080端口

```
在管理，本实例安全组，配置规则，添加安全组规则，输入端口范围8080/8080，授权对象:0.0.0.0/0全开放
```

#### 安装配置 MySQL

>MySQL 已经分支为两个版本

1. Oracle 维护 MySQL (www.mysql.com,社区版免费Community server)
2. 开源社区维护 MariaDB(mariadb.org)

**yum安装MariaDB:**
```
yum -y install mariadb mariadb-server
安装客户端跟服务端两个组件
```

**启动/停止MySQL:**
```
启动命令: systemctl start mariadb.service
停止命令: systemctl stop mariadb.service
重新启动: systemctl restart mariadb.service
设置开机启动: systemctl enable mariadb.service
关闭开机启动: systemctl disable mariadb.service
```

**运行mysql看编码是否为utf-8**
```
show variables like '%char%'; 

character_set_database的编码,默认latin1
character_set_server的编码,默认latin1
```

**设置mysql的中文编码**
```
1.进入etc文件夹: cd /etc
2.cp my.cnf my.cnf.2019 备份文件
3.修改里面的my.cnf文件: vim my.cnf
4.在文件里第一个#那行上面面加上: character-set-server=utf8
5.重启数据库: systemctl restart mariadb.service
6.设置开机启动: systemctl enable mariadb.service
7.再检查编码是否正确：show variables like '%char%'; 
```

#### 数据库迁移

>导出(备份)数据库

	mysqldump -uroot -proot data_db>data_db_日期.sql

>导入数据库

    1. 用sftp把sql文件put到服务器上,退出
    2. 用ssh登录服务器
    3. 创建新数据库，使用数据库，用source +路径，导入数据


#### 将Java WEB应用部署到远程服务器

1. 打包成.war文件

```
项目右键Export -> WAR file
```

2. put到服务器

3. 用cp把war文件复制到/usr/local/apache-tomcat/webapps文件夹下

```
tomcat会自动把.war文件部署好
```

4. 修改连接数据库的配置文件

```
webapps/项目名/WEB-INF/classes/db.properties
vim db.properties
```

5. 去tomcat目录的bin文件夹下重启tomcat

```
关闭tomcat
./shutdown.sh

看tomcat还有没有在运行
ps -A|grep java

启动tomcat
./startup.sh

看是否运行
ps -A|grep java
```

6. 查看日志

```
tomcat的bin文件夹下apache-tomcat/logs/catalina.out
```

## 输出重定向

>将一个命令的输出目标从标准控制台(标准输出)重新定向到其他设备(一般是一个文件)

```
将终端输出的结果写到指定的文件中
ls>demo.txt  创建文件并写到demo.txt里面，如果有这个文件就覆盖

ls>>demo.txt 创建文件并写到demo.txt里面，如果有这个文件就在后面追加

ls />>abc.txt
```

**用途:**
```
1.记录命令的执行日志
	tar -czvf demo.tar.gz demo>demo.log

2.快速生成文本文件
	echo "Hello World" > hello.txt
    (vi就麻烦)
```

## 文件的权限

>ls -l

```
 文件权限  连接数 当前用户 用户所在组 文件大小 日期               文件名
drwxrwxr-x. 2   soft01  soft01   4096   9月  28 16:35      demo
-rwxr-xr-x. 1   soft01  soft01    245   9月  25 2017  eclipse.desktop

d rwx rwx r-x
d:第一个字母为-的是文件，第一个字母为d的是文件夹
第一个rwx:当前拥有者的权限，可读(r)写(w)和执行或进入文件夹(x)
第二个rwx:当前拥有者的同组用户权限
r-x：与当前用户不同组的其它人的权限

soft01  soft01
第一个soft01:文件的拥有者
第二个soft01:文件的拥有者所在组
```

>chmod -x(r/w) 文件夹  （管理权限）

```
mkdir 文件夹
chmod -x 文件夹  //把进入文件夹的权限去掉，之后进入文件夹就会权限不够
chmod +x 文件夹  //把进入文件夹的权限加上

更仔细的为三块设置权限:

chmod u+x,g-x,o-x 文件/文件夹

u:user(第一组:拥有者);
g:group(第二组:拥有者的分组);
o:other(第三组:其他不同组的用户)

二进制写法:(三个数相加得到的结果为一个组所有对应权限)
rwx 421
--- 000
--x 001
、、、

d rwx rwx r-x (利用数字设置文件权限)
设置对应的权限:chmod 775 文件/文件夹
```

## 可以执行的文件

>Linux中可以执行的文件

1. 文件是可以执行的**2进制程序**或者是**可执行的脚本程序**
2. 文件具有可以执行的权限

>可以执行的脚本：也称为shell脚本，是一个文本文件，文件的每一行都是可以执行的shell命令。如果有执行权限，这个文件就可以执行，执行时候批量执行文件中每个命令，经常用于自动化运维。


例(自动化脚本):
```
1.创建文件 vim Hello.sh
2.编辑文件:
echo 'public class HelloWorld{'>HelloWorld.java
echo '	public static void main(String[] args) {'>>HelloWorld.java
echo '		System.out.println("Hello");'>>HelloWorld.java
echo '	}'>>HelloWorld.java
echo '}'>>HelloWorld.java
cat HelloWorld.java
javac HelloWorld.java  //编译生成.class文件
java HelloWorld  //执行.class文件

3.开放执行权限
chmod +x Hello.sh;

4.执行
./Hello.sh
```

## PATH 变量的作用

>操作系统可执行命令的搜索路径，操作系统在执行命令时候，会在PATH变量中的一系列路径中逐一查找命令程序，如果找到就执行这个程序，否则将报出“命令没有找到”错误。

	echo $PATH 回显PATH变量的值
    which who 查找一个命令所在的位置


**export PATH=/usr/local/java/jdk1.8.0_144/bin:$PATH**
文件路径拼接上:$PATH

**echo $PATH** 就看到拼接到前面

**/usr/local/java/jdk1.8.0_144/bin:**/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/home/soft01/.local/bin:/home/soft01/bin:/home/soft01/.local/bin:/home/soft01/bin

用**which javac**就能找到存在哪个位置**/usr/local/java/jdk1.8.0_144/bin/javac**

## Linux 操作系统的引导过程

主板加电，主机自检，磁盘引导，Linux引导程序，加载内核，初始化脚本(/etc/profile) export PATH=?，用户初始化脚本，GUI/终端，export

**当终端断开时export配置的环境变量就会没有，需要重新配置，可以把export从终端放到初始化脚本，开机就配置PATH**



