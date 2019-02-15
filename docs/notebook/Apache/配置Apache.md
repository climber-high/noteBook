## 配置Apache

### 配置文档
``` 
配置文档：http://httpd.apache.org/docs/curren
```

### 监听端口
```
监听端口可以随意修改为任意一个未被其他程序监听的端口，可以通过设置配置conf文件夹的文件 httpd.conf 
中57行左右的 Listen指令后面的数字修改。然后重启Apache服务
httpd ‐k restart ‐n "Apache"

先用httpd -t看是否成功

可以占多个端口
```

### 网站根目录
```
默认 Apache 的网站根目录是安装目录中的 htdocs 文件夹，为了方便对网站文件的管理，一般我们会将其设置在
一个自定义目录中（如果你不介意其实不修改也无所谓）。如果需要设置网站根目录，可以通过修改配置文件 
httpd.conf 中的网站根目录选项切换。

大约在248行跟下面一行  DocumentRoot后面接上自定义目录,让这个目录可以有特权打开，然后重启Apache
```

### 默认文档（默认访问的文件）
```
当客户端访问的是一个目录而不是具体文件时，服务端默认返回这个目录下的某个文档（文件），
这个文档就称之为默认文档。

配置文件 httpd.conf 的 282 行的 DirectoryIndex ，默认文档可以配置多个
例如：DirectoryIndex index.html index.php
```
### 去掉目录浏览
```
在浏览器上不让用户看到文件的目录
在配置文件 httpd.conf 的262行 Options Indexes FollowSymLinks 把 Indexes去掉

开发的时候可以不去掉，因为可以方便我们去观察目录
```