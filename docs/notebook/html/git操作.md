## git操作
1.把要提交的文件添加到上传队列中
```
git add ./
```
2.把队列中的文件保存到本地库中
```
git commit -m"提交的理由"
```
3.拉取远程库的文件下来，跟本地库的文件进行自动合并
```
git pull
```
4.把本地库的文件，推送到远程库
```
git push
```
## gitbook操作
1.先在电子书根目录cmd运行:gitbook serve
2.删掉book文件夹，复制_book文件夹改成book,在目录里面右键打开git bash
再运行上面四步走。

## 上传到码云或github
码云上新建一个项目，再你用Hbuilder新建的文件夹中运行cmd输入
git clone (复制黏贴在码云上项目的克隆地址)运行，然后把Hbuilder新建文件夹的
内容全部剪切到克隆出来的文件夹里面。
要上传的时候在从克隆出来的根目录里cmd，再运行上面四步走。

>在github上clone代码到本地，增加速度的方式

```
加上.cnpmjs.org 
github.com.cnpmjs.org 
```


## 时光穿梭
1. 先查看要回退的时间点
```
git log
```

2. 根据ID回退到历史的某一个时间
```
git reset --hard d48bf2fcd99
```
`hard`的参数值就是第一步所获取到的id(不需要全部，可以截取其中一截就可以了)、

## git status

>查看哪些文件被修改

## git diff

>查看所有文件具体被修改的地方

```
git diff 文件所在路径
查看指定文件被修改的地方
```

## git上传可以把一些文件不上传
1.touch .gitignore
2.再把不想上传的文件打上去

## Github上搜索想要的开源项目

```
in:name spring boot stars:>3000

name:项目名
readme:项目readme文件里的内容
description:项目描述
language:是什么语言
stars:星的数量
pushed:填写更新时间，>2019-11-10

搜索:awesome 资料，可以搜到学习资料
```

## git与svn区别

```
1.git是分布式管理系统，svn不是

2.git把内容按元数据方式存储，svn是按文件

3.分支不同

4.git没有一个全局的版本号，而svn有
```