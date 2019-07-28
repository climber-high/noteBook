### maven配置

## 安装配置maven

1.从Apache网站 http://maven.apache.org/ 下载并且解压缩安装Apache Maven

http://maven.apache.org/download.cgi

2.配置 Maven 的conf文件夹中配置文件settings.xml。

3.修改settings.xml，添加镜像服务器设置：

```
<mirror>
  <id>aliyu</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyu Maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
</mirror>
```

## Eclipse中Maven的配置

>最新版的Eclipse已经内嵌了Mevne插件m2e, 不需要安装Maven插件, 如果不做任何配置, 会自动连接使用maven中央库自然可以使用, 但是中央库在国外, 受到中国防火墙等因素影响其访问速度很慢, 只有连接到国内镜像库才能提高Maven运行速度. 连接到国内镜像库按照如下配置.

1.打开Eclipse的首选项设置(Window -> Preferences)

2.找到Maven的配置项目 (Maven -> User Settings -> Browse)

3.设置Maven的全局配置文件settings.xml

4.更新配置信息 Update Settings

5.查看是否配置成功 Window -> Show View -> Other -> Global Repositories

## 创建Maven 桌面项目

1.选择菜单创建Maven项目 new -> Maven Project

2.选择项目目录结构的骨架 Create a simple project

3.输入项目相关信息  

Group id:企业包名称

Artifact id:项目名称

Packaging:打包形式，如果是桌面项目请选择jar





