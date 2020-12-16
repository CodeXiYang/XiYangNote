# IDEA整合Docker

## 1. 创建项目

1. 使用 IDEA 构建一个 SpringBoot 项目

2. 编写一个helloController

   ```java
   
   ```

3. 启动测试下，端口修改下，避免8080冲突！本地访问没问题就可以！

4. 打jar包

## 2. 打包镜像

1. 在项目下编写 Dockerfifile 文件，将打包好的jar包拷贝到Dockerfifile同级目录
2. 将Dockerfifile 和 项目的 jar 包上传到linux服务器上，构建运行

## 3. IDEA安装插件

*了解即可！以后CI/CD，就完全没必要这样做！*

1. IDEA安装插件
2. 配置docker连接集成
3. 集成了docker插件就可以在IDEA中操作Docker内部的容器和镜像了，但是很鸡肋这个功能，对于我们开发人员来说！之后学习的CI/CD才是真正在企业中的王道！

