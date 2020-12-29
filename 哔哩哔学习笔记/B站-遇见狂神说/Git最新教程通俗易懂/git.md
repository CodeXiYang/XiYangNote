# GIT版本控制

## 1. 版本控制入门

### 1.1 什么是版本控制

1. 版本控制是一种在开发过程中用于管理我们对文件丶目录或工程内容的修改历史,方便查看更改历史记录,备份以便恢复以前的版本的软件工程技术

2. 版本控制可以用于管理多人协同开发项目的技术

3. 如图: 版本控制修改历史

   ![image-20200730220404598](assets/image-20200730220404598.png)

### 1.2 常见的版本控制工具介绍

> git     svn     cvs     vss     TFS

重点GIT  SVN



### 1.3 版本控制分类

#### 1.3.1 本地版本控制

> 适合个人使用,例如RCS  记录文件每次的更新,可以对每个版本做一个快照,或是记录补丁文件,适合个人使用;

![image-20200730220959357](assets/image-20200730220959357.png)

#### 1.3.2 集中式版本控制

> 所有的版本更改数据都保存在同一个服务器上,个人是没有版本控制信息的,所有的版本控制信息都在远程服务器上
>
> 缺点:
>
> - 用户的本地只有自己以前所同步的版本，如果不连网的话，用户就看不到历史版本
> - 无法切换版本验证问题，或在不同分支工作
> - 所有数据都保存在单一的服务器上，有很大的风险这个服务器会损坏，这样就会丢失所有的数据，当然可以定期备份

![image-20200730221249874](assets/image-20200730221249874.png)

#### 1.3.3 分布式版本控制

> 所有授权的人都拥有全部代码

![image-20200730221336152](assets/image-20200730221336152.png)

### 1.4 GIT和SVN的区别

- **SVN是集中式版本控制系统**，版本库是集中放在中央服务器的，而工作的时候，用的都是自己的电脑，所以首先要从中央服务器得到最新的版本，然后工作，完成工作后，需要把自己做完的活推送到中央服务器。
- **Git是分布式版本控制系统**，没有中央服务器，每个人的电脑就是一个完整的版本库，工作的时候不需要联网了，因为版本都在自己电脑上。协同的方法是这样的：比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了



### 1.5 git的下载与安装

> 淘宝镜像: http://npm.taobao.org/mirrors/git-for-windows/

任意文件夹右键都有如下,一般使用linux模式命令,效率高,安装git后他会自动配置环境变量,如果需要删除就需要删除环境变量!!

![image-20200730221738025](assets/image-20200730221738025.png)





### 1.6 常用linux命令

> 如上,支持linux的vi命令,所以可以使用一些常用的linux命令

| 指令        | 含义                                                         |
| ----------- | ------------------------------------------------------------ |
| cd 目录名称 | 进入指定目录名称的目录,如果直接`cd` 可进入默认目录`~`        |
| cd ..       | 回到上级目录                                                 |
| pwd         | 显示当前所在的文件路径                                       |
| `ls(ll)`    | 查看当前目录中的所有文件,如果添加了ll文件信息会更详细 注意:不同的文件类型使用颜色去区分的 |
| `touch`     | 创建一个文件,例如创建一个index.html文件touch index.html      |
| `rm`        | 删除一个文件,例如删除刚创建的index.html文件rm index.html     |
| `mkdir`     | 新建一个目录,新建一个目录A <u>mkdir 目录A</u>                |
| `rm -r`     | 删除一个目录,例如删除目录A可使用 <u>rm -r 目录A</u>          |
| `mv`        | 移动目录中的文件,例如将目录A中的file.txt文件移动到目录B中 mv |
| reset       | 效果和reset一样都是用来清屏的                                |
| clear       | linux中清屏可使用clear                                       |
| history     | 查看前面写了哪些命令                                         |
| help        | linuc帮着命令                                                |
| #           | 注释                                                         |
| exit        | 退出                                                         |

### 1.7 Git配置

*配置用户信息,可以追踪到提交人的信息*

7.1 查看所有配置的命令

```visual basic
git config -l
```

7.2 查看不同级别的配置文件

```shell
# 查看系统配置命令
git config --system --list
# 查看用户配置命令
git config --global --list
```

7.3 配置用户名和email(必要)

安装git后的第一步就是设置用户名和email地址,每次提交都会使用这个信息,它会永远的嵌入到提交中

```shell
# 配置用户名
git config --global user.name "黎秦赭"
# 配置邮箱
git config --global user.email "CodeXiYang@163.com"
# 查看配置新
$ git config --global --list
user.name=黎秦赭
user.email=CodeXiYang@163.com
```



7.4 查看本地配置文件

![image-20200730232031003](assets/image-20200730232031003.png)

## 2. git实际操作

### 2.1 git操作本地仓库

#### 2.1.1 创建一个本地仓库

可以使用`git init`创建一个本地仓库

![image-20200730233821961](assets/image-20200730233821961.png)

这时候这个目录A就是一个本地仓库

#### 2.1.2 git分为4个区域

- 工作区: 平时编写代码的地方(除开.git外的区域)
- 暂缓区: 用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息
- 仓库区: 仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本
- 远程仓库: 托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

![image-20200730232809150](assets/image-20200730232809150.png)

#### 2.1.3 git工作流程

1. 在工作区修改文件
2. 将需要进行管理的文件添加到暂缓区`git add .`
3. 将暂缓去中的文件提交到本地仓库`git commit -m"提交信息"`

#### 2.1.3 git管理文件的三种状态

通过git的工作流程可以知道git管理的文件有三种状态,**已修改,已暂存,已提交**可以通过`git status`查看;不同的状态的文件有不同颜色;touch index.html创建一个文件进行如下测试

![image-20200730234354464](assets/image-20200730234354464.png)

![image-20200730234504113](assets/image-20200730234504113.png)

![image-20200730234547317](assets/image-20200730234547317.png)

### 2.2 git操作远程仓库

我们可以在本地操作远程仓库,远程仓库有github和gitee...但是gitee国内的比较快

#### 2.2.1  注册gitee账号创建一个仓库

![image-20200730235243991](assets/image-20200730235243991.png)

- 公开的,任何人都可以访问
- 私有的,只有自己或者授权的人可以访问

![image-20200730235514200](assets/image-20200730235514200.png)

![image-20200730235548817](assets/image-20200730235548817.png)

#### 2.2.2 .gitignore和README.md

- .gitignore文件是用来设定你想提交哪些东西,不想提交哪些东西的

  ```bash
  # 不想提交所有的jar和war
  !*.war
  !*.jar
  # 一般的不管是前端项目还是后端sringboot项目创建的时候都有这个玩意
  
  # 一个很香的网站;会自动生成一些你当前环境的配置,copy就完事了
  	# https://gitignore.io/
  ```

- README.md是用来介绍你这个项目的一些信息的,也可以将个人简历写上去,面试的时候装逼的丢个链接上去(你需要会使用markdown语法才行)

#### 2.2.3 克隆远程仓库的地址

使用`git clone [url]`

这时候这个Demo就是远程仓库了

![image-20200730235806701](assets/image-20200730235806701.png)

然后我们就可以使用前面使用过的操作本地仓库的那3个命令了;增加一个新的命令就是每次先git pull一次,拉取最新的信息

```bash
# 拉取远程仓库最新的信息到本地厂库
git pull
# 将工作区的代码提交到暂缓区
git add . 

# 将暂缓区的代码提交到缓存区
git commit -m"提交"

# 将缓存区中的代码提交到远程仓库
git push
```

**ps: 上面4个命令基本上就是万年命令,常用操作,感觉不太复杂的开发模式这3个基本够用!!复杂的开发模式需要使用分支模型**



小故事理解这几个命令

```txt
	又是周一了,某公司开始了一个新的项目;项目经理在远程仓库以及把厂库创建好了,给小王分配了开发权限,地址是xxx;小王打开电脑进入平时编写代码的文件夹下;使用git clone xxx将项目克隆下来,然后开始写代码...,到了晚上11点,使用git add .将代码提交到暂缓区,然后使用git commit -m"写了个hello word"将暂存区中的代码提交到了缓存区,最后git push到了远程厂库...关电脑下班回家...12点,小李也push了代码
	到了第二天,小王打开电脑,首先git pull,将项目拉取到本地(因为小李也提交了)为了保证最新代码,需要拉取最新项目,顺便还可以瞅瞅小李写的代码如何,然后摸了一天的鱼,写了2个hello word,然后git add . == git commit -m"提交"  == git push一顿操作,然后下班回家...
	到了第三天,小王被开除了.....
```

#### 2.2.4 ssh

我们在执行git clone命令的时候,这里是有两种命令的一种是https的地址,一种是ssh的地址

![image-20200731001908686](assets/image-20200731001908686.png)

如果是使用了https的地址,你进行提交的时候会每次输入用户名和密码,麻烦

#### 2.4.1 ssh的好处

在于你配置后,就是免登陆的,你推送(git push)可以不用输入密码,因为你的电脑已经和远程的仓库关联了

#### 2.4.2 配置ssh

1. 生成公钥`ssh-keygen -t rsa`

   输入这个命令后3次回车完事,出现那个方块图就ok

   ![image-20200731002739712](assets/image-20200731002739712.png)

2. 命令查看公钥`cat ~/.ssh/id_rsa.pub`

   ![image-20200731003141539](assets/image-20200731003141539.png)

3. 打开文件查看

   ![image-20200731003226670](assets/image-20200731003226670.png)

4. 将公钥配置到gitee

   ![image-20200731003338847](assets/image-20200731003338847.png)

   ![image-20200731003502733](assets/image-20200731003502733.png)

5. 进行愉快的操作就不用频繁的输入用户名和密码了

   









### 2.3 idea中使用git

3.1 idea中集成了git的,不喜欢使用命令的手残党福利,不过我不是手残党,点点点就完事

![image-20200731001716792](assets/image-20200731001716792.png)

第一个拉取,第二个提交并推送到远程仓库(不喜欢这种方式..)

3.2 可以在idea中提供的命令行进行操作就是上面那几个万年git操作..

![image-20200731001636935](assets/image-20200731001636935.png)

### 2.4 git分支

我们之前的所有操作就是在master(主分支上的)可以切换到其他分支进行开发;分支模型还没使用到过,等使用完后在来补充下!

常用命令如下:

```bash
# 列出所有本地分支
git branch

# 列出所有远程分支
git branch -r

# 新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 新建一个分支，并切换到该分支
git checkout -b [branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

### 2.5 git命令学习

马云上有的亲.....

![image-20200731004641325](assets/image-20200731004641325.png)







<span style="font-size:40px">git基本使用完结撒花!</span>

<span style="font-size:40px">2020年7月31日00:49:41</span>



## 拓展笔记

*工作上碰到的git操作问题*

1. 查看当前仓库的大小

   ```bash
   git count-objects -vH
   ```

   ![image-20201204115520108](assets/image-20201204115520108.png)

2. 当远程仓库的代码发生变化后进行强制更新到本地

   ```bash
   E:\魔法森林科技有限公司\device-admin>git fetch --all  #强制更新所有
   Fetching origin
   
   E:\魔法森林科技有限公司\device-admin>git reset --hard origin/develop #重定向到本地仓库的develop分支
   HEAD is now at e24fdf8 Delete 新建文本文档.txt
   
   E:\魔法森林科技有限公司\device-admin>git pull origin/develop 
   fatal: 'origin/develop' does not appear to be a git repository
   fatal: Could not read from remote repository.
   
   Please make sure you have the correct access rights
   and the repository exists.
   
   E:\魔法森林科技有限公司\device-admin>git pull #刷新
   Already up to date.
   ```

   

## 参考

> 秦疆老师Java进阶系列课程之GIT使用 带你走进GIT世界； 深入浅出的讲解了GIT使用的全流程以及实战教学！ 狂神说Java系列，努力打造通俗易懂的教程 QQ交流群 : 664386224
>
> 视频地址: https://www.bilibili.com/video/BV1FE411P7B3?from=search&seid=9735183273475919692