## Git 单个文件回退/放弃修改
GIT 的一些基础概念：

| -- | -- |
| ---| --- |
| ** <span style='color: red;'>Remote</span>  ** | 远程仓库源/origin |
| ** <span style='color: orange;'>Repository</span> ** | 本地仓库（git commit 之后就是更新本地仓库）|
| ** <span style='color: #b3b30f;'>Index</span> ** |暂存区（git add 之后就是在更新暂存区，包括merge之后解决的冲突文件）|
| ** <span style='color: green;'>Workspace</span>  **  |本地代码，包括修改的部分|

### 工作区单个文件的修改放弃(Workspace)

> Git 在2.17中新增 两个 命令， restore 和 switch，主要用于分担 checkout 的职责；
>
>[git restore](https://git-scm.com/docs/git-restore)
>
>[git restore or git reset](https://stackoverflow.com/questions/58003030/what-is-the-git-restore-command-and-what-is-the-difference-between-git-restor)

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6823d2ef2eae4dea912df00ed7015b05~tplv-k3u1fbpfcp-watermark.image)

> 注意：这里的默认参数是 --worktree(就是工作区)

### 暂存区单个文件的修改放弃(Index)

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9f3506abfe814c6387274ebdf0344b55~tplv-k3u1fbpfcp-watermark.image)

> 注意：这里这是暂存区的内容发生了变化，工作区的内容不会收影响

### 工作区单个文件恢复到某个提交版本
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2eab7c2ee45d4efe8da583600a3eaae5~tplv-k3u1fbpfcp-watermark.image)

> 恢复操作执行之后需要add

### 暂存区单个文件恢复到某个提交版本

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8ccfae5adb154acc9d8debc98d33e296~tplv-k3u1fbpfcp-watermark.image)

如果需要将工作区与暂存区的某个文件都恢复到之前某个提交版本，则使用checkout来操作

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d3f496f14a8448d383f33fbaee008faf~tplv-k3u1fbpfcp-watermark.image)
