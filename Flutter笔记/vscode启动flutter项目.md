# vscode 启动与调试 flutter 项目

> VSCode 也是 flutter 官方推荐的编辑器。使用 vscode 启动与调试 flutter 项目，还是很多问题要注意的。我刚刚开始的时候并不能做到一气呵成，现在就总结一下具体的步骤。

## 开发前需要的东西

### 安装环境

1. 安装 flutter: [安装地址](https://flutter.io/docs/get-started/install)
2. 安装 vscode: [安装地址](https://code.visualstudio.com/)
3. 在 vscode 里安装有关的插件——Dart,flutter 两个或者其他相关的插件。
   ![有关插件](https://user-gold-cdn.xitu.io/2018/12/6/16781f024d2d4b39?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 设置变量

1. 在环境变量里填入刚刚下载的 flutter 的存放路径
   ![20191126120852.png](https://i.loli.net/2019/11/26/9M6XQn4kd7Yj21w.png)

2. 设置 PUB_HOSTED_URL”和“FLUTTER_STORAGE_BASE_URL

如果用官方提供的两个地址，可能会一直停留在 Resolving dependencies…过不去。

Gradle 默认直连网络，即使 Mac 设置了全局代理也是一样。就算你给 Android Studio 设置了代理，它依旧会风轻云淡地直连那个你在中国一辈子也不可能连上的网站……

所以后面我才改为：

```shell
FLUTTER_STORAGE_BASE_URL: https://mirrors.sjtug.sjtu.edu.cn
PUB_HOSTED_URL: https://dart-pub.mirrors.sjtug.sjtu.edu.cn
```

### 安装 android studio/xcode 和模拟器

因为写这篇文章的时候使用的是 windows,所以只支持 Android。那么去下载 Android Studio。

如果你是 Mac，又想开发 iOS，下载 XCode。

关于下载模拟器，可以在 AS 中的 AVD Manager 进行下载。

![20191126135229.png](https://i.loli.net/2019/11/26/C9MjsfuaT1GJDvt.png)

### flutter doctor

打开终端，输入`flutter doctor`诊断以下运行环境有没有问题。

![20191126135342.png](https://i.loli.net/2019/11/26/VQdeE6rqNCYigbk.png)

## 创建工程

这里主要介绍的是 VSCode 下如果创建工程：

### 新建工程

可以在`Control+p`, 输入`>flutter: new project`创建一个工程。

![20191126135520.png](https://i.loli.net/2019/11/26/xlueJjtZpnBomdT.png)

![20191126135624.png](https://i.loli.net/2019/11/26/OPZtFpxqNVu5KUn.png)

### 选择设备

点击 VScode 右下角的设备，然后选择刚刚在 AS 下载的模拟器。

![20191126135859.png](https://i.loli.net/2019/11/26/rs6M7ycYdNwGFBA.png)

若打开成功，会自动启动模拟器，比如：

![20191126140017.png](https://i.loli.net/2019/11/26/wSQROUgpnAZ3toD.png)

### 启动项目

输入命令: `flutter run`，等待依赖下载。

![20191126140118.png](https://i.loli.net/2019/11/26/WGHX3y8akN4BtPJ.png)。

成功启动后，会看到以下界面。

![20191126140228.png](https://i.loli.net/2019/11/26/WkeABud1zoa9OTh.png)

### 调试

选择到调试（虫子），添加调试配置，只管添加配置，然后保存就好了。选择左上方的开启调试。项目开始打包构建安装到选择的选择的设备上。

![20191126140429.png](https://i.loli.net/2019/11/26/SNWMuJsrqOVlogU.png)

配置可以使用这个配置，并保存：

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "dart",
      "name": "Flutter",
      "request": "launch"
    }
  ]
}
```

成功调试后会出现中间的调试面板控制器。
![20191126140739.png](https://i.loli.net/2019/11/26/F6mHYtoes2WkpMx.png)

### 热重载

lib/main.dart 中编辑插入 Text('hello flutter')，保存文件，你会发现效果会立马呈现到 App 上，开发如此丝滑。有点类似前端的 webpack 实现的热重载功能。

![20191126140935.png](https://i.loli.net/2019/11/26/rOgc5fHVveZpXPt.png)
