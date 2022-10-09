### typeScript

> ts文件执行编译成js文件的命令

```
tsc 文件路径
```

#### 自动编译typeScript

- 1. tsc --init 生成tsconfig.json

- 2. tsconfig.json文件设置outDir配置，生成的js文件存放在哪里

- 3. tsc --watch 实时监听ts文件，生成对应js

## 类型注解

> TypeScript 里的类型注解是一种轻量级的为函数或变量添加约束的方式, 提供了静态的代码分析，它可以分析代码结构和提供的类型注解。

**如果需要的参数和传入参数的类型不一致，就会提示错误**

> error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.

`注意：`要注意的是尽管`有错误`，`生成的js文件还是被创建了`。 就算你的代码里有错误，你仍然可以使用 TypeScript。但在这种情况下，TypeScript会`警告`你代码可能不会按预期执行。

```javascript
function showMsg(str: string) {
    return '123,'+ str;
}
let msg = '456';
console.log(showMsg(msg));
```

## 接口

> 在实现接口时候只要保证包含了接口要求的结构就可以

```javascript
interface Person {
    firstName: string;
    lastName: string;
}

const person = {
    firstName: '张',
    lastName: '三'
}
function showFullName(person: Person) {
    return person.firstName + "_" + person.lastName;
}
console.log(showFullName(person))
```

## 类

```javascript
interface IPerson {
    firstName: string;
    lastName: string;
    fullName: string;
}

class Person {
    firstName: string;
    lastName: string;
    fullName: string;
    constructor(firstName: string, lastName: string) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.fullName = this.firstName + "_" + this.lastName;
    }
}
const person = new Person('张', '三');
function showFullName(person: IPerson) {
    return person.fullName;
}
console.log(showFullName(person))
```

## 使用webpack打包TS

> 下载依赖

```
npm install -D typescript  (或者 yarn add -D ...)
npm install -D webpack@4.41.5 webpack-cli@3.3.10
npm install -D webpack-dev-server@3.10.2
npm install -D html-webpack-plugin clean-webpack-plugin
npm install -D ts-loader
npm install -D cross-env
```

> 1. npm init -y 生成package.json文件

```
npm install -D webpack@4.41.5 webpack-cli@3.3.10 webpack-dev-server@3.10.2 typescript@4.1.5 ts-loader@8.0.17 
html-webpack-plugin@4.5.0 cross-env@7.0.3 clean-webpack-plugin@3.0.0

"devDependencies": {
  "@webpack-cli/serve": "^1.7.0",
  "clean-webpack-plugin": "^3.0.0",
  "cross-env": "^7.0.3",
  "html-webpack-plugin": "^4.5.0",
  "ts-loader": "^8.0.17",
  "typescript": "^4.1.5",
  "webpack": "^4.41.5",
  "webpack-dev-server": "^3.10.2"
},
"dependencies": {
  "webpack-cli": "^4.10.0"
}
```

> 2. tsc --init 生成tsconfig.json配置文件

> 3. 入口src/main.ts

> 4. index页面，public/index.html

> 5. webpack配置文件webpack.config.js

```
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

const isProd = process.env.NODE_ENV === 'production' // 是否生产环境

function resolve(dir) {
  return path.resolve(__dirname, '..', dir)
}

module.exports = {
  mode: isProd ? 'production' : 'development',
  entry: {
    app: './src/main.ts'
  },

  output: {
    path: resolve('dist'),
    filename: '[name].[contenthash:8].js'
  },

  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        include: [resolve(__dirname, 'tsconfig.json')]  // tsconfig.json配置文件在哪个文件夹下
      }
    ]
  },

  plugins: [
    new CleanWebpackPlugin({}),

    new HtmlWebpackPlugin({
      template: './public/index.html'
    })
  ],

  resolve: {
    extensions: ['.ts', '.tsx', '.js']
  },

  devtool: isProd ? 'cheap-module-source-map' : 'eval-cheap-source-map',

  devServer: {
    host: 'localhost', // 主机名
    stats: 'errors-only', // 打包日志输出输出错误信息
    port: 8081,
    open: true
  }
}
```

> 6. 配置打包命令

```
在package.json

"scripts": {
    "dev": "cross-env NODE_ENV=development webpack serve --config ./webpack.config.js",
    "build": "cross-env NODE_ENV=production webpack --config ./webpack.config.js"
},
```

> 7. 运行与打包

```
yarn dev / npm run dev
yarn build / npm run build
```