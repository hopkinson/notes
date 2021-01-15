# 前端新玩具：Vite

## Vite 的定义

vite 的思想是开发时使用浏览器自己的 import/export 不进行打包, 这样热更新的时候如果文件发生了变化,只需要更新相关文件内容, 无需进行大量无效计算, 发布的时候使用 rollup 打包成生产环境代码, 对浏览器的要求比较高, 但是提高了开发效率。

## Vite 的由来

前端打包工具是将树形代码打包为一个文件或者根据模块打包成多个文件, 如果树中的任意一个节点发生变化, 那么引用该节点的所有节点和模块都要重新打包, 对于大型项目来说, 改一行代码可能就需要重新打包很久。

## 快速上手

```shell
npm init vite-app <project-name>
cd <project-name>
npm install
npm run dev
```

![2020-12-25-17-20-01](http://qn.cawsct.com/2020-12-25-17-20-01.png)

### 对比差异点

打开生成的项目过后，你会发现就是一个很普通的 Vue.js 应用，没有太多特殊的地方。

不过相比于之前 vue-cli 创建的项目或者是基于 Webpack 搭建的 Vue.js 项目，这里的开发依赖非常简单，只有 vite 和 @vue/compiler-sfc。

vite 就是我们今天要介绍的主角，而 @vue/compiler-sfc 就是用来编译我们项目中 .vue 结尾的单文件组件（SFC），它取代的就是 Vue.js 2.x 时使用的 vue-template-compiler。

再者就是 Vue.js 的版本是 3.0。这里尤其需要注意：**Vite 目前只支持 Vue.js 3.0 版本。**

> 如果你想，在后面介绍完实现原理过后，你也可以改造 Vite 让它支持 Vue.js 2.0。

## 基础体验

- index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <script type="module" src="index.js"></script>
  </head>
</html>
```

- index.js
  注意这里引入的时候使用了文件名包含后缀, 浏览器会使用该路径去请求资源, 使用缩写的话会报 404.

```js
import { Stu } from "./Stu.js";

let s = new Stu("ace");
s.say();
```

- stu.js

```js
export class Stu {
  constructor(name) {
    this.name = name;
  }

  say() {
    console.log("hello", this.name);
  }
}
```

最终会输出
![2020-12-25-17-19-16](http://qn.cawsct.com/2020-12-25-17-19-16.png)

## 内部分析

看浏览器的 Source 里会发现，

- 注入环境变量并引入 main.js
  ![index.html](http://qn.cawsct.com/2020-12-25-17-21-25.png)

- 在 main.js 中将模块引用重写, 注意后面加了文件类型  
  ![main.js](http://qn.cawsct.com/2020-12-25-17-21-44.png)

- vue 文件分了几个部分, 包括模板,脚本,样式三个。

![App.vue](http://qn.cawsct.com/2020-12-25-17-23-16.png)

![App.vue?type=template](http://qn.cawsct.com/2020-12-25-17-23-41.png)

## 打包 or 不打包

Vite 的出现，引发了另外一个值得我们思考的问题：究竟还有没有必要打包应用？

之前我们使用 Webpack 打包应用代码，使之成为一个 bundle.js，主要有两个原因：

1. 浏览器环境并不支持模块化
2. 零散的模块文件会产生大量的 HTTP 请求

随着浏览器的对 ES 标准支持的逐渐完善，第一个问题已经慢慢不存在了。现阶段绝大多数浏览器都是支持 ES Modules 的。

![es-modules](http://qn.cawsct.com/2020-12-25-14-29-51.png)

零散模块文件确实会产生大量的 HTTP 请求，而大量的 HTTP 请求在浏览器端就会并发请求资源的问题；

![HTTP请求](http://qn.cawsct.com/2020-12-25-14-30-40.png)

_如上图所示，红色圈出来的请求就是并行请求，但是后面的请求就因为域名链接数已超过限制，而被挂起等待了一段时间。

### 开箱即用

- TypeScript - 内置支持
- less/sass/stylus/postcss - 内置支持（需要单独安装所对应的编译器）

### 特性小结

Vite 带来的优势主要体现在提升开发者在开发过程中的体验。

- Dev Server 无需等待，即时启动；
- 几乎实时的模块热更新；
- 所需文件按需编译，避免编译用不到的文件；
- 开箱即用，避免各种 Loader 和 Plugin 的配置；
