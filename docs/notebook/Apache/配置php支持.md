## 配置php支持

下载php-7.1.10-Win32-VC14-x64
>地址：http://www.veryhuo.com/down/html/177606.html#download_area

#### 在httpd.conf文件添加php处理模块
```
LoadModule php7_module E:/work/exercise/forepart/php/php/php7apache2_4.dll
```
再重新执行Apache

####还要在 `<IfModule mime_module>` 节点中添加 .php 扩展名解析支持
```
第一种方法： AddType application/x‐httpd‐php .php
第二种方法：AddHandler application-x/httpd-php .php
第三种方法：<FilesMatch \.php$>
				Sethandler application/x-httpd-php
			</FilesMatch>

```
#### PHP 处理模块
```
这个模块不是根据后缀判断是否该 PHP 工作，是根据MIME Type 是不是application/x‐httpd‐php
决定是否应该让 PHP 上场
```

#### 默认文档配置节点 `<IfModule dir_module> `中添加 index.php
```
<IfModule dir_module>
	DirectoryIndex index.html index.php
</IfModule>
```
