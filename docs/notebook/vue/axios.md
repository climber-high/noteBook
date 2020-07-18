## npm i axios --save

## 移动端
cnpm i fastclick -S

### 安装

>npm install axios -S

```
创建文件夹services放接口文件跟axios方法文件

apis.js

export default{
    baseUrl : '接口域名',
    getTodo: '/index'
}
/---------------------------/
index.js

import axios from 'axios'
import apis from './apis'

const ajax = axios.create({
    baseURL : apis.baseUrl
})

//拦截器
ajax.interceptors.request.use(
    config => {
        const token = sessionStorage.getItem('token')
        if (token) { // 判断是否存在token，如果存在的话，则每个http header都加上token
            config.headers.authorization = token  //请求头加上token
        }
        return config
    },
    err => {
      return Promise.reject(err)
    }
)

export const getTodos = () => {
    return ajax.get(apis.getTodo,{参数:值})
}

//可以不用create方法，直接get/post
axios.get('但这里面要域名+接口路径(全路径)')

/----------------------------------/

//引用调用
getTodos()
    .then((res) => {
        //获取的数据
        if(resp.status === 200){

        }
    })
    .catch(err => {
        console.log(err)
    })
    .finally(() => {

    })

```

>请求拦截器

```
这个拦截器会在你发送请求之前运行
这个请求拦截器的功能是为我每一次请求去判断是否有token，
如果token存在则在请求头加上这个token。后台会判断我这个token是否过期。

const ajax = axios.create({
    baseURL : apis.baseUrl
})

//拦截器
ajax.interceptors.request.use(
    config => {
        const token = sessionStorage.getItem('token')
        if (token) { // 判断是否存在token，如果存在的话，则每个http header都加上token
            config.headers.authorization = token  //请求头加上token
        }
        return config
    },
    err => {
      return Promise.reject(err)
    }
)
```

>响应拦截器

```
const ajax = axios.create({
    baseURL : apis.baseUrl
})

ajax.interceptors.response.use(
  response => {
    //拦截响应，做统一处理 
    if (response.data.code) {
      switch (response.data.code) {
        case 1002:
          store.state.isLogin = false
      }
    }
    return response
  },
  //接口错误状态处理，也就是说无响应时的处理
  error => {
    return Promise.reject(error.response.status) // 返回接口返回的错误信息
  })
```