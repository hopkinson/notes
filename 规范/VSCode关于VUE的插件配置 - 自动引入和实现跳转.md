> 在使用 vscode 开发 vue 项目的时候，引入文件路径我们经常会用到`@`，但是 vscode 不能识别这个符号的意义，所以一般我们需要手写路径。而且也无法实现引入 Vue 组件和 JS 模块后，按住 Ctrl/Command 点击路径可直接跳到该文件。所以今天跟大家介绍一个 vscode 插件：Path Intellisense

### 1. 安装插件`Path Intellisense`

![1111](https://img-blog.csdnimg.cn/20190327204236104.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NvbG9jYW8=,size_16,color_FFFFFF,t_70)

### 2. 工作区.vscode/setting.json 加入配置：

Vue:

```
"path-intellisense.mappings": {
    "@": "${workspaceRoot}/src"
}
```

Nuxt:

```
 "path-intellisense.mappings": {
    "@": "${workspaceRoot}",
    "~": "${workspaceRoot}"
  }
```

### 3.在项目 package.json 所在同级目录下创建文件 jsconfig.json：

Vue:

```
{
    "compilerOptions": {
        "target": "ES6",
        "module": "commonjs",
        "allowSyntheticDefaultImports": true,
        "baseUrl": "./",
        "paths": {
          "@/*": ["src/*"]
        }
    },
    "exclude": [
        "node_modules"
    ]
}
```

Nuxt:

```
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "allowSyntheticDefaultImports": true,
    "baseUrl": "./",
    "paths": {
      "@/*": [
        "*"
      ]
    }
  },
  "exclude": [
    "node_modules"
  ]
}
```

最终效果
![avatar](https://segmentfault.com/img/remote/1460000015690047?w=1000&h=414)
![va](https://segmentfault.com/img/remote/1460000015690048?w=1000&h=414)
