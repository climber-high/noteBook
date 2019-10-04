### vue

## v-cloak

>解决插值表达式闪烁问题

```
解决刚打开页面时出现{{msg}}问题。

1.
	[v-cloak]{
		display: none;
	}
	
	<p v-cloak>{{msg}}</p>
	
2.<p v-text="{{msg}}"></p>
```

## v-text

```
<p v-text="{{msg}}"></p>
v-text里面的内容会替换
```

## v-html

```
msg可以写html字符串代码
<p v-html="{{msg}}"></p>
```

## v-bind

>绑定属性

```
<input type="text" :title="msgtitle" />

data: {
	"msg":"123",
	"msgtitle":"msgt"
}

<input type="text" :title="msgtitle + '123'" />
<input type="text" :title="msgtitle + msg" />

也可以在里面拼接
```

## v-on

>事件绑定

```
绑定事件时可以加show()进行传参
<p @click="show">alert</p>

data:{
	msg:"mseeage"
},
methods:{
	show(){
		//获取data的属性值要用this.属性名
		alert(this.msg);
	}
}
```

## 事件修饰符

>.stop

```
阻止事件冒泡
<div class="inner" @click="divCli">
	<input type="button" value="点击"  @click.stop="btnCli" />
</div>
```

>.capture

```
@click.capture
以事件捕获顺序执行点击事件，会对子元素的.stop阻止默认事件失效
```

>.prevent

```
阻止默认事件
@click.prevent
```

>.self

```
无视冒泡，只有点击当前元素才出发事件

<div class="inner" @click.self="divCli">
	<input type="button" value="点击"  @click="btnCli" />
</div>
```

>.once

```
事件只执行一次，可上面指令可以嵌套使用
```

## @keyup

```
监听键盘抬起时执行
可以@keyup.enter="方法"，监听按回车的时候再执行后面方法
（space,enter,up,down,left.right,esc,delete,tab）

也可以比如点F2键就填对应的码:@keyup.113="方法名"

```

## v-model

```
表单中数据的双向绑定


可以绑定选中的值
<select v-model="sele">
	<option value="+">+</option>
	<option value="-">-</option>
</select>

data: {
	"sele": "-"
}
```


## v-bind:class

```
<h1 :class="['color','font',flag?'active':'']">666</h1>
可以直接用字符串形式填写样式表的类名，也可以绑定一个属性进行三元运算

<h2 :class="{active:true,aaa:false}">555</h2>
类名的显示或不显示,对象里的键不会被当成是一个变量

<h2 :class="obj">555</h2>
可以在data里面写属性跟值当成变量来进行绑定
data: {
	"obj":"{active:true,aaa:false}",
}
```

## v-bind:style

```
<h1 :style="{color:'red',fontSize:'16px'}">666</h1>
值必须要用字符串形式，键名有斜杠必须fontSize或'fontSize',
也可以把整个对象以属性值形式把属性传到上面

<h1 :style="[style1,style2]">666</h1>
可以用数组的形式接收多个属性名

data: {
	style1：{color:'red'},
	style2:{fontSize:'16px'}
}
```

## v-for

```
for in跟for of都可以

遍历数组：(值，下标)
<p v-for="(item,index) of list">{{item}}{{index}}</p>

遍历数组对象:(对象，下标)
<p v-for="(item,index) of list">{{index}}:{{item.id}}:{{item.name}}</p>

遍历对象:(值，键，下标)
<p v-for="(value,key,index) of list">{{index}}:{{key}}:{{value}}</p>

遍历数字:(遍历出来的数字从1开始)
<p v-for="count of 10">{{count}}</p>

遍历都要加上:key="唯一标识的数字或字符串"
例：遍历出来5个多选框按钮，并选中4，再往第一个多选框前动态添加一个多选框时，
添加后发现选中的并不是添加前第四个多选框，而是变成选中添加前的第三个多选框，
因为前面多了个元素，但选中的下标又不改变，导致整体内容向下移，但勾选还停留在
下标为4的那个位置。用:key可以解决
```

## v-if和v-show

```
v-if:每次都会重新删除或创建元素
v-show:每次不会重新的删除或创建元素，只是在display:none/block;之间切换 
```

## 全局过滤器

```
过滤器调用时的格式（可以多个过滤器用"|"隔开）
{{name | 过滤器1名称("123","456") | 过滤器2名称}}

定义过滤器:
过滤器function的第一个参数是管道符前面传递过来的数据
Vue.filter("过滤器1名称",function(name,arg,arg2){  //参数列表的name就是传过来的name,第二个参数后可以多个参数
	return name + arg;
})
```

## 私有过滤器

```
<p>{{msg | msgFormat(arg)}}</p>

在vue实例里面添加filters
filters:{
	msgFormat(data,arg){
		return msg+123;
	}
}

如果全局过滤器和私有过滤器同名则会只调用私有的。
```

## 自定义全局修饰符

```
Vue.config.keycodes.f2=113;然后全局就可以使用f2这个修饰符了
```

## 自定义全局指令

