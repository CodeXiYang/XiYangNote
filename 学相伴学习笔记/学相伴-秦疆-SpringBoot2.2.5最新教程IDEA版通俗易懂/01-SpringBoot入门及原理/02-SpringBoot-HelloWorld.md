# SpringBoot-Hello World

*使用SpringBoot框架编写一个hello-world程序*

## 1. 准备工作

我们将学习如何快速的创建一个Spring Boot应用，并且实现一个简单的Http请求处理。通过这个例子对Spring Boot有一个初步的了解，并体验其结构简单、开发快速的特性。

我的环境准备：

- java version "1.8.0_181"
- Maven-3.6.1
- SpringBoot 2.x 最新版

开发工具：

- IDEA

## 2. 创建基础项目

*Spring官方提供了非常方便的工具让我们快速构建应用 , Spring Initializr： https://start.spring.io/*

### 2.1 方式一: **：**使用Spring Initializr 的 Web页面创建项目

1、打开 https://start.spring.io/

2、填写项目信息

3、点击”Generate Project“按钮生成项目；下载此项目

4、解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。

5、如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。

### 2.2 方式二: 使用 IDEA 直接创建项目

1、创建一个新项目

2、选择spring initalizr ， 可以看到默认就是去官网的快速构建工具那里实现

3、填写项目信息

4、选择初始化的组件（初学勾选 Web 即可）

5、填写项目路径

6、等待项目构建成功

 **项目结构分析：**

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

1、程序的主启动类

2、一个 application.properties 配置文件

3、一个 测试类

4、一个 pom.xml

## 3. pom.xml 分析

打开 pom.xml ，看看Spring Boot项目的依赖：

```xml
<!-- 父依赖 --> <parent> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-parent</artifactId> <version>2.2.5.RELEASE</version> <relativePath/> </parent> <dependencies> <!-- web场景启动器 --> <dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-web</artifactId> </dependency> <!-- springboot单元测试 --> <dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-test</artifactId> <scope>test</scope> <!-- 剔除依赖 --> <exclusions> <exclusion> <groupId>org.junit.vintage</groupId> <artifactId>junit-vintage-engine</artifactId></exclusion> </exclusions> </dependency> </dependencies> <build> <plugins> <!-- 打包插件 --> <plugin> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-maven-plugin</artifactId> </plugin> </plugins> </build>
```



## 4. 编写HTTP接口

1、在主程序的同级目录下，新建一个controller包，<u>一定要在同级目录下，否则识别不到</u>

2、在包中新建一个HelloController类 

```java
@RestController public class HelloController { @RequestMapping("/hello") public String hello() { return "Hello World"; } }
```

3、编写完毕后，从主程序启动项目，浏览器发起请求，看页面返回；控制台输出了 Tomcat 访问的端口号！



*简单几步，就完成了一个web接口的开发，SpringBoot就是这么简单。所以我们常用它来建立我们的微服务项目！*

## 5. 将项目打成jar包

*将项目打成jar包，点击 maven的 package*



如果遇到以上错误，可以配置打包时 跳过项目运行测试用例

```xml
<!--在工作中,很多情况下我们打包是不想执行测试用例的 可能是测试用例不完事,或是测试用例会影响数据库数据 跳过测试用例执 --> <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-surefire-plugin</artifactId> <configuration> <!--跳过项目运行测试用例--> <skipTests>true</skipTests> </configuration> </plugin>
```

如果打包成功，则会在target目录下生成一个 jar 包

打成了jar包后，就可以在任何地方运行了！OK `java -jar xxx.jar` 



## 6. SpringBoot彩蛋

如何更改启动时显示的字符拼成的字母，SpringBoot呢？ 也就是 banner 图案

只需一步：到项目下的 resources 目录下新建一个banner.txt 即可。

图案可以到：https://www.bootschool.net/ascii 这个网站生成，然后拷贝到文件中即可！



**SpringBoot这么简单的东西背后一定有故事，我们之后去进行一波源码分析！**





