### vue-cli3配置

>最新版npm和node >=8.9

## 安装步骤

>1.全局安装 npm install -g @vue/cli 或 yarn global add @vue/cli

>2.查看版本/是否安装成功 vue -V

>3.在新文件夹下创建项目 vue create my-project

```
指向的vuecli3是因为上一次记录过的cli3配置，第一次执行create是没有的
按键盘上下键可以选择默认（default）还是手动（Manually），如果选择default，一路回车执行下去就行了
```

>4.选择配置，看个人项目需求

**注意，空格键是选中与取消，A键是全选**

```
TypeScript 支持使用 TypeScript 书写源码
Progressive Web App (PWA) Support PWA 支持。
Router 支持 vue-router 。
Vuex 支持 vuex 。
CSS Pre-processors 支持 CSS 预处理器。
Linter / Formatter 支持代码风格检查和格式化。
Unit Testing 支持单元测试。
E2E Testing 支持 E2E 测试。
```

>5.css的预处理

```
dart-sass:需要保存后才会生效
node-sass:是自动编译实时的
```

>6.选择的是ESLint + Prettier

>7.选择语法检查方式

```
Lint on save : 保存就检查
Lint and fix on commit : 提交时检查
```

>8.单元测试

>9.配置文件存放地方

```
In dedicated config files : 存放在独立文件夹
In package.json 在package.json文件里
```

>10.询问是否记录这一次的配置

>11.回车下载

>12.npm run serve 启动项目