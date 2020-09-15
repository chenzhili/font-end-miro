# 由于 构建 微前端 引申出的 知识点；

## 学习到一个 git 指令 查看当前远程的分支
```shell
  git remote -v
```

## 1、lerna 的 理解

lerna 就是一个 便捷的 多项目管理工具(monorepo)，这个管理的方式是通过 git 和 npm 组合管理的，就是 他的命令 是需要通过 git 的 文件修改来定义的
```shell
  lerna init --independent // 可以 单独升级一个包的 版本，而不是 默认的 统一 保持 多项目的 版本一直的提交
```

## 2、yarn 的 workSpaces 的 了解

<!-- https://www.dazhuanlan.com/2019/12/25/5e027015a4775/ -->

通过防止 Workspaces 中依赖包的重复，使原生 Workspaces 到 Yarn 可以实现更快更轻松的依赖安装。Yarn 还可以在依赖于彼此的 Workspaces 之间创建软链接，并确保所有目录的一致性和正确性
理解：就是运用 yarn 的 workSpaces 的方式，让 多个 项目或者包（packages） 之间 可以 间统一 依赖放到 根路径，不同依赖放到 package 中的方式管理；
想 使用这种模式：需要 运行命令
```shell
  yarn config set workspaces-experimental true
```

## 对于 lerna 中 集成 yarn 的方式 管理 包

理解：就是在 lerna 的管理 多项目的情况下，同时 利用 yarn 的 workspaces 的方式 更好的 管理包，防止 重复安装的 方式

### 启用yarn 的 workspaces 的 项目配置
```json
// lerna.json 配置
{
  "npmClient": "yarn",
  "useWorkspaces": true
}
```
```json
// package.json 配置
{
 "private": true, // 这里设置为 true 的原因：根package.json不应该作为一个包使用，它通常会包含与其他项目共享的代码或特定的业务代码，这就是我们将其标记为“私有”的原因
 "workspaces": [
    "packages/*"
 ]
}
```

## 3、微前端的 实现的解决方案
1、架构 需要 lerna、single-spa、systemjs 的运用 
```url
  https://github.com/mongofeng/vue-mic/tree/master
```

2、微前端的 完善 解决方案 的 介绍文章

```url
// 微前端需要处理问题的演进内容
https://zhuanlan.zhihu.com/p/78362028

// 提出了 qiankun 这个 微前端架构
https://github.com/umijs/qiankun
```

### 微前端 需要解决的 问题；
1、如何实现 路由 以及 future state（在刷新后，由于 路由在 对应的 子路由上，但是 子路由 懒加载 导致子模块并未加载， 路由注册表中 没找到，导致404）
```js
  // 路由实现 运用 single-spa
```

2、主项目 和 子项目的 集成方式（构建时打包到一起 和 运行时动态加载），为了 满足 解耦 以及 技术栈的 分离 ，运用 运行时加载

3、子项目 加入主项目的方式 （js entry 和 html entry）
```js
  // 运用 html entry 有一个 实现库 import-html-entry
```

4、对于 应用的 隔离 以及 公共状态的 公用，包括 项目 css的隔离，js的隔离 （如果 用 html entry 的话 ，这块基本不用考虑，因为 销毁 会让 所有销毁）


# 时间推进

## 20200915 查看 import-html-entry 的源码
1、查看了 将 html 用 正则 匹配 提取 其中 的 document、script、style、link 等 内容的；