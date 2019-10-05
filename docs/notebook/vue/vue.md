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

@keyup.f2="方法名"
```

## 自定义全局指令

```
定义的时候，指令的名称前面不需要加v-前缀，在调用的时候要加v-

<input type="text" v-focus v-color="'blue'">

Vue.directive("focus",{

	//当指令绑定到元素上的时候，会立即执行这个bind函数，只执行一次
	bind:function(el){
		//el为绑定指令的这个元素，el参数是原生js对象，样式相关可以在bind中
	}, 

	 //当指令插入到dom的时候，会立即执行这个inserted函数，只执行一次
	inserted:function(el){
		el.focus();
		//与js有关的写在inserted中
	},
	updated:function(){} //当VNode更新的时候，会执行updated函数，可能执行多次
});

Vue.directive("color",{
	bind:function(el,binding){
		//第二个参数是一个对象，包含多个属性
		//1.binging.name (指令的名字color)
		//2.binding.value (指令运算后的值，如果是'1+1',就会为2)
		//3.binding.expression(不会运算，直接返回'1+1')
		el.style.color=binging.value;
	}
})
```

## 自定义私有指令

```
<input type="text" v-color="'blue'">

1.
directives:{
	color:{
		bind:function(el,binding){
			el.style.color=binding.value;
		}
		...
	}
}

2.
directives:{
	color:function(el,binding){  //相当于把代码写到bind和update里面
		el.style.color=binding.value;
	}
}
```

## 实例的生命周期

```
创建:
	beforeCreate:实例化了vue，但还没初始化data和methods
	Created:data和methods初始化完成

	beforeMount:创建了模板，但没有挂载在页面当中
	mounted:模板挂载在了页面中

运行:
	beforeUpdate:数据已经更新，但是页面上还是旧的数据
	updated:新的数据在页面上也更新显示了

销毁:
	beforeDestory:在销毁前夕，但实例仍然完全可用
	destoryed:vue实例销毁后调用，所有东西解绑定，所有事件监听移除，子实例也被销毁
```

## vue-resource

```
//全局配置根路径
Vue.http.options.root="主路径/";
下面填的开头不能加"/"

//全局配置emulateJSON
Vue.http.options.emulateJSON=true;

引入js文件

this.$http.get(路径名).then(function(result){
	成功的回调函数
})

//通过post方法的第三个参数，设置提交的内容类型为普通表单格式
this.$http.post(路径名,{参数},{emulateJSON:true}).then(result=>{
	成功的回调函数
})

this.$http.jsonp(路径名).then(function(result){
	成功的回调函数
})
```

## transtion动画元素(单个标签)

>使用transtion元素，把需要被动画控制的元素，包裹起来

```
/*进入前跟离开后*/
.v-enter,
.v-leave-to{
	opacity:0;
	transform: translateX(150px);
}

/*入场跟离场的时间段*/
.v-enter-active,
.v-leave-active{
	transition: all 0.8s linear;
}

<input type="button"  @click=" flag=!flag" value="toggle">
<transition>
	<h3 v-if="flag">666</h3>
</transition>
```

## 多个元素标签动画

```
<transition-group appear tag="ul">
	appear是刚进入是的动画效果，tag会让transition-group元素用ul标签显示，默认渲染为span 
</transition-group>
```


## 动画 修改v-前缀

```
transition添加name属性，在样式上就可以用自己定义的不用再用v-，改成m-
<transition name="m">
	<h6 v-if="flag2">666</h6>
</transition>
```

## 引用第三方animated样式

```
<input type="button"  @click="flag=!flag" value="toggle">

//进入时的动画，离开时的动画，动画的持续时间
//如果元素显示出来了，那么下一步会执行离开操作
//如果元素还没有显示，那么下一步会执行进入操作
<transition enter-active-class="bounceIn" leave-active-class="bounceOut" :duration="{enter:200,leave:400}">
	<h3 v-if="flag" class="animated">666</h3>
</transition>
```

## 半场动画

>使用js钩子函数

```
<transition @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
	<div class="ball" v-if="flag"></div>
</transition>

methods: {
	beforeEnter(el){
		//动画入场之前，动画尚未开始，写开始之前样式
		el.style.transform="translate(0,0)";
	},
	enter(el,done){
		el.offsetHeight;  //offset触发了动画的重排重绘
		//动画开始之后的样式
		el.style.transition = "all 800ms linear";
		el.style.transform="translate(150px,450px)";

		done() //这个时afterEnter函数的引用
	},
	afterEnter(el){
		this.flag=false;
	}
}
```