### typeScript

> ts文件执行编译成js文件的命令

```
tsc 文件路径
```

#### 自动编译typeScript

- 1. tsc --init 生成tsconfig.json

- 2. tsconfig.json文件设置outDir配置，生成的js文件存放在哪里

- 3. tsc --watch 实时监听ts文件，生成对应js