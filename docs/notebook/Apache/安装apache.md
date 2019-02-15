## 安装apache

### 下载地址
```
https://www.apachelounge.com/download/
```

### 安装服务
```
下载后的apache下的文件夹bin路径下cmd
安装服务：httpd -k install -n "Apache"
卸载服务： httpd ‐k uninstall ‐n "Apache"
```

### 安装后报错
```
根据多少行把Apache的路径填正确
E:/work/exercise/forepart/php/Apache24/bin  然后把错误信息删除

再执行httpd ‐t

在#ServerName www.example.com:80下加上
ServerName localhost
命名
再执行httpd ‐t

配置完，再执行httpd ‐k restart ‐n "Apache"
```

### 开启服务等命令
```
开启服务
httpd ‐k start ‐n "Apache"

重新启动 Apache 服务
httpd ‐k restart ‐n "Apache"

停止 Apache 服务
httpd ‐k stop ‐n "Apache"

```