## form表单
```
form表单    
让用户填写完毕之后提交到后台记录下来
```

###### 例如：
```
填写姓名，年龄（文本框），性别（单选），
报名选项（多选），户籍（选择框）备注（文本域） 
```
### input输入框
```
属性:
name  给后台人员使用的属性，识别这个数据的作用
value  值   发送到后台的值
placeholder   提示语
```

### type输入框
| 属性 | 对应功能 |
|:-----:|:-----:|
| text | 文本框 |
| password | 密码 |
| radio | 单选 |
| checkbox | 多选 |
| select | 下拉框 |
| textarea | 文本域 |



### 关于url     
```
hello.php?username=xxx&sex=man&pos=top
?紧跟参数
&连接多个参数
```
```
action      提交到后台的地址，php,jsp,asp,nodejs,java
method      提交方式
```

#### 注意
```
get
提交的信息会拼接在url的后面以参数的形式显示
安全性较低，例如新闻，天气，路况
信息长度较短
post
信息长度不受限制
完全性较为高
信息放在form data内
```

###### 例：
```
<form action="hello.php" method="post" target="_blank">
姓名：
<input type="text" name="username" id="" value="" placeholder="请输入用户名" /><br /> 
密码：
<input type="password" name="" id="" value="" /><br />
性别：
男：
<input type="radio" name="sex" id="" value="man" />  
女：
<input type="radio" name="sex" id="" value="lady" /><br />

 擅长位置：
<label for="pos_mid">中单：</label>                                
<input type="checkbox" name="pos" id="pos_mid" value="mid" />

<label for="pos_top">上单：</label>
<input type="checkbox" name="pos" id="pos_top" value="top" />

<label for="pos_jug">打野：</label>
<input type="checkbox" name="pos" id="pos_jug" value="jug" /><br />

<select name="">
<option value="天河区">天河区</option>
<option value="白云区">白云区</option>
<option value="越秀区<">越秀区</option>
</select><br />
```


#### 注意
```
label标签for属性和input标签的id相关联
```
```
固定文本域大小：resize: none
```
