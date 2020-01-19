### HTTPS通讯

>HTTPS 基于SSL加密的HTTP通讯

1. 底层是SSL加密的TCP协议
2. 应用层还是传统的HTTP编程
3. 默认的通讯端口是443
4. 需要去CA申请证书，配置到服务器上。

#### 让阿里云帮域名申请CA证书

>在安全，申请CA证书服务(代理)，购买证书，购买后补全信息

1. 输入证书绑定的域名

2. 填写详细信息

**域名验证类型:DNS。根据解析的域名绑定证书**

>申请后下载证书，上传到服务器

```
1.创建文件夹:
mkdir /usr/local/nginx/conf/cert（把.key跟.pem文件放到这个目录下）

2.更改nginx.conf
include 文件名.conf

3.可以在nginx的配置文件夹conf添加一个文件，就可以不用在里面写一大串代码
server{
	server_name t3.climber.com; （不用加https）
	...
    
	location / {
        //路径 /usr/local/nginx/t3
        root t2;
        index index.html
    }
}

4. 在服务器上添加文件/usr/local/nginx/t3/index.html

5.测试并重新启动nginx

6.访问测试 https://t3.climber.com

注意:
可以在配https配置文件时可以加上：
server{
	listen 80;
    server_name t3.climber.com;
	return 301 https://t3.climber.com;（301永久重定向，搜索引擎友好）
	//或 rewrite ^(.*)$ https://$host$1 permanent;
}
如果输入的域名没有加https用80端口访问的话就重定向到https://t3.climber.com;
302临时重定向
```




