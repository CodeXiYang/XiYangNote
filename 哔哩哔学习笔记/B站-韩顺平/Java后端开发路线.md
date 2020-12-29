# 2021Java后端开发路线梳理

 

> 视频: https://www.bilibili.com/video/BV14K4y177Qk
>
> 补充: 超全超详细的java学习路线 包括 java javascript html css spring springboot springmvc mybatis redis linux springcloud 数据结构 设计模式 算法等

## 第一阶段:Java基础

*java基础入门的知识点需要搞懂下面这些知识点*

- 变量
- 控制结构
  - 顺序结构
  - 分支结构
  - 循环结构
- OOP
  - 封装
  - 继承
  - 多态
- 数组
- Java API
- 异常和处理
- 集合
- 泛型
- IO
- 反射(框架之魂)
- 网络通信

## 第二阶段: Java高级

*Java高级部分包含 <u>Java多线程/高并发</u> <u>数据结构和算法</u> <u>设计模式</u> <u>JVM</u> 这4个板块的学习*

### 2.1 Java多线程和高并发

- 并发基础
  - 互斥同步
  - 非阻塞同步
  - 指令重排
  - synchronized
  - volatile
- 线程
  - 线程的概念
  - 线程的创建
- 锁
  - 自旋锁
  - 偏向锁
  - 可重入锁
- 线程池
- 并发容器
- JUC
  - executor
  - collections
  - locks
  - atomic（原子类）
  - tools(CountDownlatch, Exchanger, ThreadLocal, CyclicBarrier)

### 2.2 数据结构和算法

**数据结构常见的8种**

- 数组(稀疏数组)
- 队列
- 栈
- 链表
- 树
- 散列
- 堆
- 图

**算法**

- 排序算法(8种)
  - 冒泡
  - 选择
  - 插入
  - 快排
  - 基数排序
  - 归并排序
  - 堆排序
- 查找算法
  - 二分查找
- 分治算法
- 动态规划(解决背包问题)
- 回溯算法(解决骑士周游问题)
- 贪心算法
- KMP
- Prim
- Kruskal
- floyd(解决最短路径)
- 迪杰斯特拉(最短路径)





### 2.3 设计模式

设计模式一共有23种

**常用8种设计模式**

- 单例模式
- 观察者模式
- 工厂模式
- 适配器模式
- 装饰者模式
- 代理模式
- 模板模式
- 职责链模式

**其他设计模式**

- 组合模式
- 桥接模式
- 原型模式
- ...

### 2.4 JVM

- JVM体系
- 类加载过程/机制
- 双亲委派机制/沙箱安全机制
- JMM(Java内存模型)
- 字节码执行的过程/机制
- 垃圾回收算法
- JVM的性能监控和故障定位
- JVM调优

## 第三阶段: JavaWeb

*需要学习前端基础,前端框架(可以弱化),JavaWeb后端*

### 3.1 前端基础

- HTML
- CSS
- JavaScript
- Ajax
- Jquery

### 3.2 前端框架

- Vue
- React
- Angular
- BootStrap
- NodeJS

### 3.3 JavaWeb后端

- Tomcat
- Servlet
- JSP

## 第四阶段: 主流的框架和项目管理

*需要学习java后端主流的框架和项目管理技术*

### 4.1 主流框架

- Linux(操作系统,必学必会)

- Nginx(做反向代理的web服务器)

- SSM

  - Spring(轻量级的容器框架)
  - SpringMVC(分层的web开发框架)
  - MyBatis(持久层ORM框架)

  

### 4.2 项目管理技术

- Maven(解决项目的依赖关系)
- GIT/github/gitlab
- SVN(比较老)

### 4.3 数据库

- Redis
- MySQL
- Oracle

### 4.4 其他框架

- WebService(soa面向服务编程的框架)
- Activiti(工作流框架/引擎)
- Shiro(安全框架,解决安全问题)
- SpringSecurity(安全框架,解决安全问题)
- JPA(持久化,把实体对象持久化到数据库的)
- SpringData(持久层的通用解决方案)

## 第五阶段: 分布式 微服务 并行架构

*这阶段需要学习分布式微服务并行架构*

- Netty(非常重要)
- Dubbo(阿里巴巴开源的RPC框架)
- FastDFS(分布式的文件系统)
- Docker(应用容器引擎)
- Spring家族
  - SpringBoot
  - SpringCloud(组件贼多,每个组件都有自己的功能,归纳一些常见的组件)
    - nacos(阿里巴巴的开源组件,用来做服务发现和配置以及管理)
    - Seata(阿里巴巴的开源组件,用来做分布式事务的中间件)
    - Sentinel(阿里巴巴的开源组件,用来做流量控制,熔断,系统负载保护)
    - GateWay(网关,限流,日志,监控,鉴权)
    - OpenFeign(服务间调用)
- 搜索引擎
  - ElasticSearch
  - Solr
- 中间件
  - MyCat(数据库中间件,分库分表)
  - ActiveMQ(消息中间件)
  - RabbitMQ(消息中间件)
  - KafKa(消息中间件)
- 日志分析与监控(ELK)
  - ElasticSearch(数据收集,存储数据)
  - LogStah(分析日志)
  - Kibana(数据可视化)
- Zookeepr(一致性服务:比如配置维护,域名维护,分布式同步)

## 第六阶段: DevOps(开发运维一体化) 自动化管理项目,解决CI/CD

*DevOps指的是开发运维一体化,也可以认为是自动化部分管理项目,解决的CI/CD*

- k8S(让部署容器化的应用简单高效,部署项目非常高效轻松)
- 普罗米修斯(prometheus): 开源的系统监控和报警的工具
- Jenins(监控持续的工作,例如部署,集成,交互这些工作)
- Harbor(容器的镜像仓库)
- GitLab(项目管理和代码托管的平台)
- Sonargube项目工程代码质量检测

## 第七阶段: 大数据技术

*做java后端的也需要了解一些java大数据相关的技术*

- Hadoop
- Hive
- Impals
- Spark
- Flink



## 第八阶段: 项目

[github高效查找项目学习]()

- 电商类项目
- 金融类项目
- 教育类项目
- 直播类项目
- CRM系统
- ERP系统

## 第九阶段: 大厂高频面试题

- Java高级部分
  - 多线程 / 高并发
  - JVM
- SSM底层源码
- Redis优化
- 中间件
- SpringCloud底层
- Netty
- 项目(贼重要)

## 第十阶段: 底层源码/内核研究

*非常人所能企及*

**框架源码**

**内核二开**



## 编程基础的扩展(科班)

- 计算机网络
- 操作系统
- 编译原理
- 离散数学
- 数值分析
- 计算机组成原理
- 汇编语言(所有高级语言最底层的语言,汇编才是爹)

