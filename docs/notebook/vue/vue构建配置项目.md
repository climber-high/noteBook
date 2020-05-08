### vue构建配置项目

### vs code插件Vetur

## 全局安装 vue-cli
```
npm i vue-cli -g
```

## 创建一个基于 webpack 模板的新项目
```
vue init webpack vue-project
第一个yes,其他no
```
## 进入文件夹里面

## cnpm i

## 在vue-project运行或修改配置
```
npm run dev
```
## 生产
```
npm run build
```

**注意：**
要想npm run build后把代码git到服务器要在.gitignore去掉/dist/

要在config的index.js中修改为`assetsPublicPath: './'`，因为dist中的index.html和static是同个目录所以要`./`，默认是返回根路径找static文件夹
