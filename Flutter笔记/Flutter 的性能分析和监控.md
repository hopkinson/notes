# 微信小程序集成 jenkins 自动打码

![2020-09-08-14-32-59](http://qn.cawsct.com/2020-09-08-14-32-59.png)

## 背景

在微信小程序的测试或者发布中，如果在没有自动构建（CI/CD）相关工具的情况下，存在着如下的问题：

- 小程序开发助手中，同一个开发者只能显示一个开发版本，可能会出现漏传代码
- 测试同事找开发要二维码，效率较低
- 本地生成的二维码会出现携带本地代码、未及时拉取分支其他改动等问题

所以采用**微信小程序集成 Jenkins** 的方法达到持续集成的效果。

## 回顾以往

在介绍实现方案之前，先来回顾一下常规的微信小程序发布流程。

![2020-09-08-14-33-18](http://qn.cawsct.com/2020-09-08-14-33-18.png)

微信小程序的预览以及上传都是需要在微信开发者工具中进行的。

除了**图形化界面**，微信开发者工具还提供了**命令行**与 **HTTP 服务**两种接口供外部调用，来进行登录、预览、上传等操作。

## 新的方法

### 工具准备

- miniprogram-ci

miniprogram-ci 是从微信开发者工具中抽离的关于小程序/小游戏项目代码的编译模块。开发者可不打开小程序开发者工具，独立使用 miniprogram-ci 进行小程序代码的上传、预览等操作。

详情可以跳转[官网](https://developers.weixin.qq.com/miniprogram/dev/devtools/ci.html)

- 密钥

密钥、存放二维码的目录不能和小程序代码同级，需要分开（不然小程序目录大小会超出限制）

- IP 白名单配置

ip 白名单可能过期，具体在工具运行时会提示，按照提示，在小程序后台更新一下 ip 白名单即可

- appid
- 一个存放打包后二维码的目录

注意：
需要提前安装 nodejs（目前我安装的版本是 v10.8.0，其他版本自行测试）

### 开始

- 安装 miniprogram-ci

```dotnetcli
npm install -g miniprogram-ci
```

- 运行 ci 命令

```bash
miniprogram-ci \
  preview \
  --pp ./demo-proj/ \
  --pkp ./private.YOUR_APPID.key \
  --appid YOUR_APPID \
  --uv PACKAGE_VERSION \
  -r 1 \
  --enable-es6 true \
  --proxy YOUR_PROXY \
  --qrcode-format image \
  --qrcode-output-dest '/tmp/x.jpg'
```

- 出现以下提示，则是成功

![2020-09-08-14-52-33](http://qn.cawsct.com/2020-09-08-14-52-33.png)

### 集成到 jenkins

- 所需插件

1. OWASP Markup Formatter
2. Git
3. set build description
4. Text Finders
5. Git Parameter Plug-In

- 更换 jenkins build 页面描述信息为 html

安装 OWASP Markup Formatter 插件, 在 Manage Jenkins -> Configure Global Security，将 Markup Formatter 的设置更改为 Safe HTML 即可

- 配置 git,用于拉取代码

![2020-09-08-14-54-54](http://qn.cawsct.com/2020-09-08-14-54-54.png)

![2020-09-08-14-55-02](http://qn.cawsct.com/2020-09-08-14-55-02.png)

### 打包命令配置

```bash
#!/bin/sh -l

# 固定一个默认的appid
app_id="XXXXX"

# 判断上面的environment变量值，用于多环境打包判断
case $environment in
"qa")
    app_id="XXX"
;;
"production")
    app_id="XXX"
;;
esac
echo $app_id
echo "-----------当前所选环境为：$environment-----------"
echo "-----------当前环境所对应的appid为：$app_id-----------"

# 因为我这边项目有多个环境的配置文件，这里需要根据前面选择的环境来对应的选择配置文件，大家结合自己项目实际情况来就行
echo "-----------正在复制环境数据config-${environment}.js到config.js-----------"
cd /XXX/Live-Test-Miniprgram/config
sudo rm -f config.js
sudo mv config-${environment}.js config.js
echo "-----------环境数据复制完毕！！！-----------"

time=$(date "+%Y%m%d%H%M%S")

# 此处为ci插件的执行命令
miniprogram-ci \
  preview \
  --pp /XXX/Live-Test-Miniprgram \
  --pkp /XXX/private.$app_id.key \
  --appid $app_id \
  --uv 1.0 \
  --enable-es6 true \
  --preview-page-path "${pagePath}" \
  --qrcode-format image \
  --qrcode-output-dest "/Qrcode_img/${time}.jpg"
```

### 构建后的操作

- 构建后操作添加一个 set build description

- Regular expression 输入(\w\*.jpg) 此处用于匹配日志中的二维码文件名

- Description 输入如下数据

```html
<h5>发布环境:$environment</h5>
<h5>发布分支:$branch</h5>
<img src="图片存放目录\1" height="140" width="140" />
```

![2020-09-08-14-57-05](http://qn.cawsct.com/2020-09-08-14-57-05.png)

### 最后大功告成

![2020-09-08-14-58-42](http://qn.cawsct.com/2020-09-08-14-58-42.png)