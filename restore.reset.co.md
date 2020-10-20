## Git 单个文件回退/放弃修改

### 工作区单个文件的修改放弃(Workspace)

> Git 在2.17中新增 两个 命令， restore 和 switch，主要用于分担 checkout 的职责；
>
>[git restore](https://git-scm.com/docs/git-restore)
>
>[git restore or git reset](https://stackoverflow.com/questions/58003030/what-is-the-git-restore-command-and-what-is-the-difference-between-git-restor)

```bash
$ git restore src/index.js src/images/file.png
```

> 注意：这里的默认参数是 --worktree(就是工作区)

### 暂存区单个文件的修改放弃(Index)

```bash
$ git restore --stage src/index.js src/images/file.png
```

> 注意：这里这是暂存区的内容发生了变化，工作区的内容不会收影响

### 工作区单个文件恢复到某个提交版本

```bash
$ git restore --source HEAD src/index.js src/images/file.png
$ git restore --source e686fd8sff5ddc3 src/index.js src/images/file.png
```

> 恢复操作执行之后需要add

### 暂存区单个文件恢复到某个提交版本

```bash
$ git reset HEAD src/index.js src/images/file.png
$ git reset e686fd8sff5ddc3 src/index.js src/images/file.png
```

如果需要将工作区与暂存区的某个文件都恢复到之前某个提交版本，则使用checkout来操作

```bash
$ git co HEAD src/index.js src/images/file.png
$ git co e686fd8sff5ddc3 src/index.js src/images/file.png
```
