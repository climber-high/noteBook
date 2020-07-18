### uni-app

### 使用Hbuilder X创建项目

```
第一次使用Hbuilder X要在 工具-插件安装 scss等插件，才能在uni-app使用
```

## uni-app相关

### 取消默认的自带标题

```
"style": {
		"navigationBarTitleText": "",
		"app-plus":{
			"titleNView":false //取消自带标题
		}
	}
```

## 目录结构

```
unpackage 启动自动生成目录
manifest.json 项目常用配置
main.js 项目入口文件
App.js 应用配置，配置全局样式跟监听应用的生命周期
static 存放应用静态文件
pages 添加路由配置页面
uni.scss 全局通用样式库 
```

## 生命周期


应用生命周期:

```
App.vue

export default {
	onLaunch: function(option) {
		console.log('App Launch');
		console.log(option);  //可以获取用户进入微信小程序的场景值
	},
	onShow: function() {
		console.log('App Show');
	},
	onHide: function() {
		console.log('App Hide');
	}
};
```

页面生命周期:

```
onLoad		页面初始化
onShow		页面进入
onReady		页面加载完毕
onHide		页面离开
onUnload	监听页面卸载
onResize	监听窗口尺寸变化
onPullDownRefresh  下拉
onReachBottom	上拉
```

## 组件生命周期

```
vue的8个生命周期
```


## 事件订阅

```
父子组件互相传参可以用vue的语法

也可以使用uni里面的，不用像vue绑定属性和方法
uni.$on 子组件可以无限调用父组件onLoad()生命周期里自定义的事件
uni.$once 调用一次
uni.$emit 触发事件

父组件:
onLoad() {
	uni.$on('eve',(data) => {
		console.log(data)
	})
}

子组件:
methods:{
	test(){
		uni.$emit("eve",{'name':'zs'});
	}
}
```

## 路由与页面跳转

```
uni.navigateTo
	redirectTo
	reLaunch  //关闭所有页面，打开某个页面
	switchTab
	navigateBack
	
uni.navigateTo({
	url:'xxx?name=zs'
})

在xxx页面的生命周期onLoad()获取参数
onLoad(option){
	console.log(option)  //{'name':'zs'}
}
```

## 获取dom元素

```
uni.createSelectorQuert().in(this)
```

## 授权 wx.authorize

>提前向用户发起授权请求。调用后会立刻弹窗询问用户是否同意授权小程序使用某项功能或获取用户的某些数据

```
1.可以通过 wx.getSetting 获取当前用户的设置，返回值中只会出现小程序已经向用户请求过的权限

wx.getSetting({
  success(res) {
    if (!res.authSetting['scope.userInfo']) {
      wx.authorize({
        scope: 'scope.userInfo',
        success () {
          wx.getUserInfo()
        }
      })
    }
  }
})
```