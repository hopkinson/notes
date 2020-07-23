# Flutter 规范

## git 规范

![2020-07-17-15-34-58](http://qn.cawsct.com/2020-07-17-15-34-58.png?imageView2/1/w/200)

- feat：新功能（feature）
- fix：修补 bug
- docs：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改 bug 的代码变动）
- perf: （改进性能的代码更改）

## 规范讨论

### 命名

| 种类  | 用途           | 例子                       |
| ----- | -------------- | -------------------------- |
| aa_bb | 文件、文件夹   | file_utils.dart            |
| aaBb  | 常量、变量命名 | String isFlutterConst = '' |
| AaBb  | 类             | Class FlutterClass         |

#### 文件命名规则

##### 组件类型的文件

规则： **业务/种类 + 组件类型.dart**
例子：回收选择图片的组件，他是属于 UI 库规范——grid 网格种类，命名可为：images_grid.dart

##### 非组件类的方法/库/provider 定义 model 的文件

规则：**方法/库 ——（方法类型/库）\_utils.dart**
例子：平台 platform 相关的方法： platform_utils.dart

provider： **(业务)\_model.dart**
例子：回收相关的 provider： recycle_model.dart

##### 全局组件命名

规则: **Wb+组件名**
例子：全局按钮 button: WbButton

### 库引入相关

#### 导包顺序

规则： 导包有顺序要求，且每“每部分”间空行分隔开

具体如：

1. dart sdk 内的库
2. flutter 内的库
3. 第三方库（来自 pub）
4. 自己封装的库

```dart
import 'dart:io';
import 'dart:html';

import 'package:flutter/material.dart';
import 'package:flutter/cupertino.dart';

import 'package:shared_preferences/shared_preferences.dart';

import 'package:flutter_module_auction/utils/index.dart';
import 'package:flutter_auction_module/provider/provider_widget.dart';
```

#### 导包入口

项目里，utils 有一个 index.dart. 这文件的作用是 utils 的所有包含。

index.dart 的构成

```dart
export 'screen_utils.dart';
export 'tool_utils.dart';
```

具体业务引用的时候直接 index 入口即可。
页面引用：

```dart
import 'package:flutter_module_auction/utils/index.dart';
```

## 文件规范

### Router 路由管理

#### 引入规则

1. 定义路由

- 遵循模块划分：即按照不同模块放在不同文件
- 主要分为两种路由：一个是 flutter 内部的路由，另一个是 flutter 与原生交互的路由
- 尽量不要把内部的路由划分到 boost 路由里，boost 路由只放 flutter 与原生交互的路由。

```dart

class Recycle {
  // flutter内部的路由
  static Map inner = {'/recycle-home': RecycleHome()};

  // 与app交互的路由
  static Map<String, PageBuilder> boost = {
    '/recycle-boost':
        (String pageName, Map<String, dynamic> params, String _) =>
            FirstFirstRouteWidget(),
  };
}
```

### page 页面管理

模块分为两个部分：

![2020-07-20-10-14-04](http://qn.cawsct.com/2020-07-20-10-14-04.png)

1. widgets 部分： 页面用到的组件
2. provider 部分： 状态管理部分——接口，定义的数据
3. 页面部分： 拼接 widgets 和引用 provider

#### 注册路由

分别按照定义的路由归纳
![2020-07-17-11-29-29](http://qn.cawsct.com/2020-07-17-11-29-29.png)

| 文件名 | 范围 |
| - | |
| flutter_boost_router.dart | flutter 与原生交互的路由 |
| flutter_inner_router.dart | flutter 内部的路由 |

## 用到的工具

### 图片静态资源相关

#### 1. png,jpg 图片资源

1. 最好使用封装的方法，避免过长

##### 引用

```dart
import 'package:flutter_module_auction/utils/index.dart';
ToolUtils.getAssets('/images/xxx.png');

```

#### 2. 单色矢量图: 使用 iconfont 管理

![2020-07-17-10-45-44](http://qn.cawsct.com/2020-07-17-10-45-44.png?imageView2/1/w/400)

##### 获取 iconfont 途径

1. 可以使用 [https://xwrite.gitee.io/blog/](https://xwrite.gitee.io/blog/)iconfont 转 Flutter IconData

![2020-07-17-10-50-29](http://qn.cawsct.com/2020-07-17-10-50-29.png)

所有图标资源已放到 `utils\resource\iconfont_utils.dart`

##### 图标引用

```dart
import 'package:flutter_module_auction/utils/index.dart';

Icon(IconFont.icon_takePhoto)

```

#### 多色矢量图使用 svg 管理

##### 获取 svg 途径

叫设计上传到 iconfont 相关项目中，逐个下载 svg；或者单独给

![2020-07-17-10-53-29](http://qn.cawsct.com/2020-07-17-10-53-29.png)

![2020-07-17-10-53-43](http://qn.cawsct.com/2020-07-17-10-53-43.png?imageView2/1/w/400)

##### svg 引用

主要用到的库是： [flutter_svg](https://pub.flutter-io.cn/packages/flutter_svg)

1. 将 svg 放在 assets 统一管理, 已在配置工程的 pubspec.yaml 文件配置
2. 引用

```dart
import 'package:flutter_module_auction/utils/index.dart';

SvgPicture.asset(
              ToolUtils.getAssets('/images/xxx.svg')
              color: Color(0xff388DF3)

```

### flutter_boost - 与原生页面的相互跳转和通信

#### 目的

在使用 flutter 混合开发中，免不了 flutter 和原生 native 页面的相互跳转和通信，flutterboost 就是闲鱼团队开发的一个可复用的插件，旨在把 Flutter 容器做成浏览器的感觉。填写一个页面地址，然后由容器去管理页面的绘制。在 Native 侧我们只需要关心如果初始化容器，然后设置容器对应的页面标志即可。

### flutter_screenutil - 实现屏幕适配

#### 引用目的

flutter 的单位有点像前端的 px,比如我们的设计稿一个 View 的大小是 300px，如果直接写 300px，可能在当前 iphone6 设备显示正常，但到了其他设备可能就会偏小或者偏大，这就需要我们对屏幕进行适配。
flutter 本身并没有适配规则，这就需要我们自己去对屏幕进行适配。

#### 具体使用

**宽度、高度、padding、margin、字体大小**一般都需要用。

已经将常用的封装到 utils

```dart
import 'package:flutter_module_auction/utils/index.dart';

ScreenUtils.setWidth()
ScreenUtils.setheight()
ScreenUtils.setSp()

```

### provider - 状态管理

目前，封装了一个 MVVM 版的 provider，方便使用。若不满足可以自己封装

```dart
import 'package:flutter_auction_module/provider/provider_widget.dart';
ProviderWidget(
  builder: (context,model,child){},
  model: XXXXModel(),
  onModelReady:() {}
)
```

## 开发

### 代码格式化 - dartfmt

开发过程中或者提交代码前，可以使用: `dartfmt -w --fix lib/` 对 lib 文件夹下的内容代码美化。

注意官方说法：

> 建议不要使用 idea/as/vscode 的自动储存格式化作为交付
> 因为 idea/vs 有自己的格式化工具,并不是使用的 dartfmt,这样会造成代码交付格式不统一,写代码过程中可以使用以方便提高阅读性,但每次代码 commit 前建议使用 dartfmt -w lib/ 来格式化代码

### 代码质量检查 - dartanalyzer

目前接入了**pedantic**库，

在终端运行:`dartanalyzer lib`即可代码质量检查，dartanalyzer 工具已经集成到了 dart sdk 中了。

### 注释文档生成 - dartdoc

通过官方 sdk 提供的 dartdoc 工具可以生成文档,
在终端运行: `dartdoc`。 通过 Dart SDK 中的命令行工具来将注释转换为文档。**所以最好写中文注释**

![2020-07-17-11-56-57](http://qn.cawsct.com/2020-07-17-11-56-57.png)
