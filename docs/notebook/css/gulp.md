# Gulp的学习

## 什么是gulp
自动化处理工具的平台，自动，高效，带感。
具备的功能有：
- 图片的处理,压缩优化，雪碧图
- CSS的压缩，自动预编译，兼容处理
- JS的压缩，加密
- HTML压缩
- 同步浏览功能
- 文件的常规操作
- 。。。此处省略N个字
gulp类似一个插槽，上面的这些功能，全部是利用它的插件来实现。
所以gulp的核心功能就是方便开发者开发，和发布最终产品。

## 安装gulp
0.安装node+npm环境
```
npm -v         //检查npm版本号
```
1.安装全局gulp
```
	cnpm i gulp-cli -g
	gulp -v  //检查gulp版本号
```

2.去到你的开发目录下，安装本地gulp
	```
		cnpm i gulp -D
	```
	然后目录下会有`node_modules`和`package.json`
	如果没有自动生成一个package.json,可以执行下面的命令生成一个
	```
	npm init -y
	```


## gulp的使用
1.创建`gulpfile.js`
2.需要什么插件，那么就安装什么插件
```
cnpm i gulp-xxx -D
```

`-D` 的作用其实是帮你记下你的插件列表，方便下次再次安装
3.安装完插件后，你就可以引入插件，用类似下面的代码，来定义自己的任务
```
var gulp = require("gulp")
var gulp_minify_css = require("gulp-minify-css");

gulp.task("mincss",function(){
	gulp.src("./css/**/*.css")
	    .pipe(gulp_minify_css())
	    .pipe(gulp.dest("./dist/css"));
});
```

4.调用任务
gulp mincss

### 步骤总结
1.先在根目录cmd 用`cnpm i gulp -D`
2.把配置好的gulpfile,js和package.json复制到新项目的根目录
3.再用cnpm i下载插件
4.然后就可以调用任务了
