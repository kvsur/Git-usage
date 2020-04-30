[TOC]

<style>
pre {background: #000;}
</style>

> <p style="color: #409eff; font-size: 14px; font-weight: bold;">
本文档结合经验总结一些GIT在项目中使用的最佳实践，希望能对大家有一定的帮助，如果你有自己的一些最佳实践，也可以添加到这里来，供大家一起学习使用；
</p>

### GIT 的一些基础概念
![d](https://image-static.segmentfault.com/928/697/928697185-581eed269d236_articlex)

| - | - |
| ---| --- |
| ** <span style="color: red;">Remote</span>  ** | 远程仓库源/origin |
| ** <span style="color: orange;">Repository</span> ** | 本地仓库（git commit 之后就是更新本地仓库）|
| ** <span style="color: #b3b30f;">Index</span> ** |暂存区（git add 之后就是在更新暂存区，包括merge之后解决的冲突文件）|
| ** <span style="color: green;">Workspace</span>  **  |本地代码，包括修改的部分|

### 常用GIT 操作

#### Config 本地Git 基础配置
#### Clone 远程代码克隆
#### Remote 代码迁移
当我们在做代码迁移的时候，就可以用到remote 命令；
```
git remote -v // 仓库源的配置查看，origin 是可以配置多个的
// origin的使用：git push originName dev, 就是将本地dev分支的变动推送到 远程 originName 源的dev 分支



```
