### HOC

>高阶组件 Higher-Order Component

>可以劫持组件的渲染

#### 写法一:

1. 定义一个函数并返回一个类组件，导出该方法

```
WithCopyright.js

import React, { Component } from 'react'

const withCopyright = (YourComponent) => {
    return class WithCopyright extends Component {
        render() {
            return (
                <div>
                    <YourComponent {...this.props}/>
                    这是高阶组件的内容
                </div>
            )
        }
    }
}

export default withCopyright
```

2. 定义一个常规组件，导出时调用withCopyright方法并把该组件传进去

```
Another.js

import React, { Component } from 'react'
import withCopyright from './WithCopyright'

// ---@withCopyright---/  安装了装饰器的写法
class Another extends Component {
    render() {
        return (
            <div>
                另外一个组件{this.props.name}
            </div>
        )
    }
}

export default withCopyright(Another)
//----export default Another----/
```

3. 在App.js组件使用Another组件

```
import React, { Component, Fragment } from 'react'
import Another from './Another'

class App extends Component {
    render() {
        return (
            <Fragment> 
                <Another name="666"/>
            </Fragment>
        )
    }
}

export default App
```

**注意**

```
App.js使用Another组件并传入参数，其实是先传到高阶组件WithCopyright里面，
WithCopyright才是子组件，还要在使用形参的组件时，顺便把this.props的参数，
再传给孙组件Another，才能使用由App.js组件传过来的参数
```

**流程:**

```
App组件使用创建好的普通组件，该普通组件导出是，调用高阶组件定义的方法，传入
该普通组件给高阶组件，高阶组件就再使用形参渲染传过来的普通组件，可以再把App组件
传给高阶组件的参数再传给普通组件使用并渲染该普通组件。
```

## CRA中装饰器模式的写法

>www.npmjs.com

#### 第一种方式

>npm install react-app-rewired --save-dev

1. 修改package.json文件

```
"scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
}
```

2. 根目录创建config-overrides.js文件

```
module.exports = (config) => {
    //这里可以对config进行配置
    return config
}

3.配置webpack
```

#### 第二种方式

>npm install react-app-rewired --save-dev

1. 修改package.json文件

```
"scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
}
```

>npm install customize-cra --save-dev

2. 根目录创建config-overrides.js文件

```
const { override, addDecoratorsLegacy } = require('customize-cra')

module.exports = override(
  addDecoratorsLegacy()
)
```

3. 安装装饰器

>cnpm i @babel/plugin-proposal-decorators -D
