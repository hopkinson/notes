# 手牵手使用 Husky & commitlint 规范 commit messages

## 背景

在多人协作项目中，如果代码风格统一、代码提交信息的说明准确，那么在后期协作以及 Bug 处理时会更加方便。
因此，在 git push 代码之前检测 commit messages：

1. commitlint
2. husky

## commitlint

### 1. 全局安装

npm install -g @commitlint/cli @commitlint/config-conventional

### 2. 新建配置文件

```
/**
build：主要目的是修改项目构建系统(例如 glup，webpack，rollup 的配置等)的提交
ci：主要目的是修改项目继续集成流程(例如 Travis，Jenkins，GitLab CI，Circle等)的提交
docs：文档更新
feat：新增功能
fix：bug 修复
perf：性能优化
refactor：重构代码(既没有新增功能，也没有修复 bug)
style：不影响程序逻辑的代码修改(修改空白字符，补全缺失的分号等)
test：新增测试用例或是更新现有测试
revert：回滚某个更早之前的提交
chore：不属于以上类型的其他类型
wip：移除文件或者代码

@example fix: 修复某某功能
 */

module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      [
        'init',
        'build',
        'ci',
        'docs',
        'fix',
        'perf',
        'feat',
        'style',
        'refactor',
        'test',
        'revert',
        'chore',
        'wip'
      ]
    ],
    'subject-full-stop': [0, 'never'],
    'subject-case': [0, 'never']
  }
}

```

## husky

husky 继承了 Git 下所有的钩子，在触发钩子的时候，husky 可以阻止不合法的 commit,push 等等。注意使用 husky 之前，必须先将代码放到 git 仓库中，否则本地没有.git 文件，就没有地方去继承钩子了。

```
npm install husky --save-dev
```

安装成功后需要在项目下的 package.json 中配置

```
"gitHooks": {
    "pre-commit": "lint-staged"
  },
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -e $HUSKY_GIT_PARAMS"
    }
  }
```

最后我们可以正常的 git 操作

```
git add .
git commit -m ""
```

git commit 的时候会触发 commlint。下面演示下不符合规范提交示例：

```
F:\accesscontrol\access_control>git commit -m "featdf: aas"
husky > npm run -s commitmsg (node v8.2.1)

⧗   input:
featdf: aas

✖   type must be one of [feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert] [type-enum]
✖   found 1 problems, 0 warnings

husky > commit-msg hook failed (add --no-verify to bypass)

```

上面 message 不符合提交规范，所以会提示错误。

我们修改下 type:

```
F:\accesscontrol\access_control>git commit -m "feat: 新功能"
husky > npm run -s commitmsg (node v8.2.1)

⧗   input: feat: 新功能
✔   found 0 problems, 0 warnings

[develop 7a20657] feat: 新功能
 1 file changed, 1 insertion(+)

```

commit 成功。
