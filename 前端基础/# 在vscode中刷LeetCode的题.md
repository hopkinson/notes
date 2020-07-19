# 在 vscode 中刷 LeetCode 的题

练习算法绕不开的一个网站就是**力扣**，身边有朋友为了拿到大厂 offer，经常在上面刷题。

然而如果直接在 LeetCode 上写代码，那是很痛苦的一件事，那就相当于用 txt 写代码一样，没有 IDE 的各种功能。毕竟只是一个普通的文本编辑器，在进行算法题训练的初期，我们的主要目标其实不是去记住常用函数的名称和用法，而是需要快速理解和稳固解题思路， 理解算法本身。因此，在一个更智能的编辑环境下做题目，可以帮助提升做题效率，在同样的时间内完成更多的题目，将训练的效果达到最大化。

> VS Code 的 LeetCode 插件帮助我们解决了这一问题。

![Leetcode](https://pic3.zhimg.com/80/v2-f84e8128667efe69eff1b1517e5a9c7e_1440w.jpg)

## 安装和使用

### 安装 LeetCode 插件

首先需要安装的是 Node.JS，因为 LeetCode 插件依赖 Node.JS。

其次在 VS Code 中搜索并安装 LeetCode 插件。安装完成之后，左边会出现一个 LeetCode 图标，见下图：

![vscode 的位置](https://user-gold-cdn.xitu.io/2020/3/29/17123bae7e3f6f23?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 登录 Leetcode

![登录](https://oscimg.oschina.net/oscnet/57899b5fd0fa93a6c34c1723796f0c173bb.gif)

但是你会发现登陆失败了。

怎么回事，为什么失败了，难道是记错密码了吗？

于是你打开 leetcode 的网站又尝试了一下，发现密码没有记错，网页可以登陆。

我们打开官网，会发现官方已经知道登陆失败的问题了，这是由于 leetcode 官网升级了登陆机制导致的。

![登录失败](https://user-gold-cdn.xitu.io/2020/3/29/17123baebb9f9565?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

但是 leetcode 只升级了国际版，对于国内的版本还没有升级，所以如果你使用的是国内的 leetcode 账号，那么我们只需要更换 leetcode 版本即可。更换的方式也很简单，点击上方地球形状的按钮进行选择即可：

![切换语言](https://user-gold-cdn.xitu.io/2020/3/29/17123baee1f8aa8f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 答题

leetcode 的使用很简单，和网页版差距不大，我们点开 all 可以看到所有的问题，我们点击问题的标题会自动为我们加载题目的详细信息，已经通过的问题会打上绿色的勾。

![题目](https://user-gold-cdn.xitu.io/2020/3/29/17123baf3e690da2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

我们要做题的话就右击选择 _Show Problem_

![show problem](https://user-gold-cdn.xitu.io/2020/3/29/17123baf4e306e31?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

之后会弹出语言让我们选择，我们就选择我们最常用的语言就好。

![答题语言](https://user-gold-cdn.xitu.io/2020/3/29/17123baf5405b45f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

之后选择 \*\*Just Open The problem file

![open](https://user-gold-cdn.xitu.io/2020/3/29/17123baf528422b7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

vscode 会自动为我们打开一个分屏。我们就可以一边看问题一边写代码了，不得不说实在是非常方便。

![分屏](https://user-gold-cdn.xitu.io/2020/3/29/17123baf7db0029d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

选择语言之后，需要选择一个 _workspace_。官方文档中说，需要更新配置项 leetcode.workspaceFolder，但是，如果你用的是最新版的 VS Code，就没那么麻烦了。我不得不说，最新版的 VS Code 配置上改变得很好了，以前的配置，如果你不习惯的话，那就是反人类的。新版的配置变得非常的简单了，点点就好。

![workspace](https://user-gold-cdn.xitu.io/2020/3/29/17123baf9aac06f3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

最后，写完之后,Submit 是提交当前的 code 到 leetcode 网站，帮我们提交代码。Test 是执行样例，看看样例是否能够通过。除了这两个之外还有两个，一个叫做 Solution，可以查看当前最高赞的代码。另一个是 Description，是显示问题描述。
![submit](https://user-gold-cdn.xitu.io/2020/3/29/17123bafac1c139b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 总结

入大厂面试必备吧!虽然可能考得很简单,但平时也要多看看.
