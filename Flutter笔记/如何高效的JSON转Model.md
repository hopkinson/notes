# Flutter 中 JSON 转 Model——在线生成

在开发过程中，我们一般都是使用插件或工具一键生成实体类的，这样极大的提高了开发效率，目前我们可以通过在线生成和安装插件生成的方式来一键生成 Dart 类。

## 1. 使用 json_to_dart

1.  首先打开 [json_to_dart](https://javiercbk.github.io/json_to_dart/)

地址：

[https://javiercbk.github.io/json_to_dart/](https://javiercbk.github.io/json_to_dart/)

页面如下：

![2020-06-28-18-56-39](http://qn.cawsct.com/2020-06-28-18-56-39.png)

2. 将 json 数据赋值到输入框中，点击创建 Dart 类，然后右边就是生成好的 Dart 代码，类名可以复制到编辑器后自行修改

![2020-06-28-18-52-45](http://qn.cawsct.com/2020-06-28-18-52-45.png)

3. 使用如下
   ![2020-06-28-18-56-18](http://qn.cawsct.com/2020-06-28-18-56-18.png)

## 2. 使用 json_serializable

json_serializable 是一个自动化的源代码生成器，可以为我们生成 JSON 序列化模板。在 pubspec.yaml 中添加依赖并执行 flutter pub get：

```dart
dependencies:
  json_annotation: ^3.0.0

dev_dependencies:
  build_runner: ^1.0.0
  json_serializable: 3.2.0
```

生成模型类我们使用写的[json2dart](https://caijinglong.github.io/json2dart/index_ch.html)工具。

[https://caijinglong.github.io/json2dart/index_ch.html](https://caijinglong.github.io/json2dart/index_ch.html)

2. 工具使用很简单直接粘贴生成对应的类名称

![json2dart](https://user-gold-cdn.xitu.io/2020/1/6/16f7ab1e6030944d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

3. 将生成代码复制到我们创建的模型中。

![2020-06-28-18-48-01](http://qn.cawsct.com/2020-06-28-18-48-01.png)

4. 运行代码生成程序

上面的模型类生成之后会先报错，因为模型类的生成代码还不存在，所以我们需要运行代码生成器来为我们生成序列化模板。

```shell
# 一次性生成
flutter packages pub run build_runner build

# 持续生成
flutter packages pub run build_runner watch
```

这里选择哪种方式取决于你的改动频率，推荐使用 watch 的方式。

5. 使用

```dart
Map personList = JSON.decode(json);
var list = getPersonModelList(personList);
```

json_serializable 这种方式，我们可以轻松的生成一个模型类。通过源代码生成器创建一个 g.dart 的文件，它具有所有必需的序列化逻辑。

## 3. 使用工具网站 - app.quicktype.io

其实我挺推荐这种。。

[app.quicktype.io](app.quicktype.io)是一个将 JSON 转换成模型类的工具网站，目前来看支持大部分常用语言，并且灵活的可选项也非常多：

![https://user-gold-cdn.xitu.io/2020/1/6/16f7ad6f46ba7a7c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1](https://user-gold-cdn.xitu.io/2020/1/6/16f7ad6f46ba7a7c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

用上面的 JSON 做一下尝试：

![https://user-gold-cdn.xitu.io/2020/1/6/16f7ad8f28990374?imageView2/0/w/1280/h/960/format/webp/ignore-error/1](https://user-gold-cdn.xitu.io/2020/1/6/16f7ad8f28990374?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

生成的模型类是使用了 Flutter 内置的 dart:convert 做序列化。

![2020-06-28-18-51-23](http://qn.cawsct.com/2020-06-28-18-51-23.png)

可以看到这个模型类正是我们需要的，使用方式也在上面注释的很清楚，目前来讲这种方式操作起来会比使用 json_serializable 操作起来更简便一些。

## 总结：

1. json_to_dart: 效率高。
1. json_serializable：效率高，watch 很好用。
1. app.quicktype.io 工具网站：效率高，更多语言和功能可选。
