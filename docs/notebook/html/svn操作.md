## svn

>SVN是subversion的缩写，是一个开放源代码的版本控制系统

```
1.下载subversion

2.把svn安装目录里的bin目录添加到path路径中，在命令行窗口中输入 svnserve --help ,查看安装正常与否。

3.下载TortoiseSVN客户端
```

>使用

```
建立库：

1、新建文件夹，目录和文件夹名称最好都用英文，不要使用中文；

2、打开文件夹，在空白处按下“shift键+鼠标右键”；

3、在弹出的菜单中选择“TortoiseSVN - Create repository here”；

4、弹出对话框，提示创建成功，并自动在文件夹中创建了目录结构


导入项目：

1、打开已有的项目文件夹，在空白处按下“shift键+鼠标右键”；

2、在弹出的菜单中选择“TortoiseSVN - Import”；

3、选择导入路径，填写备注信息，点击“OK”开始导入；

4、导入完成后会弹出提示，可以拖动滚动条查看导入的文件，点击“OK”，完成导入；


建立工作目录（检出）：

1、新建工作目录文件夹，在空白处按下“shift键+鼠标右键”；

2、在弹出的菜单中选择“SVN Checkout...”；

3、在弹出的对话框中选择库目录、工作目录，点击“OK”开始检出；

4、弹出详细信息对话框，导出完成后，点击“OK”；


更新工作目录：

1、通常在你对工作目录进行修改前，为保证你的文件是最新的，需要进行更新操作；

2、在工作目录空白处点击鼠标右键，选择“SVN Update”；

3、会弹出对话框开始更新，并显示更新了哪些内容，库版本是多少；


提交工作目录：

1、在做了修改，需要保存到库中时，用到提交操作；

2、在工作目录空白处点击鼠标右键，选择“SVN Commit”；

3、会弹出对话框，可以输入备注信息，显示将要提交哪些文件，点击“OK”开始提交；

4、弹出对话框显示提交进度，完成后点击“OK”完成提交；
```


