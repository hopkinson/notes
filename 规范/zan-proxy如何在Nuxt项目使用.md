# zan-proxy 如何在 Nuxt 项目使用

## 代理工具的意义

在开发中，有几个常见的场景：

1. 如果想调试微信公众号的页面或者嵌入 APP 的 H5 页面，都要需要把页面放到线上测试环境进行调试。【提问：能不能本地调试 APP 的 H5 页面，能不能本地调试微信公众号页面】

2. 本地开发域名一般是 localhost:8080，如果有新旧项目结合需要 cookie 共享，一般是修改系统的 host，使得二级域名一样。但是只能修改域名，端口还是存在，比如：xxx.com:8080。【提问：能不能本地调试的时候跟测试环境/正式环境的域名完全一样呢？】

## zan-proxy

![2019-09-19-10-32-29](http://qn.cawsct.com/2019-09-19-10-32-29.png)

> 官方介绍是：本地代码调试线上页面，环境再也不是问题。

本篇文章就不介绍 zan-proxy 的优势以及能做什么，其实除了 zan-proxy。GitHub 开源的工具也有几个，比如：whistle。但是看到 zan-proxy 的 ui 比较好看，所以就推荐使用。

## 如何在 nuxt 项目使用

### 所需工具

1. 浏览器扩展程序 —— Proxy SwitchyOmega
2. node&npm
3. SSL 证书（原因是：本地环境有些资源是 https，比如微信 js-sdk 和 OSS 的地址。如果页面是 http，会打不开 https 资源）

### 安装和启动 zan-proxy

#### 1. 安装

```javascript
npm i -g zan-proxy
zan-proxy --version // 安装校验
```

#### 2. 启动

```javascript
zan - proxy;
```

### 安装和配置 Proxy SwitchyOmega

#### 1.安装

1. 如果能科学上网的童鞋，那就直接进去“打开 chrome 网上商店”进行搜索 Proxy SwitchyOmega 下载。
2. 如果不能科学上网的童鞋，可以直接百度搜索 Proxy SwitchyOmega，官网下载，解压压缩包然后安装。

#### 2.配置

1. 进入 SwitchyOmega 的配置页，新建情景模式。
   ![2019-09-19-10-40-22](http://qn.cawsct.com/2019-09-19-10-40-22.png)

2. 设置情景模式的代理协议为 HTTP，代理服务器为 127.0.0.1，端口为 8001（ ZanProxy 的默认代理端口）
   ![2019-09-19-10-40-45](http://qn.cawsct.com/2019-09-19-10-40-45.png)

3. 进入选项，修改 auto switch 的情景模式，修改我们即将要修改 host 的域名，并为它选择 zan-proxy 情景模式。把多余的条件类型删掉，默认情景模式改为“直接连接”代理模式
   ![2019-09-19-11-07-23](http://qn.cawsct.com/2019-09-19-11-07-23.png)
4. 切换情景模式：auto switch
   ![2019-09-19-10-44-09](http://qn.cawsct.com/2019-09-19-10-44-09.png)

### 修改 Nuxt 配置

#### 1. ssl 证书放进项目里

推荐放到 static 文件夹下
![2019-09-19-10-46-43](http://qn.cawsct.com/2019-09-19-10-46-43.png)

#### 2. 修改 nuxt.config.js 的配置

nuxt 的[API](https://zh.nuxtjs.org/api/configuration-server/) 为我们提供了 https 的配置，只需要在 server 加入即可（但是，我加了也不行）

![2019-09-19-10-46-58](http://qn.cawsct.com/2019-09-19-10-46-58.png)

### 3. 修改 server 启动文件

在 server/index.js 里，在 http 后面加入 https 的配置。

```javascript
import https from "https";
// https相关 - dev开发环境才需要
if (process.env.NODE_ENV !== "production") {
  const httpsServer = https.createServer(
    config.server.https,
    this.app.callback()
  );
  httpsServer.listen(443, host, function() {
    console.log("https启动...");
  });
}
```

启动的时候，访问：https://localhost:443即可。

### zan-proxy 修改 host

进去 Host 管理，首先创建 Host 文件。然后在刚创建的 Host 文件里添加域名和对应的 IP 地址

![2019-09-19-10-51-37](http://qn.cawsct.com/2019-09-19-10-51-37.png)

### 可以访问

记得切换 auto switch 模式，然后就可以访问https://m-test.wbiaotest.cn/，现在情景为本地代码，而不是线上测试环境

### 手机抓包

如果我们需要抓包，而且能够实时观察界面。需要手机代理设置。**前提条件：手机网络和电脑网络要在同一网段才行**

#### 1. 在 wifi 中设置代理

server 为运行 proxy 机器的 ip；port 为 ZanProxy 的代理端口（8001）

#### 2. 手机打开页面

手机可以实时看到界面。

## 最后

zan-proxy 目前为我们提供方便修改 host，抓包等功能，其实它还有 Mock, 修改请求地址等功能。但是我们还没有实际运用，感觉其他不是我们迫切需要的，希望以后能运用起来。
