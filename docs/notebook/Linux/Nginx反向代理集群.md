### Nginx反向代理集群

## 反向代理:

1. 正向代理是代理客户端上网
2. 反向代理是应用服务器提供网络服务

## Nginx反向代理原理

Nginx 提供了upstream模块，将用户请求交给服务器进行处理:

## 搭建步骤:

>规划服务器的角色用途

1. 先解析域名，配置客户端域名解析

2. 配置tomcat服务器

```
可以在/usr/local/apache-tomcat/webapps/ROOT文件夹修改index.jsp写上对应某段ip成功后可以在访问域名看到使用了哪台服务器

重启Tomcat,看是否可用
```

3. 在Nginx所在服务器修改配置文件

```
进入文件夹:/usr/local/nginx/conf/

修改主配置文件nginx.conf
添加另外配置include n.conf并添加配置

upstream tomcats {  //多个tomcat的ip
	server 10.6.12.10:8080;    //都要安装tomcat
	server 10.6.12.224:8080;
    server 10.6.12.117:8080;
}

server{
	listen 80;
    server_name t3.climber.com;
    location / {
    	proxy_pass http://tomcats
        proxy_redirect     off;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_max_temp_file_size 0;
        proxy_connect_timeout      90;
        proxy_send_timeout         90;
        proxy_read_timeout         90;
        proxy_buffer_size          4k;
        proxy_buffers              4 32k;
        proxy_busy_buffers_size    64k;
        proxy_temp_file_write_size 64k;
    }
}
```

4. 检查文件是否正确

```
/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
```

5. 重新启动ngnix

```
/usr/local/nginx/sbin/nginx -s reload
```
