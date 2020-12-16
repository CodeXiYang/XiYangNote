# Docker常用命令

## 1. Docker帮助命令

```shell
docker version # 显示 Docker 版本信息。 
docker info # 显示 Docker 系统信息，包括镜像和容器数。
docker --help # 帮助
```

*[docker命令文档地址](https://docs.docker.com/engine/reference/commandline/)*

## 2. 镜像命令

### 2.1 docker images

`docker images`用于显示本地镜像

```shell
# 查询本地主机上的所有镜像 
[root@xiyang ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    bf756fb1ae65   11 months ago   13.3kB
```

```shell
# 解释 
# REPOSITORY 镜像的仓库源 
# TAG 镜像的标签 
# IMAGE ID 镜像的ID 
# CREATED 镜像创建时间 
# SIZE 镜像大小 

# 同一个仓库源可以有多个 TAG，代表这个仓库源的不同版本，我们使用REPOSITORY：TAG 定义不同 的镜像，如果你不定义镜像的标签版本，docker将默认使用 lastest 镜像！ 
# 可选项 
-a： 列出本地所有镜像 
-q： 只显示镜像id 
--digests： 显示镜像的摘要信息
```



### 2.2 docker search

`docker search 镜像名`用于搜索镜像的命令

```shell
# 搜索镜像(搜索mysql的镜像)
# docker search 某个镜像的名称 对应DockerHub仓库中的镜像 
[root@xiyang ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10269     [OK]       
mariadb                           MariaDB is a community-developed fork of MyS…   3794      [OK]       
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   749                  [OK]
percona                           Percona Server is a fork of the MySQL relati…   515       [OK]       
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   86                   
...
```

```shell
# 可选项 
--filter=stars=50 ： 列出收藏数不小于指定值的镜像。
[root@xiyang ~]# docker search mysql --filter=stars=5000;
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   10269     [OK]  
```



### 2.3 docker pull

`docker pull 镜像名[:tag]`用于拉取某个镜像,从远程仓库下载下来

```shell
# 下载mysql镜像 
[root@xiyang ~]# docker pull mysql
Using default tag: latest # 如果不写tag(版本),会下载所有,默认使用最后一个latest
latest: Pulling from library/mysql
6ec7b7d162b2: Pull complete # 分层下载,docker images的核心 
fedd960d3481: Pull complete 
7ab947313861: Pull complete 
64f92f19e638: Pull complete 
3e80b17bff96: Pull complete 
014e976799f9: Pull complete 
59ae84fee1b3: Pull complete 
ffe10de703ea: Pull complete 
657af6d90c83: Pull complete 
98bfb480322c: Pull complete 
9f2c4202ac29: Pull complete 
a369b92bfc99: Pull complete 
Digest: sha256:365e891b22abd3336d65baefc475b4a9a1e29a01a7b6b5be04367fcc9f373bb7 # 签名防伪
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest # 真实地址
# docker pull mysql <==等价于==> docker pull docker.io/library/mysql:latest
```

```shell
# 指定版本下载 
[root@xiyang ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
6ec7b7d162b2: Already exists 
fedd960d3481: Already exists 
7ab947313861: Already exists 
64f92f19e638: Already exists 
3e80b17bff96: Already exists 
014e976799f9: Already exists 
59ae84fee1b3: Already exists 
7d1da2a18e2e: Pull complete 
301a28b700b9: Pull complete 
979b389fc71f: Pull complete 
403f729b1bad: Pull complete 
Digest: sha256:d4ca82cee68dce98aa72a1c48b5ef5ce9f1538265831132187871b78e768aed1
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```



### 2.4 docker rmi

`docker rmi 镜像名` 删除指定的镜像

```shell
# 删除镜像 
docker rmi -f 镜像id # 删除单个镜像 
docker rmi -f 镜像名:tag 镜像名:tag # 删除多个 
docker rmi -f $(docker images -qa) # 删除全部(递归删除)
```

```shell
# 删除指定id的镜像
[root@xiyang ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
mysql         5.7       697daaecf703   4 days ago      448MB
mysql         latest    ab2f358b8612   4 days ago      545MB
hello-world   latest    bf756fb1ae65   11 months ago   13.3kB
[root@xiyang ~]# docker rmi -f 697daaecf703
Untagged: mysql:5.7
Untagged: mysql@sha256:d4ca82cee68dce98aa72a1c48b5ef5ce9f1538265831132187871b78e768aed1
Deleted: sha256:697daaecf703e82e8755034e816282fc3e912151b7818c85af8647fdcdcee517
Deleted: sha256:5214ea7c0bfb5429533d91b143604067a50042ae7b371dddb6ae53baadd3f7ef
Deleted: sha256:e9082a53da66fba9c49f51580500919310a36722cc4d3eb0c78e7000ad058655
Deleted: sha256:8615ae1ee613441540ee54a2c517eb0600a6c83667a79f7ca74acc9ffec4c9a4
Deleted: sha256:252efab3ecb7891820c5a340645044850d6edc7815c6588450d74b0a743424f4
[root@xiyang ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
mysql         latest    ab2f358b8612   4 days ago      545MB
hello-world   latest    bf756fb1ae65   11 months ago   13.3kB
```



## 3. 容器命令

 **说明**：有镜像才能创建容器，我们这里使用 centos 的镜像来测试，就是虚拟一个 centos ！

```shell
# 通过docker下载centos
[root@xiyang ~]# docker pull centos
```

### 3.1 新建容器并启动

```shell
# 命令 
docker run [OPTIONS] IMAGE [COMMAND][ARG...] 
# 常用参数说明 
--name="Name" # 给容器指定一个名字 
-d # 后台方式运行容器，并返回容器的id！ 
-i # 以交互模式运行容器，通过和 -t 一起使用 
-t # 给容器重新分配一个终端，通常和 -i 一起使用 
-P # 随机端口映射（大写） 
-p # 指定端口映射（小结），一般可以有四种写法 
	ip:hostPort:containerPort # 主机端口映射到容器端口
	ip::containerPort 
	hostPort:containerPort (常用) 
	containerPort 
```

```shell
# 测试 
[root@xiyang ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
centos       latest    300e315adb2f   8 days ago   209MB
# 使用centos进行交互(it)模式启动容器，在容器内执行/bin/bash命令！ 
[root@xiyang ~]# docker run -it centos /bin/bash
# 注意地址，已经切换到容器内部了! 
[root@3b82a54d32d7 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
# 使用 exit 退出容器(从容器中退出到主机中) 
[root@3b82a54d32d7 /]# exit
exit
```



### 3.2 列出所有运行的容器

```shell
# 命令 
docker ps [OPTIONS] 
# 常用参数说明 
-a # 列出当前所有正在运行的容器 + 历史运行过的容器 
-l # 显示最近创建的容器 
-n=? # 显示最近n个创建的容器;其中?表示个数,例如-n=2,显示的就是最近创建的2个容器 
-q # 静默模式，只显示容器编号。
```

```shell
# 测试
[root@xiyang ~]# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS                          PORTS     NAMES
844ea68968c0   centos         "/bin/bash"   27 seconds ago   Exited (127) 16 seconds ago               eager_roentgen
3b82a54d32d7   centos         "/bin/bash"   2 minutes ago    Exited (0) About a minute ago             musing_bohr
80b9e4bd54c9   bf756fb1ae65   "/hello"      2 hours ago      Exited (0) 2 hours ago                    admiring_noyce
```



### 3.3 退出容器

```shell
# 命令
# 停止容器并退出 
[root@844ea68968c0 /]# exit
exit
# 不停止容器退出
ctrl+P+Q 
```



### 3.4 启动停止容器

```shell
# 命令
docker start (容器id or 容器名) # 通过容器id或容器名 启动容器 
docker restart (容器id or 容器名) # 通过容器id或容器名 重启容器 
docker stop (容器id or 容器名) # 通过容器id或容器名 停止容器 
docker kill (容器id or 容器名) # 通过容器id或容器名 强制停止容器
```



### 3.5 删除容器

```shell
# 命令
docker rm 容器id # 删除指定容器;注意不能删除正在运行的容器,如果要强制删除需要使用 rm -f 
docker rm -f $(docker ps -a -q) # 删除所有容器(递归删除)
docker ps -a -q|xargs docker rm # 删除所有容器(通过linux中的管道来删除)
```



## 4. 常用其他命令

### 4.1 后台启动容器

```shell
# 命令 
docker run -d 容器名 
```

```shell
# 例子 
docker run -d centos # 启动centos，使用后台方式启动 
# 问题： 使用docker ps 查看，发现容器已经退出了！ 
# 解释：Docker容器后台运行，就必须有一个前台进程，容器运行的命令如果不是那些一直挂起的命 令，就会自动退出。 
# 比如，你运行了nginx服务，但是docker前台没有运行应用，这种情况下，容器启动后，会立即自 杀，因为他觉得没有程序了，所以最好的情况是，将你的应用使用前台进程的方式运行启动。
```



### 4.2 查看日志

```shell
# 命令 
docker logs -f -t --tail 容器id 
```

```shell
# 例子：我们启动 centos，并编写一段脚本来测试玩玩！最后查看日志
[root@kuangshen ~]# docker run -d centos /bin/sh -c "while true;do echo kuangshen;sleep 1;done" 
[root@kuangshen ~]# docker ps 
CONTAINER ID IMAGE 
c8530dbbe3b4 centos 
# -t 显示时间戳 
# -f 打印最新的日志 
# --tail 数字 显示多少条！ 
[root@kuangshen ~]# docker logs -tf --tail 10 c8530dbbe3b4 
2020-05-11T08:46:40.656901941Z kuangshen 
2020-05-11T08:46:41.658765018Z kuangshen 
2020-05-11T08:46:42.661015375Z kuangshen 
2020-05-11T08:46:43.662865628Z kuangshen 
2020-05-11T08:46:44.664571547Z kuangshen 
2020-05-11T08:46:45.666718583Z kuangshen 
2020-05-11T08:46:46.668556725Z kuangshen 
2020-05-11T08:46:47.670424699Z kuangshen 
2020-05-11T08:46:48.672324512Z kuangshen 
2020-05-11T08:46:49.674092766Z kuangshen
```



### 4.3 查看容器中运行的进程信息，支持 ps 命令参数

```shell
# 命令 
docker top 容器id 
```

```shell
# 测试 
[root@kuangshen ~]# docker top c8530dbbe3b4 
UID PID PPID C STIME TTY TIME CMD root 27437 27421 0 16:43 ? 00:00:00 /bin/sh -c ....
```



### 4.4 查看容器/镜像的元数据

```shell
# 命令 
docker inspect 容器id
```



### 4.5 进入正在运行的容器

```shell
# 命令1 
docker exec -it 容器id bashShell
# 测试1 
[root@kuangshen ~]# docker ps 
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES c8530dbbe3b4 centos "/bin/sh -c 'while t…" 12 minutes ago Up 12 minutes happy_chaum 
[root@kuangshen ~]# docker exec -it c8530dbbe3b4 /bin/bash 
[root@c8530dbbe3b4 /]# ps -ef 
UID PID PPID C STIME TTY TIME CMD root 1 0 0 08:43 ? 00:00:00 /bin/sh -c while true;do echo kuangshen;sleep root 751 0 0 08:56 pts/0 00:00:00 /bin/bash root 769 1 0 08:56 ? 00:00:00 /usr/bin/coreutils -- coreutils-prog-shebang=s root 770 751 0 08:56 pts/0 00:00:00 ps -ef
# 命令2 
docker attach 容器id
# 测试2 
[root@kuangshen ~]# docker exec -it c8530dbbe3b4 /bin/bash 
[root@c8530dbbe3b4 /]# ps -ef 
UID PID PPID C STIME TTY TIME CMD root 1 0 0 08:43 ? 00:00:00 /bin/sh -c while true;do echo kuangshen;sleep root 856 0 0 08:57 pts/0 00:00:00 /bin/bash root 874 1 0 08:57 ? 00:00:00 /usr/bin/coreutils -- coreutils-prog-shebang=s root 875 856 0 08:57 pts/0 00:00:00 ps -ef 
# 区别 
# exec 是在容器中打开新的终端，并且可以启动新的进程 
# attach 直接进入容器启动命令的终端，不会启动新的进程
```



### 4.6 从容器内拷贝文件到主机上

```shell
# 命令 
docker cp 容器id:容器内路径 目的主机路径 
# 测试 
# 容器内执行，创建一个文件测试 
[root@c8530dbbe3b4 /]# cd /home 
[root@c8530dbbe3b4 home]# touch f1 
[root@c8530dbbe3b4 home]# ls
f1
[root@c8530dbbe3b4 home]# exit 
exit
# linux复制查看，是否复制成功 
[root@kuangshen ~]# docker cp c8530dbbe3b4:/home/f1 /home 
[root@kuangshen ~]# cd /home 
[root@kuangshen home]# ls 
f1
```











## 5. 命令小结

```shell
attach Attach to a running container # 当前 shell 下 attach 连接指定运行镜像 
build Build an image from a Dockerfile # 通过 Dockerfile 定 制镜像 
commit Create a new image from a container changes # 提交当前容器为新的镜像 
cp Copy files/folders from the containers filesystem to the host path #从容器中拷贝指定文件或者目录到宿主机中 
create Create a new container # 创建一个新的容器，同 run，但不启动容器 
diff Inspect changes on a container's filesystem # 查看 docker 容器变化 
events Get real time events from the server # 从 docker 服务获取容 器实时事件 
exec Run a command in an existing container # 在已存在的容器上运行命 令
export Stream the contents of a container as a tar archive # 导出容器的内 容流作为一个 tar 归档文件[对应 import ] 
history Show the history of an image # 展示一个镜像形成历史 
images List images # 列出系统当前镜像
import Create a new filesystem image from the contents of a tarball # 从 tar包中的内容创建一个新的文件系统映像[对应export] 
info Display system-wide information # 显示系统相关信息 
inspect Return low-level information on a container # 查看容器详细信息 
kill Kill a running container # kill 指定 docker 容 器
load Load an image from a tar archive # 从一个 tar 包中加载一 个镜像[对应 save] 
login Register or Login to the docker registry server # 注册或者登陆一个 docker 源服务器 
logout Log out from a Docker registry server # 从当前 Docker registry 退出 
logs Fetch the logs of a container # 输出当前容器日志信息 
port Lookup the public-facing port which is NAT-ed to PRIVATE_PORT # 查看映射端口对应的容器内部源端口 
pause Pause all processes within a container # 暂停容器 
ps List containers # 列出容器列表 
pull Pull an image or a repository from the docker registry server # 从docker镜像源服务器拉取指定镜像或者库镜像 
push Push an image or a repository to the docker registry server # 推送指定镜像或者库镜像至docker源服务器 
restart Restart a running container # 重启运行的容器 
rm Remove one or more containers # 移除一个或者多个容器 
rmi Remove one or more images # 移除一个或多个镜像[无容器使用该 镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除] 
run Run a command in a new container # 创建一个新的容器并运行 一个命令 
save Save an image to a tar archive # 保存一个镜像为一个 tar 包[对应 load] 
search Search for an image on the Docker Hub # 在 docker hub 中搜 索镜像 
start Start a stopped containers # 启动容器 
stop Stop a running containers # 停止容器 
tag Tag an image into a repository # 给源中镜像打标签 
top Lookup the running processes of a container # 查看容器中运行的进程信 息
unpause Unpause a paused container # 取消暂停容器 
version Show the docker version information # 查看 docker 版本号 
wait Block until a container stops, then print its exit code # 截取容 器停止时的退出状态值
```



## 6. Docker安装实战

### 6.1 使用Docker安装Nginx

```shell

```



### 6.2 使用docker安装 tomcat

```shell

```



### 6.3 使用docker 部署 es + kibana

```shell

```



## 7. 可视化



