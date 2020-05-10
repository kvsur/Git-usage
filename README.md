本文档结合经验总结一些GIT在项目中使用的最佳实践，希望能对大家有一定的帮助，如果你有自己的一些最佳实践，也可以添加到这里来，供大家一起学习使用

### GIT 的一些基础概念
![d](https://image-static.segmentfault.com/928/697/928697185-581eed269d236_articlex)

| - | - |
| ---| --- |
| ** Remote ** | 远程仓库源/origin |
| ** Repository ** | 本地仓库（git commit 之后就是更新本地仓库）|
| ** Index ** |暂存区（git add 之后就是在更新暂存区，包括merge之后解决的冲突文件）|
| ** Workspace  **  |本地代码，包括修改的部分|

### 常用GIT 操作

#### Config 本地Git 基础配置
##### 1. 用户信息配置
```bash
# 操作全局配置时候需要在config 后加 --global
# 查看配置项目配置信息，-l 为 --list简写
$ git config -l

# 更新配置 git config module.key value
$ git config user.age 25

# 全局用户信息配置
$ git config --global user.name licheng
$ git config --global user.email licheng@ths.com

# 针对当前用户生成公钥，用来添加托管平台的权限（github，gitlab）
# 建议在clone项目的时候使用ssh协议方式，而不是http或者https协议方式
# 在用户目录.ssh文件夹找到id_rsa.pub 复制其里内容到托管平台即可
# -t 指定密钥类型 -C 注释，这里一般是我们的自己的邮箱或者名称
$ ssh-keygen -t rsa -C licheng@ths.com
```

##### 2. short cmd 简写命令配置（个人常用配置）

```bash
# 全局alias，命令简写配置(可根据个人喜好来配置)
$ git config alias.co 'checkout'
$ git config alias.st 'status'
$ git config alias.ci 'commit'
$ git config alias.br 'branch'
$ git config alias.aa 'add .'
$ git config alias.cim 'commit -m'
$ git config alias.ps 'push'
$ git config alias.pl 'pull'
$ git config alias.brr 'branch -r'
$ git config alias.bra 'branch -a'
$ git config alias.rs 'reset'
$ git config alias.ar 'archive'
$ git config alias.ro 'remote'
$ git config alias.cp 'cherry-pick'
$ git config alias.rb 'rebase'

# 也可以通过vim的方式编辑配置，加上--global是全局
$ git config --global -e
```

#### .gitignore 文件
> .gitignore 文件用于指定项目开发中不需要git 维护版本管理的文件，可以包括文件，文件夹（正则匹配与实名匹配）
> 开发过程临时需要添加 ignore规则时，修改之后需要首先单独提交.gitignore文件才能生效规则

```.gitignore
# 所有js后缀文件
*.js
# 排除main.js 文件，!表示非，不进行对应的匹配
!main.js
# 所有a或者b后缀文件
*.[ab]
# 所有以两位字母数字下划线组成后缀的文件
*.\w{2}
# node_modules 文件或者文件夹
node_modules
# node_modules文件夹
/node_modules
# 只匹配file文件
licheng:
# 只匹配dir目录及其下的所有，不匹配dir文件
licheng/:
```

有时候会出现在项目中新增了一个文件但是git st（status）的时候并没有记录，说明.gitignore配置规则有问题
可以使用以下方式排除问题：
```bash
# 假设在dir目录下添加了file.js没有看到记录
$ git check-ignore -v dir/file.js
#echo: .gitignore:3:dir/*.js  dir/file.js
# 输出表示在.gitignore 配置的第三行中dir/*.js配置规则 将 dir/file.js匹配到之后进行屏蔽
# 然后就可以按需修改了，如果真是针对这一个文件做处理，可以在dir/*.js 下添加一行 !dir/file.js即可
```

#### Clone 远程代码克隆
```bash
# 基础，完成之后文件夹名称是demo
$ git clone git@ths.licheng.com/proejcts/demo.git 
# 可指定文件目录名称，完成之后是 renameDemo
$ git clone git@ths.licheng.com/proejcts/demo.git renameDemo 
# 克隆master 在本地创建 dev分支，自动切换到dev
$ git clone -b dev git@ths.licheng.com/proejcts/demo.git 
```
#### Remote （代码迁移）
当我们在做代码迁移的时候，就可以用到remote 命令；

##### 什么是remote

```bash
# origin的使用：git push originName dev, 就是将本地dev分支的变动推送到 远程 originName 源的dev 分支
# 仓库源的配置查看，origin 是可以配置多个的
$ git remote -v 

# 重命名源
$ git remote rename oldOrigin newOrigin

# 添加源
$ git remote add licheng git@licheng.com/projects/demo.git
$ git remote add originName gitSourceURL

# 移除添加过的源
$ git remote remove originName

# 修改源的托管地址
$ git remote set-url originName originURL

# 平时开发我们一般都只会维护一个源，如果有多个源的时候
# 相关操作都需要带上originName，默认的originName 就是origin
# 不建议同时维护多个源，如果需要的时候要注意区分 
$ git push licheng dev # 推送之前会做分支是否存在的检测
$ git push origin master
```

##### 代码迁移

```bash
# 以下是代码迁移一般流程，代码迁移一般是从一个源移动到另一个源，
# 比如从 gitlab.ths.com/znkf/frontend
# 到 aipx-git.ths.com/znkf/frontend（这里一般是一个空项目）
$ git remote -v # check origin --> origin git@gitlab.ths.com/znkf/frontend.git
$ git remote remove origin
$ git remote add origin git@aipx-git.ths.com/znkf/frontend.git
$ git add .
$ git commit -m 'change origin'
$ git push -u origin --all
# 以上就基本能完成迁移工作

# 也可以通过下面的方式
$ git remote set-url origin git@aipx-git.ths.com/znkf/frontend.git
$ git add .
$ git commit -m 'change origin'
$ git push -u origin --all
```

#### 开发过程中GIT的使用
##### 信息查看（status, log, shortlog, blame, diff, show, reflog）
```bash
# 显示本地及暂存区的变化(基础)
$ git status
# 如果要显示具体的代码修改，也可以指定对应的文件，显示文件中对应哪些行发生了变化
$ git status -v
$ git status src/action/main.java -v
# 极简显示（仅显示对应的文件名，及状态）
$ git status -s


# log 针对的维度是 分支对应的 commit（本地及远程）
# 显示全部日志（当前分支）,[提交信息，提交人，日期，commit id]
$ git log
# 显示指定条数的日志记录
$ git log -3
# 仅显示日志的message 部分
$ git log --oneline
# 显示日志具体有哪些文件发生了变化
$ git log --stat
# 再上一个命令的基础上显示文件具体的变化
$ git log -p
# 针对单个文件维度查看日志
$ git log src/main.js
# 以上所有的参数都是可以组合使用（如下）
$ git log --stat -3 --oneline -p src/main.js


# 日志信息统计(以提交人作为统计维度，显示提交人次数及具体提交信息)
$ git shortlog
# 仅仅统计提交次数(从大到小次数排序)
$ git shortlog -sn

# 显示指定文件在具体什么时间点进行过修改，由谁操作（整个文件会列出来，每一行对应一个时间点）
$ git blame src/main.js

# diff操作
# 显示暂存区和工作区的对比（如果修改之后使用add，则该部分不做对比），--stat仅仅显示文件变化，没有详细信息, --shortstat 仅仅显示文件变化个数
$ git diff
$ git diff src/main.js
$ git diff --stat
$ git diff --shortstat
$ git diff --stat src/main.js
# 显示工作区与最新commit之间的对比(add 之后的内容也会做对比)
$ git diff HEAD
$ git diff HEAD src/main.js
# 显示暂存区与最新commit之间的对比
$ git diff --cached
$ git diff --cached src/main.js
# 显示暂存区和版本库之间的对比（不是最新commit，最新commit 可能是其他人提交的，这里的指的是已经pull或fetch到本地的代码）
$ git diff --staged

# show 操作针对的维度是 commit
# 显示最近一次commit 变化
$ git show
# 显示指定commit 的变化
$ git show commitId

# 显示本地仓库的变化历史（包括了chckeout，commit，merge， reset，rebase，cherry-pick 等操作 ）
$ git reflog
```

##### 添加，删除，提交（add, rm, commit）
```bash
# add 操作支持全部add或者部分add
# 使用正则匹配(多个同时匹配)(建议使用)
$ git add index.html src/main.js src/imgs/*.png src/views/follow/*
# add 当前所有工作取的更改
$ git add .

# rm 操作，可以使用正则匹配，如果文件已经add操作则不能 rm
$ git rm file.js dir/file.js dir/ddd/*

# commit (ci)可以使用正则匹配, -m 可以不用，git 会跳转vim供编辑信息（推荐，可以多行编辑）
$ git ci index.html src/main.js src/imgs/*.png src/views/follow/* -m 'message info'
# 集合add 步骤，为add文件也同时ci(未add文件不能是新建的文件)
$ git ci -a src/main.js -m 'message info'
# 修改上一次commit的信息
$ git ci --amend
$ git ci --amend -m 'message info'
# 继承上一次提交的信息继续提交文件
$ git ci src/main.js --amend
# 提交暂存区的所有文件(如果改动很多的时候不推荐使用这样的方式，不能很好的区分提交信息)
$ git ci . -m 'message info'
$ git ci .
```
##### 分支操作（branch，checkout）
##### 远程同步（fetch, pull, push）
##### 撤销操作（checkout, reset, revert, stash ）
##### 合并（merge, cherry-pick, rebase）
##### 标签（tag）

### GIT 常用工具推荐
#### Gitlab merge request
#### Gitlens
#### TortoiseGit
