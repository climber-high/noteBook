### Nuxt.js 服务器端渲染

## 目录结构

```
assets (存放全局的css和js，也可以存放图片,在nuxt.config.js引入)

components (存放组件)

layouts (里面有default.vue，项目的主入口文件)

middleware

pages (存放页面)

plugins  (可以存放插件资源，axios请求，在nuxt.config.js引入,head属性中的script属性)

static (存放静态资源或需要直接在页面上引入的js,在nuxt.config.js引入,head属性中的script属性)

store (存放vuex)

nuxt.config.js (配置文件)
```

## 需要注意的问题

```
1. 没有window对象 (有window对象的js，只能在nuxt.config.js中引入)

2. 路由是自动生成，只需在pages文件夹命名好
    如: index.html.vue
    如需传参(前面加多一个_) : _id -> index.html.vue

3. asyncData({params}){ 用于生成静态化数据页面
    return {
      msg: '123'  为vue的data中的属性赋值
    }
  }

4. 页面中存在大量的css，可以取消展示
    nuxt.config.js在build属性添加
    extractCSS: { allChunks: true }

5. 不存在的页面会自动跳转到pages的第一个文件夹
    创建vue文件,并且页面重定向
    asyncData({route, redirect}){
      if(route.path.indexOf('.html') === -1){
        return redirect(route.path + '.html')
      }
    }

```

## 配置环境变量

```
1. 在nuxt.config.js添加

// 环境变量
const envFunc = require('./env);
const env = envFunc(process.env.NODE_ENV);

export default {
    env,
    head: {
        title: '标题'
        ....
    }
}

2. 创建env文件夹，创建index.js文件

const envConfig = {
    development: require('./development), // 开发环境
    small: require('./small'), //测试环境
    big: require('./big')  //正式环境
}
module.exports = function (mode) {
    return envConfig[mode];
}

3. 在env文件夹下创建 small.js , big.js文件

module.exports = {
    VUE_APP_Link: 'https://',
    VUE_APP_BASEURL: 'https://',
}

4. 使用方法

全局可以使用: process.env.VUE_APP_Link

5. 配置packjson.js

"scripts": {
    "dev": "nuxt --mode development",
    "build": "cross-env NODE_ENV=small nuxt build --mode",
    "build:big": "cross-env NODE_ENV=big nuxt build --mode",
}
```








