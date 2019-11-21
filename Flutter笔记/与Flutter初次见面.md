# 与 Flutter 初次见面

> Flutter 是谷歌推出的一套跨平台的，而且开源的 UI 框架，支持 iOS/Android 系统开发，并且是未来操作系统 Fuchsia 的默认开发套件。从 Flutter 团队开发的那么多版本再结合当前的发展形势看，谷歌正在大力推广 Flutter。

## Flutter 的优势

综合来看，Flutter 优势有以下几个方面：

- **跨平台性，开发环境不高**

一套代码在 iOS/Android 两大平台使用，避免过高维护成本和开发资源。支持 Android Studio/XCode,同时也支持 VSCode。

Android Studio 和 VSCode 两个编辑器是 Flutter 官方推荐。对于从事前端开发的可以选择 VSCode。更为轻量。

![VSCode](https://pic4.zhimg.com/v2-5ce7d2416ec9479e0882ddb8dfbb0ed7_1200x500.jpg)

- **通过“自绘 UI+原生系统”实现高帧率的流畅 UI**

  不再用 Webview 比较老的开发模式，而是使用 skika 作为 2D 渲染引擎，使用 Dart 语言运行时（Flutter 官方的语言），使用 Text 作为文字排版的引擎。
  ![常用得客户端技术](https://user-gold-cdn.xitu.io/2019/1/28/168925cbf99667a1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

  1. RN/Weex 通过 Javascript 与原生交互，并通过 Javascript 发送布局消息传递给原生端，实现布局渲染。
  2. Hybrid 通过 webview 容器实现，并由 JSBridge 与原生系统通信。
  3. Flutter 默认使用 Dart AOT 模式编译，不支持动态化（动态下发送代码实现热更新）。它直接调用系统的 API 绘制 UI，性能更加接近原生。

- **支持开发过程中得热重载**

再修改代码得过程中，不需要重启项目，只需要保存文件，即可刷新 Flutter 项目。有点类似前端得 webpack 实现的热重载功能。

- **学习成本低**

学习 Dart 语法，就感觉是写 Javascript+Typescript 的结合，学起来比较省力。

## Flutter 架构

Flutter 的架构，分为两个部分：**Framework** 和 **Engine**。

![flutter架构图](https://user-gold-cdn.xitu.io/2018/6/21/1642078f48635665?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### Flutter Framework

Framework 是 Dart 实现的 SDK。

- Animation/Painting/Gestures 和 Foundation

  底层 UI 库，提供动画，手势，绘制能力。谷歌官方提供给开发者使用的。

- Rendering

  负责构建 UI 树，UI 树的 Element 发生变化时，都会计算变化的部分并更新 UI 树，然后绘制到屏幕。

- wigets

  基础组件库。Flutter 提供了 2 套风格的组件库——Material 和 Cupertino。

### Flutter Engine

Engine 由 C++实现的 SDK。Framework 的 UI 层都会用到 Engine 这一层。

skika 作为 2D 渲染引擎，使用 Dart 语言运行时（Flutter 官方的语言），使用 Text 作为文字排版的引擎。
