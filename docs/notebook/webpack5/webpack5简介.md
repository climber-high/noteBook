### webpack5简介

开发时，我们会使用框架（React、Vue），ES6 模块化语法，Less/Sass 等 css 预处理器等语法进行开发。

这样的代码要想在浏览器运行必须经过编译成浏览器能识别的 JS、Css 等语法，才能运行。

所以我们需要打包工具帮我们做完这些事。

除此之外，打包工具还能压缩代码、做兼容性处理、提升代码性能等。

> Webpack 是一个静态资源打包工具。

它会以一个或多个文件作为打包的入口，将我们整个项目所有文件编译组合成一个或多个文件输出出去。

输出的文件就是编译好的文件，就可以在浏览器段运行了。

我们将 `Webpack` 输出的文件叫做 `bundle`。

### 功能介绍

Webpack 本身功能是有限的:

- 开发模式：仅能编译 JS 中的 ES Module 语法
- 生产模式：能编译 JS 中的 ES Module 语法，还能压缩 JS 代码

### 使用

#### 1. 资源目录

```
webpack_code # 项目根目录（所有指令必须在这个目录运行）
    └── src # 项目源码目录
        ├── js # js文件目录
        │   ├── count.js
        │   └── sum.js
        └── main.js # 项目主文件
    └── public
        └── index.html
```

#### 2. 创建文件

```
1. count.js

export default function count(x, y) {
  return x - y;
}

2. main.js

import count from "./js/count";
console.log(count(2, 1));

3. public --> index.html
引入main.js文件
```

#### 3. 下载依赖

> 在项目根目录初始化package.json

**需要注意的是 package.json 中 name 字段不能叫做 webpack, 否则下一步会报错**

```
npm init -y
```

> 下载依赖

```
npm i webpack webpack-cli -D
```

#### 4. 启用webpack

> 开发模式

```
npx webpack ./src/main.js --mode=development
```

> 生产环境

```
npx webpack ./src/main.js --mode=production
```

`npx webpack`: 是用来运行本地安装 `Webpack` 包的。

`./src/main.js`: 指定 `Webpack` 从 `main.js` 文件开始打包，不仅会打包 main.js，还会将其依赖也一起打包进来。

`--mode=xxx`：指定模式（环境）。

#### 5. 输出文件

> 默认 `Webpack` 会将文件打包`输出到 dist目录`下