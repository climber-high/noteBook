## export default和import

```
向外暴露成员(一个js只能暴露export default一次)
export default{
     baseUrl:"abc"
}

用export暴露的成员，只能用{}的形式来接收，并且名字要匹配上,可以使用多个export
export var title = "666";
export var title2 = "123";

用import任意变量名来接收export defaul,export可以按需{}来接收
import url,{title,title2} from '@/common/config'
可以用as 别名，用别名来接收
```

**在一个模块中可以同时使用export default和export**

## scoped

>在当前组件的style标签中加上scoped，能让样式只在当前组件生效

```
添加lang="scss"可以使用sass编译语言

<style scoped lang="scss"></style>
```

## element UI

### 按需导入组件库

>1.安装babel-plugin-component：

```
cnpm install babel-plugin-component -D
```

>2.在babel.config.js添加
```
  presets: ["@vue/app",
     ["@babel/preset-env", { "modules": false }]
  ],
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
```

>3.安装新版babel

```
     npm install @babel/preset-env -D
```

>4.使用

```
在main.js中导入并使用组件
import { Button, Select } from 'element-ui';

Vue.component(Button.name, Button); 或 Vue.use(Button)；
```

#### 在vue组件中使用

```
<el-button type="primary">主要按钮</el-button>
```