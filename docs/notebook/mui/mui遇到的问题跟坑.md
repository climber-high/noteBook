## 项目中遇到的问题

#### webview顶部和底部导航栏太高
```
在引入的webviewGroup.js中_proto.createWebview函数下面可以调整导航栏的你喜欢的
高度。
```
#### 顶部导航栏下面内容滑动时底部webview有多余像素也跟着滑动
```
底部webview的**subpage_style**设置其高度
```
#### 引入webviewGroup.js安卓出现等待icon
```
在248行plus.nativeUI.showWaiting();注释掉
```

#### 利用预加载页面通过监听自定事件，在里面的tap事件，每当退进同个页面，tap事件就会执行多一次
```
要重写页面返回逻辑取消监听tap事件
mui.back = function() {
	plus.webview.currentWebview().hide("auto", 300);
	mui("#Sapplication").off('tap');
}
```

#### mui框架的上拉刷新id="pullrefresh"里面的div小心被a.innerHTML=b;覆盖掉；
```
上拉刷新id="pullrefresh"应该是在父元素，里面包一个子元素再进行输出
```

#### 判断表单提交某个输入框是否为空
```
更好的体验：不应该只判单输入框为空，不然所有为空都会，会一次性警告出来，要判断本框为空并且上一个输入框
不为空，这样只会提示第一个输入框没有填，有更好的体验
```

#### 批量关闭webview
```
关闭除了入口页以及当前页以外的其他WebView
function fCloseOtherWebView() {
	当前窗口
	var ws = plus.webview.currentWebview();
	启动页,保留,因为推送用到
	var h = plus.webview.getLaunchWebview();
	var login=plus.webview.getWebviewById("login");
	var wvs = plus.webview.all();
	for(var i = 0; i < wvs.length; i++) {
		if(wvs[i] != ws && wvs[i] != h && wvs[i] != login) {
			plus.webview.close(wvs[i],"none");
		}
	}
}
```

#### 左右div滑动顶部导航栏如果加个底部边框字会顶上去
```
改样式.mui-segmented-control .mui-control-item 的  overflow: hidden;去掉
```

#### 获取手机唯一标识码
```
var Identify = plus.device.uuid;
```

#### 顶部导航栏跟下面内容页联动，可以调最外面div宽度自动调整
```
.mui-segmented-control.mui-segmented-control-inverted
```

#### 输入框想出现自带的清除内容按钮 在外面的div加上mui-input-row,里面的input加上类名class="mui-input-clear"
```
<div class="search_input mui-input-row">
	<input id="search" class="mui-input-clear" type="text" placeholder="输入查找内容"/>
</div>
想让手机键盘监听点击键盘就能执行搜索的功能
document.getElementById("search").addEventListener("keypress", function(event) {
	if(event.keyCode == "13") {
		document.activeElement.blur(); //收起虚拟键盘
		searchVideo(Vsearchtext);
	}
});
```

#### 用mui.init写的上拉加载执行一遍最后没数据时，想要重复执行
```
就可以在有用的侦听事件初始化加多一句执行一下*mui('#pullrefresh').pullRefresh().refresh(true);*
```

#### 不用复写的上拉加载方法
```
(function($) {
	$.ready(function() {
		$("#pullrefresh").pullToRefresh({
			up: {
				callback: function() {
					**var self = this;**  //把上拉区域保存起来
					setTimeout(function() {
						self.endPullUpToRefresh();  //一定要加，不加不然上拉加载方法只执行一次
					}, 1000);
				}
			}
		});
	});
})(mui);
```

#### 自定义函数
```
var list = plus.webview.getWebviewById("webview的id");//本页面获取想要传到的值的webview
mui.fire(list, 'recolor',{id});
'recolor'两个webview通信的事件名字

//接收值的webview
window.addEventListener("recolor", function(event) {
	var id=event.detail.id;
	alert(id);
})
```

#### 等待动画ui
```
plus.nativeUI.showWaiting();  //显示等待
plus.nativeUI.closeWaiting(); //关闭等待
```

#### 图片懒加载
```
引入两个文件
<script src="mui.lazyload.js" type="text/javascript"></script>
<script src="mui.lazyload.img.js" type="text/javascript"></script>

在每个图片的src改成data-lazyload
然后再
(function($) {
	$(document).imageLazyload({
		placeholder: '../../image/bgnote.png'  //懒加载图片路径
	});
})(mui);

```

#### mui.confirm样式
```
var btnArray = ['否', '是'];
mui.confirm('是否清除全部历史', '视频', btnArray, function(e) {
	if(e.index == 1) {
		//执行是的函数
	}
},"div"//加这个div可以变成iphone的样式)
```

#### 通过a连接href跳转到第三方浏览器
```
plus.runtime.openURL(this.href);
```

#### 调整mui-toast的样式
```
<div class="mui-toast-container mui-active">
	<div class="mui-toast-message">手机号码有误，请重填</div>
</div>
```

#### 上拉加载跟下拉刷新
```
iphone可以单独下拉刷新；但要上拉的话，把下拉也加上不然报错。安卓分开写都没问题
```

#### 解决在ios上fixed元素focusin时位置出现错误的问题
```
if(mui.os.ios){
	// 解决在ios上fixed元素focusin时位置出现错误的问题 
	document.addEventListener('DOMContentLoaded',function(){
		var footerDom = document.querySelector('footer');
		
		document.addEventListener('focusin', function() {
			footerDom.style.position = 'absolute';
		});
		document.addEventListener('focusout', function() {
			footerDom.style.position = 'fixed';
		});
	});
}
```

#### 取消下拉刷新只要上拉加载
```
down配置不要用安卓原生对象
引用mui.js在3010行加上
$.fn.pullRefresh = $.fn.pullRefresh_native;
并且加上
mui('.mui-content').on('tap','.mui-table-view-chevron', function() {
	mui('#pullrefresh').pullRefresh().setStopped(true);
})
```

#### mui强制横竖屏
```
plus.navigator.setFullscreen(true);//是否全屏显示
plus.screen.lockOrientation('landscape'); //锁死屏幕方向为横屏
plus.screen.lockOrientation('portrait-primary'); //锁死屏幕方向为竖屏
```