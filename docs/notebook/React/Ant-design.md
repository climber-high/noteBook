### Ant Design

>1.cnpm i antd -S

```
import Button from 'antd/lib/button'
import 'antd/dist/antd.css'
```

>高级配置

```
安装:react-app-rewired和customize-cra

1. react-app-rewired是对create-react-app进行自定义配置

2. 使用react-app-rewired还要安装customize-cra

3. babel-plugin-import 是一个用于按需加载组件代码和样式的 babel 插件
```

>2.cnpm i react-app-rewired -D

>3.cnpm i customize-cra -D

>4.cnpm i babel-plugin-import -D


```
1.修改package.json

"scripts": {
    -   "start": "react-scripts start",
    -   "build": "react-scripts build",
    -   "test": "react-scripts test",
    +   "start": "react-app-rewired start",
    +   "build": "react-app-rewired build",
    +   "test": "react-app-rewired test",
    "eject": "react-scripts eject"
}

可能需要安装cnpm i react-scripts -S


2.然后在项目根目录创建一个 config-overrides.js 用于修改默认配置

const { override, fixBabelImports } = require('customize-cra')
module.exports = override(
  fixBabelImports('antd', {
    libraryDirectory: 'es',
    style: 'css',
  }),
);


3.引入

import { Button } from 'antd'
之后css也不用引入了
```

## UI组件显示为国际标准

>可以改成中文，一些组件的文字改成中文

```
index.js
用渲染的文件中引入
import zhCN from 'antd/lib/locale-provider/zh_CN';
import { LocaleProvider } from 'antd'

将要渲染的组件包裹起来
<LocaleProvider locale={zhCN}>
    <App />
</LocaleProvider>,
```

## 自定义主题

>按照 配置主题 的要求，自定义主题需要用到 less 变量覆盖功能

>cnpm i less less-loader -D

```
修改 config-overrides.js

const { override, fixBabelImports, addLessLoader } = require('customize-cra');
const theme = require("./theme")

module.exports = override(
  fixBabelImports('antd', {
    libraryDirectory: 'es',
-   style: 'css',
+   style: true,
  }),
+ addLessLoader({
+   lessOptions: { // 如果使用less-loader@5，请移除 lessOptions 这一级直接配置选项。
+     javascriptEnabled: true,
+     modifyVars: theme, //新建一个配置文件放置配置项
+   },
+ }),
);

可以创建theme.js文件
module.exports = {
    '@primary-color': 'red'
}
```

**有可能报错Less Loader has been initialized using**

**更改customize-cra版本**

**cnpm install customize-cra@1.0.0-alpha.0 -S**