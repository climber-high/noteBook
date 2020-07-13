### wxs

>是小程序的一套脚本语言

### 小程序原理架构

```
微信小程序的框架包含两部分:
View视图层(多个)----用来渲染页面结构(使用WebView渲染)
AppService逻辑层(一个)----用来逻辑处理，数据请求，接口调用(使用JSCore运行)

视图层和逻辑层通过 系统层WeinxinjsBridage 进行通信
```

### wxs语法(不支持es6语法)

```
<wxs module="wxs1">
  function getNum(num){
    console.log(1,num);
  }
  function tapView(event,ownerInstance){  //ownerInstance为触发事件的组件的实例
	  console.log(JSON.stringify(event))
    var viewObj = ownerInstance.selectComponent('.view1');  //获取要操作的组件的类名
    viewObj.setStyle({  //设置样式
      'color':'red'
    })
  }
  function formateDate(time){
    var dateObj = getDate(time);  //date对象，相当于js的new Date()
    var year = dateObj.getFullYear();
    var month =  dateObj.getMonth()+1;
    var date = dateObj.getDate();
    return year + '-' + month + '-' + date;
  }
  module.exports = {
    getNum:getNum,  //不能简写成getNum
    tapView:tapView,
    formateDate:formateDate
  }
</wxs>

<view>{{wxs1.getNum(23)}}</view>
<view class=".view1" bindtap="{{wxs1.tapView}}">点击</view>
<view>{{wxs1.formateDate(1594645518361)}}</view>
```

**wxs的callMethod('方法名','参数')可以调用js里的方法**

>引入外部的wxs文件

```
跟pages同级目录创建wxs文件夹

<wxs module="wxs1" src="../../wsx/xxx.wxs"></wxs>

<view>{{wxs1.getNum(23)}}</view>
<view class=".view1" bindtap="{{wxs1.tapView}}">点击</view>
<view>{{wxs1.formateDate(1594645518361)}}</view>
```

