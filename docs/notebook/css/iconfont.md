## iconfont
```
1.在iconfont.cn找到想要的图标，下载解压
2.在项目里面新建font文件夹，把解压好的Css,EOT,SVG,truetype,woff五个文件放到里面
3.把css文件拉到scss文件夹里面，把css文件改成scss文件
4.在scss文件里上面写@charst "utf-8";，在引入这个scss文件，并修改路径1，2，4，5前面加上../font/
5.在index.html里面写<i class="iconfont icon-fangdajing"></i>
```
