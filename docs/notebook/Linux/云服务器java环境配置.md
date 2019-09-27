## 购买云服务器

1.地域:国内配置域名时候必须备案，国外服务器无需备案
2.配置:x86服务器，1CPU 1G 内存
3.镜像:公共镜像 CentOS 7.4
4.网络:1M
5.安全组(防火墙):开放端口 80，443，22
  - 注意:没有开放 8080 需要以后配置

6.设置root密码
7.设置服务器名

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

#### 配置Java环境
	
1. 从Oracle网站下载Linux x64 JDK (.tar.gz)
2. 将jdk释放到/usr/local/java


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
设置自动启动: systemctl enable tomcat.service
关闭自动启动: systemctl disable tomcat.service
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

***当终端断开时export配置的环境变量就会没有，需要重新配置，可以把export从终端放到初始化脚本，开机就配置PATH*

## 使用vim编辑文本文件

>www.gnu.org

基于命令行的全屏幕可视化编辑器

vi与vim

## 安装 vim

	yum -y install vim
	-y 不用咨询你就直接安装vim

#### vim命令

1. vim


	vim  (vim版本)
    vim 新文件 (创建新文件)
	vim 旧文件 (编辑文件)

2. 命令模式

	
    yy 复制当前行到剪切板
    5yy 复制5行到剪切板
	P(大写) 粘贴到当前行之前
	p(小写) 粘贴到当前行之后
	dd 删除(剪切)当前行
	5dd 删除(剪切)5行
    ?关键字 向前查找(支持正则)
    /关键字 向后查找(支持正则)
    n 查找下一个
    u 撤销
    Ctrl+r 恢复撤销

3. 插入模式(编辑文本文件内容)esc退出


	i:在光标所在字符前开始插入
	a:在光标所在字符后开始插入
	o:在光标所在行的下面另起一新行插入
	s:删除光标所在的字符并开始插入


4. 末行模式(esc命令结束)


	:w [文件名] 另存
    :w 保存
    :q 退出
    :wq 保存并退出
    :q! 强制退出

**more 文件名查看内容**

