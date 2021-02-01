#  Git版本控制

> 课程名称: https://www.it666.com/my/course/188
>
> 课程概述: git的基本使用
>
> 讲述人: 李南江

## 第 1 章: GIT入门

### 版本控制的起源

现在的软件项目通常是由一个研发小组共同分析、设计、编码、维护以及测试的
针对团队开发需要解决以下问题：

- 备份多个版本，费空间，费时间
- 难于恢复至以前正确版本
- 容易引发BUG
- 解决代码冲突困难
- 代码管理混乱
- 难于追溯问题代码的修改人和修改时间
- 无法进行权限控制
- 项目版本发布困难

源代码管理工具就是为了解决上述问题应运而生的

### 版本控制(Revision Control)

是维护工程蓝图的标准做法，能追踪工程蓝图从诞生一直到定案的过程。是一种记录若干文件内容变化，以便将来查阅特定版本修订情况的系统

- 如果是开发团队中的一员，使用版本控制是强制性的！
- 如果是单人开发，也强烈建议现在就开始使用版本控制!

使用版本控制可以：

- 不会对现有工作造成任何损害
- 不会增加工作量
- 添加新的功能拓展时，会变得更加容易

### 常见版本控制工具

CVS 开启版本控制之门

- CVS 1990年诞生，远古时代的主流源代码管理工具

SVN 集中式版本控制之王者

- SVN:又称subversion，是CVS的接班人，是一款集中式源代码管理工具。曾经是绝大多数开源软件的代码管理工具(google code)，前几年在国内软件企业使用最为普遍

GIT 分布式版本控制之伟大作品

- GIT:一款分布式源代码管理工具，目前国内企业几乎都已经完成了从SVN到GIT的转换

### 集中式与分布式

集中式源代码管理: 代表有svn

![image-20210122150717345](assets/image-20210122150717345.png)

![image-20210122150859302](assets/image-20210122150859302.png)

分布式源代码管理: 代表有git

![image-20210122150734177](assets/image-20210122150734177.png)

![image-20210122150909098](assets/image-20210122150909098.png)

**分布式和集中式的最大区别在于：**

- 在集中式下, 开发者只能将代码提交到服务器, 在分布式下, 开发者可以本地提交
- 在集中式下, 只有远程服务器上有代码数据库, 在分布式下, 每个开发者机器上都有一个代码数据库

**集中式与分布式的对比:** 

速度

- 在很多情况下，git的速度远远比SVN快

结构

- SVN是集中式管理，git是分布式管理

其他

- SVN使用分支比较笨拙，git可以轻松拥有无限个分支
- SVN必须联网才能正常工作，git支持本地版本控制工作
- 旧版本的SVN会在每一个目录置放一个.svn，git只会在根目录拥有一个.git



### GIT简介

GIT是一款自由和开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目

在世界上所有的分布式版本控制工具中，git是最快、最简单、最流行的

是Linux之父李纳斯的第二个伟大作品

- 2005年由于BitKeeper软件公司对Linux社区停止了免费使用权。

- Linus为了辅助Linux内核的开发(管理源代码),迫不得己自己开发了一个分布式版本控制工具，从而Git诞生了



## 第 2 章: GIT单人开发

### GIT工作原理

*如果想学好GIT必须先了解GIT的工作原理!*

**工作区(Working Directory)**: 仓库文件夹里面, 除了.git目录以外的内容

**版本库(Repository)**:.git目录, 用于存储记录版本信息

- 版本库中的**暂缓区(staga)**: 暂时存放的区域

- 版本库中的**分支(master)**: git自动创建的第一个分支

- 版本库中的**HEAD指针**:用于指向当前分支

![image-20210122154324282](assets/image-20210122154324282.png)

### ppt演示git是如何做版本管理

*这里一定需要特别理解,让你更明白后面的命令`git add`  `git commit` `gitpush`的含义!*  

1. 首先是在工作区中进行文件处理,例如你增加一个index.html文件

   ![image-20210122154613495](assets/image-20210122154613495.png)

2. 然后通过命令将工作区中的内容提交到暂缓区

   ![image-20210122154630042](assets/image-20210122154630042.png)

3. 最后会将暂缓区中的内容提交到HEAD指针指向的分支上去

   ![image-20210122154639981](assets/image-20210122154639981.png)

### 安装git

git下载地址: https://git-scm.com/downloads

然后全程next...

当在cmd命令窗口中输入`git --version`有git的版本号就表示下载安装成功了

![image-20210122155429942](assets/image-20210122155429942.png)

git安装成功后,你在某一个文件夹下左击,会出现两个git的特殊图标也表示安装成功了

![image-20210122155605553](assets/image-20210122155605553.png)

- Git GUI : 图形化方式操作git
- Git Bash : 命令行方式操作git;如果会使用linux可以在这里面进行一些简单的linux命令操作

*git学习文档: https://backlog.com/git-tutorial/cn/*



### GIT使用环境

多人开发时需要一个共享版本库, 单人开发初始化一个本地库即可

共享版本库的形式:

- 本地共享库: 文件夹/U盘/硬盘
- 远程共享库: 自己搭建git服务器/托管到第三方平台(github/oschina等)

无论是单人开发还是多人开发, 客户端都可以使用命令行或者图形化界面使用git



### git单人开发常用命令

没有对接团队,一个人做版本管理使用git的常用命令大全

#### git help

`git help` 查看git的帮助命令

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/product (master)
$ git help
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone             Clone a repository into a new directory
   init              Create an empty Git repository or reinitialize an existing
one

work on the current change (see also: git help everyday)
   add               Add file contents to the index
   mv                Move or rename a file, a directory, or a symlink
   restore           Restore working tree files
   rm                Remove files from the working tree and from the index
   sparse-checkout   Initialize and modify the sparse-checkout

examine the history and state (see also: git help revisions)
   bisect            Use binary search to find the commit that introduced a bug
   diff              Show changes between commits, commit and working tree, etc
   grep              Print lines matching a pattern
   log               Show commit logs
   show              Show various types of objects
   status            Show the working tree status

grow, mark and tweak your common history
   branch            List, create, or delete branches
   commit            Record changes to the repository
   merge             Join two or more development histories together
   rebase            Reapply commits on top of another base tip
   reset             Reset current HEAD to the specified state
   switch            Switch branches
   tag               Create, list, delete or verify a tag object signed with GPG


collaborate (see also: git help workflows)
   fetch             Download objects and refs from another repository
   pull              Fetch from and integrate with another repository or a local
 branch
   push              Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
See 'git help git' for an overview of the system.

```

`git help 其他指令` 如果你某个指令忘了如何使用,可以使用这种方式查看某个命令的具体使用; 使用后会弹到对应的网页,里面记录了详细命令

```shell
$ git help config
```

#### git init

`git init` 是用来做仓库的初始化操作,让这个文件夹被仓库所管理

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/新建文件夹
$ git init
Initialized empty Git repository in C:/Users/ASUS/Desktop/新建文件夹/.git/

# Initialized empty Git repository: 初始化了一个repository
```





#### git config

`git config user.name "姓名"` 在git中配置你的用户名,因为在做记录追踪的时候需要信息,还需要邮箱

`git config -user.email "邮箱"` 在git中配置你的邮箱,因为在做记录追踪的时候需要信息,还需要姓名

`git config -l` 查看当前git的配置信息

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/新建文件夹 (master)
$ git config -l
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=openssl
http.sslcainfo=D:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager
user.name=CodeXiYang  # 配置了用户名
user.email=CodeXiYang@163.com # 配置了用户邮箱
core.autocrlf=false
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
```

#### git status

`git status` 查看文件当前的状态,被git管理了吗?提交到展缓区了吗?push成功了吗?

如果文件名的颜色是红色(index.txt是红色),表示当前文件并没有被git管理,只是存在于工作区中

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/product (master)
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index.txt

nothing added to commit but untracked files present (use "git add" to track)
```

将index.txt交给git管理,提交到暂缓区 `git add 文件名称`

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/product (master)
$ git add index.txt
```

重新查看当前文件的状态,文件名是绿色的就表示受git管理

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/product (master)
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   index.txt
```

#### git commit

当工作区中的文件通过git add提交到了git的暂缓区后,还没完

需要将暂缓区中的文件提交到 **HEAD指针所指向的分支上** 默认HEAD指针指向的就是master分支上

`git commit -m "提交信息"` 将暂缓区中的代码提交到分支上

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/product (master)
$ git commit -m "初始化项目,新建了一个index.txt文件"
[master (root-commit) 12efd8b] 初始化项目,新建了一个index.txt文件
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 index.txt
```

然后我们在通过git status查看状态,发现暂缓区中的文件被提交到了master分支上了

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/product (master)
$ git status
On branch master
Your branch is based on 'origin/master', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)

nothing to commit, working tree clean
```

#### git diff

git satus还可以查看哪些文件做了内容修改 ; modified: 修改的;只能查看哪些文件做了修改,而不清楚文件里面具体做了什么操作

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/product (master)
$ git status
On branch master
Your branch is based on 'origin/master', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   index.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`git diff 文件名` 可以查看指定文件名修改了哪些内容;例如将index.txt文件中编写一点内容后;最近一次修改的内容会呈绿色文字

注意点: 当文件发生了修改后,他只是在工作区中发生的,并没有受到版本库的管理,需要就修改后的文件受到git管理,就需要像之前一样提交暂缓区`git add` 然后提交到分支`git commit`

```shell
ASUS@DESKTOP-6S61DQD MINGW64 ~/Desktop/product (master)
$ git diff index.txt
diff --git a/index.txt b/index.txt
index e69de29..63ddc8f 100644
--- a/index.txt
+++ b/index.txt
@@ -0,0 +1,2 @@
+写入一点内容
+
```



## 第 3 章: GIT多人开发



## 第 4 章: gitflow工作流程



## 第 5 章: github & git



## 第 6 章: WebStrom使用git







