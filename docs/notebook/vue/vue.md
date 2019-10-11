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

## vue实例的属性

```
new Vue({
	el:"#app",
	data:{},
	methods:{
		mt(){

		}
	},
	filters:{ //字符串过滤器
		msgFormat(data,arg){
			return msg+arg;
		}
	},
	directives:{ //自定义指令
		color:{
			bind:function(el,binding){
				el.style.color=binding.value;
			}
		}
	},
	components:{
		mycom:{
			template:'模板'
		}
	},
	beforeCreate:{},   //生命周期钩子(函数)
	created:{},
	beforeMount:{},
	mounted:{},
	beforeUpdate:{},
	updated:{},
	beforeDestory:{},
	destoryed:{}
})
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
	created:data和methods初始化完成

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

## 全局vue组件

>拆分vue实例的代码量，不同的组件来划分不同的模块

```
模块化:从代码逻辑的角度进行划分
组件化:从UI界面的角度进行划分
```

>1.使用Vue.extend来创建全局的Vue组件模板对象

```
创建组件模板对象:
var coml = Vue.extend({
	template:'<h3>extend创建的组件</h3>'
})

创建组件并使用:
<my-coml></my-coml>

Vue.component('myComl',coml);
或:
Vue.component('myComl',{
	template:'<h3>extend创建的组件</h3>'
});
```

**注意:template中只能有一个根元素**

>创建组件的另外一种方式

```
HTML:

<div id="app">
	<my-coml></my-coml>  //一样填组件的名称
</div>

<template id="tem1"> //要把模板放到vue实例容器外面，填上对应的id
	<div>
		<h3>创建的组件</h3>
		<span>6666</span>
	</div>
</template>

JS:

Vue.component('myComl',{
	template:"#tem1"	
});
```

## 定义私有组件

```
<my-com></my-com>

components:{
	myCom:{   //定义的组件名称
		template:'<h3>666</h3>'  //组件内容
	}
}
```

## 组件可以有自己的data跟methods

```
Vue.component('myComl',{
	template:"<h3>{{msg}}<h3/>",
	data:function(){     //组件中的data必须是一个方法
		return {		//方法内部还必须返回一个对象，数据可以给组件使用
			msg:'组件中的data定义的数据'
		}		
	},methods: {
		mt(){
			
		}
	}
});

data是一个方法的原因:让组件中每个数据都独立出来，当成局部变量。如果使用了多次
这个组件，可以让数据也独立出来，防止一修改数据而导致所有组件内容数据都发生变化
```

## 组件component元素

```
HTML:
<a href="" @click.prevent="comName='login'">登录</a>
<a href="" @click.prevent="comName='register'">注册</a>
<!-- :is属性可以用来指定来展示的组件名称 -->
<component :is="comName"></component>

JS:
Vue.component("login", {
	template: '<h3>登录组件</h3>'
})

Vue.component("register", {
	template: '<h3>注册组件</h3>'
})

let vue = new Vue({
	"el": "#app",
	data: {
		comName : 'login'
	}
})
```

## 父组件向子组件传值props

```
<com1 :parentmsg="msg"></com1>  //自定义名字绑定父组件数据

data: {
	msg:'父组件的数据'
},
components:{
	com1:{
		//把父组件传递过来的属性，先在props数组中定义了才能使用
		props:['parentmsg'], 
		template:'<h3>{{parentmsg}}</h3>'
	}
}
```

## 父组件向子组件传方法并调用 this.$emit()

>通过子组件绑定父组件的方法，子组件中通过this.$emit('自定义绑定名',data)，可以调用父组件的方法并且把data传到父组件方法的参数列表中，进行使用。（先父绑定方法，子调用方法并后面接着传递参数回去）

```
<div id="app">	
	<com @func="show"></com>   //1.绑定父组件的show方法
</div>

<template id="teml">
	<div>
		<h1>666</h1>
		<input type="button" value="子组件拿父组件的方法" @click="myclick">
	</div>
</template>


var com2={
	template:'#teml',
	methods: {
		myclick(){
			//2.拿到父组件传递过来的func方法并调用,并向父组件的方法show传一个参
			this.$emit('func',123);
		}
	},
}

var vue = new Vue({
	"el": "#app",
	data: {
		msg:'父组件的数据'
	},
	methods: {
		show(data){
			console.log("父组件的方法"+data)  //父组件的方法123
		}
	},
	components:{
		com:com2
	}
})
```

## ref获取元素跟dom组件

>获取元素

```
<div id="app">
	<h3 ref="myh3">哈哈哈</h3>
</div>

mounted() {
	console.log(this.$refs.myh3.innerText)
}
```

>**获取**组件

```
<div id="app">
	<login ref="mylogin"></login>
</div>

var login={
	template:'<h1>登录组件</h1>',
	data:function(){
		return{
			msg:'son msg'
		}
	},
	methods: {
		show(){
			console.log('调用了子组件的方法')
		}
	},
}

var vue = new Vue({
	"el": "#app",
	data: {
		
	},
	methods: {
		getElement(){
			console.log(this.$refs.mylogin.msg)   //可以获取子组件并获取属性跟调用方法
		}
	},
	components:{
		login:login
	}
})
```

## 路由

>对于单页应用，主要通过URL中的hash(#号)，来实现不同页面之间的切换，http请求中不会包含hash相关的内容

#### 创建路由(new VueRouter)并使用(router-view元素)

```
HTML:
<div id="app">
	<!-- <a href="#/login">登录</a>
	<a href="#/register">注册</a> -->

	<!-- 路由跳转可以用router-link标签，不指定标签的话默认是a标签
	跟上面效果等同，可以用tag属性改变展示的标签，也可以进行点击 -->
	<router-link to="login" tag="span">登录</router-link>
	<router-link to="register">注册</router-link>

	<!-- 这是vue-router提供的元素，当作占位符，将来路由规则匹配到的组件，
	就会展示到这个router-view中去 -->
	<router-view></router-view>
</div>


JS:
var login = {
	template:'<h1>登录组件</h1>'
}
var register = {
	template :'<h1>注册组件</h1>'
}

const routerObj = new VueRouter({
	routes:[ //路由匹配规则
		//每个路由规则，都是一个对象
		//属性1:是path,表示监听哪个路由链接地址
		//属性2:component，表示，如果路由是前面匹配到的path，则表示component属性对应的哪个组件
		//注意:component的属性值，必须是一个组件的模板对象

		{path:'/',redirect:'/login'},   //路由重定向
		{path:'/login',component:login},
		{path:'/register',component:register},
	]
})

var vue = new Vue({
	"el": "#app",
	data: {},
	//将路由规则对象，注册到vm实例上，用来监听URL地址的变化，然后展示对应的组件
	router:routerObj
})
```

#### 路由重定向redirect

```
const routerObj = new VueRouter({
	routes:[ //路由匹配规则
		//重定向到哪个路径
		{path:'/',redirect:'/login'},   //路由重定向
	]
})
```

#### 设置当前路由高亮样式

```
//路由切换的高亮样式
.router-link-active,myactive{
	color:red;
}

const routerObj = new VueRouter({
	routes:[],
	linkActiveClass : 'myactive'  //修改.router-link-active激活类为myactive
})
```

#### 路由query方式传参

```
<div id="app">
    <!--如果在路由中，使用查询字符串，给路由传递参数，则不需要修改路由规则的path属性-->
    <router-link to="login?id=10&name=123">登录</router-link>
    <router-view></router-view>
</div>


var login = {
	template:'<h1>登录组件{{$router.query.id}}--{{$router.query.name}}</h1>',
	created:{
	  console.log(this.$router.query.id)  //获取router的id参数值,$router.query可以获取多个参数
	}
}
const routerObj = new VueRouter({
	routes:[
	    {path:'/',redirect:'/login'}, 
	    {path:'/login',component:login}
	]
})
var vue = new Vue({
	"el": "#app",
	data: {},
	router:routerObj
})
```

#### 利用params方式传参

```
<div id="app">
    <router-link to="login/12/zs">登录</router-link>
    <router-view></router-view>
</div>


var login = {
	template:'<h1>登录组件{{$router.params.id}}</h1>',
	created:{
	  console.log(this.$router.params.id)  //获取router的id参数值,$router.query可以获取多个参数
	}
}
const routerObj = new VueRouter({
	routes:[
	    {path:'/',redirect:'/login'},
	    {path:'/login/:id/:name',component:login} //:id 和 :name 占位符所对应router-link to对应的参数
	]
})
var vue = new Vue({
	"el": "#app",
	data: {},
	router:routerObj
})
```

#### 使用children属性实现路由嵌套

```
<div id="app">
    <router-link to="/account">Account<router-link>
    <router-view></router-view>
</div>

<template>
    <div>
      <h1>这是Account组件</h1>
      <router-link to="/account/login">登录<router-link>
      <router-link to="/account/register">注册<router-link>
    </div>
<template>


var acount = {
	template:'#tmpl',
}
var login = {
	template:'<h1>登录模板</h1>',
}
var register = {
	template:'<h1>注册模板</h1>',
}


const routerObj = new VueRouter({
	routes:[
	    {
	      path:'/account',
	      component:acount,
	      children:[   //children实现子路由的嵌套，子路由的path前面不要带"/"
		{path:'login',component:login},
		{path:'register',component:register}
	      ]
	     }
	]
})
var vue = new Vue({
	"el": "#app",
	data: {
	}，
	router:routerObj
})
```

