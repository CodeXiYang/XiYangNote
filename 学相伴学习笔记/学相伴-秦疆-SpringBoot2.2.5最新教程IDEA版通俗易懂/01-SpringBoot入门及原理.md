# SpringBoot入门

## 1. SpringBoot简介

### 1.1 什么是Spring

Spring是一个开源框架(<span style="color:red">Spring就是一个容器</span>)， 2003年兴起的一个轻量级的Java开发框架，作者： Rod Johnson(音乐学博士)

<u>Spring是为了解决企业级应用开发的复杂性而创建的、简化开发。</u>

### 1.2 Spring是如何简化Java开发的

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1. 基于POJO的轻量级和最小侵入性编程；
2. 通过10C，依赖注入（DI）和面向接口实现松耦合；
3. 基于切面（AOP）和惯例进行声明式编程；
4. 通过切面和模版减少样式代码；

### 1.3 什么是SpringBoot?

学过javaweb的同学就知道，开发一个web应用，从最初开始接触Servlet结合Tomcat, 跑出一个HelloWolrld程序，是要经历特别多的步骤； 后来就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；你们有经历过框架不断的演进，然后自己开发项目所有的技术也再不断的变化、改造吗？建议都可以去经历一遍；

---

言归正传，什么是SpringBoot呢，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开发，约定大于配置， you can "just run"，能迅速的开发web应用，几行代码开发一个http接口。所有的技术框架的发展似乎都遵循了一条主线规律：从一个复杂应用场景 衍生 一种规范框架，人们只需要进行各种配置而不需要自己去实现它，这时候强大的配置功能成了优点；发展到一定程度之后，人们根据实际生产应用情况，选取其中实用功能和设计精华，重构出一些轻量级的框架；之后为了提高开发效率，嫌弃原先的各类配置过于麻烦，于是开始提倡“约定大于配置”，进而衍生出一些一站式的解决方案。是的这就是Java企业级应用->J2EE->spring->springboot的过程。

---

随着 Spring 不断的发展，涉及的领域越来越多，项目整合开发需要配合各种各样的文件，慢慢变得不那么易用简单，违背了最初的理念，甚至人称配置地狱。Spring Boot 正是在这样的一个背景下被抽象出来的开发框架，目的为了让大家更容易的使用 Spring 、更容易的集成各种常用的中间件、开源软件；Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），SpringBoot 应用中这些第三方库几乎可以零配置的开箱即用。简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。

---

Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

---

Spring Boot的主要优点：

1. 为所有Spring开发者更快的入门

2. **开箱即用**，提供各种默认配置来简化项目配置

3. 内嵌式容器简化Web项目

4. 没有冗余代码生成和XML配置的要求

### 1.4 什么是微服务？

微服务是一种架构风格，它要求戴们在开发一个应用的时候，这个应用必须构建成一系列小服务的组合；可以通过http的方式进行互通。要说微服务架构，先得说说过去我们的单体应用架构。

### 1.5 单体应用架构

所谓单体应用架构（all in one）是指，我们将一个应用的中的所有应用服务都封装在一个应用中。

无论是ERP、CRM或是其他什么系统，你都把数据库访问， web访问，等等各个功能放到一个war包内。

这样做的好处是，易于开发和测试；也十分方便部罢；当需要扩展时，只需要将war复制多份，然后放到多个服务器上，再做个负载均衡就可以了。

单体应用架构的缺点是，哪怕我要修改一个非常小的地方，我都需要停掉整个服务，重新打包、部署这个应用war包。特别是对于一个大型应用，我们不可能吧所有内容都放在一个应用里面，我们如何维护、如何分工合作都是问题。

### 1.6 微服务架构

all in one的架构方式，我们把所有的功单元放在一个应用里面。然后我们把整个应用部署到服务器上。如果负载能力不行，我们将整个应用进行水平复制，进行扩展，然后在负载均衡。

所谓微服务架构，就是打破之前all in one的架构方式，把每个功能元素独立出来。把独立出来的功能元素的动态组合，需要的功能元素才去拿来组合，需要多一些时可以整合多个功能元素。所以微服务架构是对功能元素进行复制而没有对整个应用进行复制。

这样做的好处是：

1. 节省了调用资源。

2. 每个功能元素的服务部是一个可替换的、可独立升级的软件代码。

   ![image-20201111133544361](assets/image-20201111133544361.png)

   >  *MartinFlower于2014年3月25日写的《Microservices》，详细的阐述了什么是微服务。*
   >
   > *原文地址：http://martinfgwler.com/articles/microservices.html*
   >
   > *翻译：https://www.cnblogs.com/liuning8023/p/4493156.html*

### 1.7 如何构建微服务

一个大型系统的微服务架构，就像一个复杂交织的神经网络，每一个神经元就是一个功能元素，它们各自完成自己的功能，然后通过http相互请求调用。比如一个电商系统，查缓存、连数据库、浏览页面、结账、支付等服务都是一个个独立的功能服务，都被微化了，它们作为一个个微服务共同构建了一个庞大的系统。如果修改其中的一个功能，只需要更新升级其中一个功能服务单元即可。但是这种庞大的系统架构给部署和运维带来很大的难度。于是，spring为我们带来了构建大型分布式微服务的全套、全程产品：

- 构建一个个功能独立的微服务应用单元 可以使用springboot，可以帮我们快速构建一个应用；
- 大型分布式网络服务的调用，这部分由spring cloud来完成，实现分布式；
- 在分布式中间，进行流式数据计算、批处理，我们有spring cloud data flow.
- spring为我们想清楚了整个从开始构建应用到大型分布式应用全流程方案。

![image-20201116125927275](assets/image-20201116125927275.png)



## 2. SpringBoot入门程序

> 通过SpringBoot构建一个快速入门的程序"Hello World"
>
> 搭建环境: jdk1.8丶maven3.6.1丶SpringBoot2.2.0丶IDEA

### 2.1 IDEA构建SpringBoot工程

1. 创建项目

   ![image-20201116142350422](assets/image-20201116142350422.png)

2. 从springboot官方构建springboot项目

   ![image-20201116142419311](assets/image-20201116142419311.png)

3. 为springboot项目添加一些项目信息

   ![image-20201116142755777](assets/image-20201116142755777.png)

4. 为springboot项目勾选一些需要的jar包

   ![image-20201116142903324](assets/image-20201116142903324.png)

5. 选择项目存放的地址

   ![image-20201116142959497](assets/image-20201116142959497.png)

6. 当项目构建成功后的目录结构如下

   ![image-20201116143449333](assets/image-20201116143449333.png)

### 2.2 启动项目

点击**启动类**中的▶️启动;记住**启动类**,后面会多次提到

![image-20201116143848356](assets/image-20201116143848356.png)

日志如下表示成功

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::       (v2.2.11.RELEASE)

2020-11-16 14:35:34.797  INFO 21844 --- [           main] c.x.Springboot01HellowrldApplication     : Starting Springboot01HellowrldApplication on DESKTOP-6S61DQD with PID 21844 (E:\A-XiYang\XiYangCode\学相伴学习代码\秦疆-springboot\springboot-01-hellowrld\target\classes started by ASUS in E:\A-XiYang\XiYangCode\学相伴学习代码\秦疆-springboot\springboot-01-hellowrld)
2020-11-16 14:35:34.800  INFO 21844 --- [           main] c.x.Springboot01HellowrldApplication     : No active profile set, falling back to default profiles: default
2020-11-16 14:35:35.763  INFO 21844 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2020-11-16 14:35:35.779  INFO 21844 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2020-11-16 14:35:35.779  INFO 21844 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.39]
2020-11-16 14:35:35.862  INFO 21844 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2020-11-16 14:35:35.862  INFO 21844 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1006 ms
2020-11-16 14:35:36.045  INFO 21844 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2020-11-16 14:35:36.214  INFO 21844 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2020-11-16 14:35:36.217  INFO 21844 --- [           main] c.x.Springboot01HellowrldApplication     : Started Springboot01HellowrldApplication in 1.821 seconds (JVM running for 3.28)
```

### 2.3 Hello World

>  编写一个hello wrold程序并运行

1. 在项目启动类同级的位置创建一个controller包,并创建一个对应的类

   ![image-20201116144252334](assets/image-20201116144252334.png)

   编写如下的程序

   ```java
   @RestController //注册到spring中
   public class HelloWorldController {
       @RequestMapping("/hello")//支持String返回
       public String hello(){
           return "hello world";
       }
   }
   ```

2. 重新启动项目,然后在浏览器地址中进行访问,springboot的默认端口是8080

   ![image-20201116144346002](assets/image-20201116144346002.png)

   *ps: 如果不想默认的8080端口可以在配置文件中使用如下配置进行端口修改*

   ```properties
   # 修改项目的端口号
   server.port=8081
   ```

   重启项目访问

   ![image-20201116145705644](assets/image-20201116145705644.png)

小结:

1. 创建的包,例如pojo包,controller包,dao包都需要创建在启动类同级的文件夹下,否则无法扫描
2. springboot的默认端口号是8080,如果没有在配置文件中进行配置端口的话,浏览器地址访问的时候就是`http://localhost:8080/hello`

### 2.4 彩蛋

>  可以修改springboot项目的启动标志

![image-20201116151057211](assets/image-20201116151057211.png)



图案可以到：https://www.bootschool.net/ascii 这个网站生成，然后拷贝到文件中即可

![image-20201116151210877](assets/image-20201116151210877.png)

到项目下的 resources 目录下新建一个banner.txt ,把上面copy的图案复制到这个文件里面

重启项目,可以看到启动的kogo被修改了

![image-20201116151352105](assets/image-20201116151352105.png)



## 3. SpringBoot自动装配原理





## 4. Yaml语法学习



## 5. 注入配置文件



## 6. 多环境切换



## 7. 自动配置原理



## 8. 自定义starter