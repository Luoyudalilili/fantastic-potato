# Git教程

## Git简介

用C开发，分布式版本控制系统。

## 安装Git

打开Git Bash

输入`git` 得Git相关命令

输入以下内容进行设置

```bash
$ git config --global user.name "Lukkkkk"
$ git config --global user.email "luoyudajones@163.com"
```

其中`git config` 命令的`global` 参数表示这台机器上所有的Git仓库都会使用这个配置。

## 创建版本仓库

版本库，仓库：repository

1. 创建一个空目录

```bash
$ mkdir learngit // 新建文件夹
$ cd learngit // 跳转到文件夹
$ pwd // 显示当前目录
```

2. 通过`git init` 命令把这个目录变成Git可以管理的仓库，`ls -ah` 命令可以看见新建的`.git` 目录
3. 先建立一个文件，再用`git add <file>` 把文件添加到版本库

```bash
$ git add readme.txt
```

4. 用`git commit <message>` 把文件提交到仓库

```bash
$ git commit -m "这里是本次提交的说明"
[master (root-commit) e092e58] 这里是本次提交的说明
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 readme.txt

$ git commit -m "在readme.txt文件中加了两行字"
[master 3869ab8] 在readme.txt文件中加了两行字
 1 file changed, 2 insertions(+)
```

中间文件改动过后要先`git add` 才能`git commit` 

## 时光机穿梭

`git status` 可以让我们时刻掌握当前仓库的状态

`git diff <file>  ` 可以查看文件做了哪些修改

### 版本回退

`git log` 命令显示提交历史

`git log --pretty=oneline` 可以少显示很多信息，只显示版本号和提交日志

```bash
LGSM@LAPTOP-HJU3TVHJ MINGW64 /d/Github (master)
$ git log --pretty=oneline
e1db2a067d1fef775ae11d89c8458acac17ca61d (HEAD -> master) add change, a new line
3869ab8cf03178759fd3cd7a3894e3a19b40265d 在readme.txt文件中加了两行字
e092e58c19a66fe48f31d8cf542e99593a42041e 这里是本次提交的说明
```

前面显示的一大串数字为`commit id` 

Git中，用`HEAD` 表示当前版本，用`HEAD^` 表示上一个版本，`HEAD^^` 表示上上个版本，`HEAAD~100` 表示往上100个版本。

`git reset` 可以把当前版本回退到某个版本

```bash
$ git reset --hard HEAD^
HEAD is now at 3869ab8 在readme.txt文件中加了两行字
```

但是要注意回退之后，新版本的找回需要它的`commit id` 

```bash
$ git reset --hard e1db2a // 只要写前几位
HEAD is now at e1db2a0 add change, a new line
```

`git reflog` 用来查看命令历史，可以用来查`commit id` 

### 工作区和暂存区

工作区：Working Directory，例如`Github` 这个文件夹

版本库：Repository，工作区的一个隐藏目录`.git` ，这个不算工作区，而是Git的版本库

版本库中最重要的为称为stage（或者叫index）的暂存区，自动创建的第一个分支`master` ，以及指向`master` 的第一个指针`HEAD` 。

`git add` 实际上就是把文件添加到暂存区，`git commit` 就是把暂存区的所有内容提交到当前分支。

### 管理修改

第一次修改 -> `git add` -> 第二次修改 -> `git commit` ，这样不会提交第二次修改。

两次修改后再`git add` 或者分别`git add` 后再`git commit` 才能提交两次修改的内容。

### 撤销修改

`git checkout -- <file>` 丢弃工作区的修改。

`git reset HEAD <file>` 丢弃提交到暂存区的修改，就到了上一条的情况。

提交到版本库的修改先用版本回退。

### 删除文件

把文件提交到版本库后用`rm <file>` 删除文件后：

1. 删错了，用`git checkout -- <file>` 丢弃修改
2. 用`git rm <file>` 删除后用`git commit` 提交到版本库保存
3. 从来没有被添加到版本库的文件无法恢复

## 远程仓库

创建SSH Key：

```bash
$ ssh-keygen -t rsa -C "luouydajones@163.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/d/Program_Files/Cadence/AppData/SPB_16.5/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /d/Program_Files/Cadence/AppData/SPB_16.5/.ssh/id_rsa
Your public key has been saved in /d/Program_Files/Cadence/AppData/SPB_16.5/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:IQ9EVkVvSZgoR10BuIob80DFSwCNpgo4OvlioHcraN8 luouydajones@163.com
The key's randomart image is:
+---[RSA 3072]----+
|  .+.++oo*+=+.   |
|  o .o= + +o .   |
|.o   oo+..  +    |
|=   . .+.. .     |
|+o . . .S        |
|*   = .          |
|o+   *           |
|+o+ + .          |
|oo.+.E           |
+----[SHA256]-----+
```

### 添加远程仓库

关联远程库：`git remote add origin git@github.com:Luoyudalilili/learnGit.git` 

添加后，远程库的名字为`origin` （默认）

把本地库的所有内容推送到远程库上：

```bash
$ git push -u origin master
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 339 bytes | 339.00 KiB/s, done.
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:Luoyudalilili/learnGit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

```

以上为把当前分支`master` 推送到远程，`-u` 参数使得本地`master` 分支和远程`master` 分支关联，以后推送或拉取时只要`git push origin master` 

`git remote -v` 查看远程库信息

`git remote rm origin ` 删除远程库

### 从远程仓库克隆

先创建一个远程库，用`git clone` 克隆一个本地库：

```bash
LGSM@LAPTOP-HJU3TVHJ MINGW64 /d/Github/learnGit (master)
$ cd /d/Github

LGSM@LAPTOP-HJU3TVHJ MINGW64 /d/Github (master)
$ git clone git@github.com:Luoyudalilili/gitSkills.git
Cloning into 'gitSkills'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

也可以用`https://github.com/Luoyudalilili/gitSkills` 这样的地址，使用`https` ，但`ssh` 协议速度最快。

## 分支管理

### 创建与合并分支

一开始的时候，`master` 分支是一条线，Git用`master` 指向最新的提交，再用`HEAD` 指向`master`，就能确定当前分支，以及当前分支的提交点。

当我们创建新的分支，例如`dev `时，Git新建了一个指针叫`dev`，指向`master `相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev `上。

从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev `指针往前移动一步，而`master `指针不变。

直接把`master `指向`dev `的当前提交，就完成了合并。

合并完分支后，甚至可以删除`dev `分支。删除`dev `分支就是把`dev` 指针给删掉。

**实战部分** 

创建并切换到`dev` 分支：

```bash
$ git checkout -b dev
Switched to a new branch 'dev'
// 等价于
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
// 等价于
$ git switch -c dev
```

`git checkout <branch>` 也等价于`git switch <branch>` 

查看当前分支：`git branch` 

在其他分支上提交的内容在`master` 中不会显示（`master` 分支此刻的提交点没有变），要`git merge dev` 将`dev` 分支的成果合并到`master` 分支上。

```bash
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

合并后删除分支：

```bash
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

### 解决冲突



### 分支管理策略

### Bug分支

### Feature分支

### 多人协作

### Rebase

## 标签管理

### 创建标签

### 操作标签

## 使用GitHub

## 使用GItee

## 自定义Git

### 忽略特殊文件

### 配置别名

### 搭建Git服务器

## 使用SourceTree

## 	期末总结

