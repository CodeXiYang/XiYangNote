# JavaWEB

> 视频: https://www.bilibili.com/video/BV1Y7411K7zz
>
> 涵盖JavaWeb核心技术点主要有：
>
> - Servlet程序、Filter过滤器、Listener监听器、jsp页面、EL表达式、JSTL标签库、jQuery框架、Cookie技术、Session会话、JSON使用、Ajax请求，并在讲解知识点过程中会带领大家完成一个书城项目。

## 第 1 章 : HTML与CSS

### 1.1 基础概念

#### 1.1.1 B/S软件的结构

#### 1.1.2 前端的开发流程

#### 1.1.3 网页的组成部分

### 1.2 HTML基础

### 1.2 HTML简介

### 1.3 创建HTML文件

#### 2.3 HTML文件的属性规范

#### 2.4 HTML标签介绍

#### 2.5 HTML常用标签

### 3. CSS基础

#### 3.1 CSS技术介绍

#### 3.2 CSS语法规则

#### 3.3 CSS和HTML的结合方式

#### 3.4 CSS选择器

#### 3.5 CSS常用选择器

---

## 第 2 章: JavaScript

### 1. JavaScript入门基础

#### 1.1 JavaScript介绍

#### 1.2 JavaScript和html代码的结合

#### 1.3 变量

#### 1.4 数组

#### 1.5 函数

#### 1.6 JS中的自定义对象

#### 1.7 JS中的事件

### 2. DOM模型

#### 2.1 Document对象

#### 2.2 Document对象中的方法介绍

#### 2.3 节点的常用属性和方法

---

## 第 3 章: JQuery

### 1. JQuery基础入门

#### 1.1 JQuery介绍

#### 1.2 JQuery的初体验

#### 1.3 JQuery核心函数

#### 1.4 JQuery对象和DOM对象的区分

### 2. JQuery相关操作

#### 2.1 JQuery选择器

#### 2.2 JQuery元素筛选

#### 2.3 JQuery的属性操作

##### JQuery练习一

#### 2.4  DOM的增删改查

##### JQuery练习二

#### 2.5 CSS样式操作

#### 2.6 JQuery动画

#### 2.7 JQuery事件操作

---

## 第 4 章: XML与Tomcat

### 1. XML

#### 1.1 XML简介

#### 1.2 XML语法

#### 1.3 XML解析技术介绍

#### 1.4 DOM4j解析技术

### 2. Tomcat

#### 2.1 JavaWeb的概念

#### 2.2 Web资源的分类

#### 2.3 常用的Web服务器

#### 2.4 Tomcat服务器和Servlet版本的对应关系

#### 2.5 Tomcat的使用

#### 2.6 IDEA整合Tomcat服务器

#### 2.7 IDEA中动态web工程的操作

---

## 第 5 章: Servlet

### 1. Servlet技术

#### 1.1 什么是Servlet

#### 1.2 手动实现Servlet程序

#### 1.3 url地址到Servler程序的访问

#### 1.4 Servlet的生命周期

#### 1.5 GET和POST请求的分发处理

#### 1.6 通过继承HttpServlet实现Servlet程序

#### 1.7 使用IDEA创建Servlet程序

#### 1.8 Servlet类的继承体系

### 2. ServletConfig类

#### 2.1 ServletConfig类的三大作用

### 3. ServletContext类

#### 3.1 什么是ServletContext

#### 3.2 ServletContext类的4个作用

### 4. Http协议

#### 4.1 什么是HTTP协议

#### 4.2 请求的HTTP协议格式

#### 4.3 响应的HTTP协议格式

#### 4.4 常用的响应码说明

#### 4.5 MIME类型说明

### 5. HttpServletRequest类

#### 5.1 HttpServletRequest类有什么作用

#### 5.2 HttpServletRequest类的常用方法

#### 5.3 如何获取请求参数

#### 5.4 doGet请求的中文乱码解决

#### 5.5 POST请求的中文乱码解决

#### 5.6 请求的转发

#### 5.7 base标签的作用

#### 5.8 Web中的相对路径和绝对路径

#### 5.9 web中 / 斜杠的不同含义

### 6. HttpServletResponse类

#### 6.1 HttpServletResponse类的作用

#### 6.2 两个输出流的说明

#### 6.3 如何往客户端回传数据

#### 6.4 响应的乱码解决

#### 6.5 请求重定向

---

## 第 6 章: JSP

### 1. 为什么要学习JSP技术

#### 1. 什么是JSP

#### 2. Servlet程序输出HTML页面

#### 3. 如何创建一个jsp动态页面程序

#### 4. 如何修改jsp文件的默认编码

#### 5. 注意事项

### 2. JSP的运行原理

### 3. JSP的语法

#### 1. jsp文件头部声明介绍

#### 2. jsp中的三种脚本介绍

#### 3. jsp中的注释

### 4. JSP的九大内置对象

### 5. JSP的四大作用域

### 6. JSP中的out输出流和response.getwriter输出流

#### 1. JSP中out和response的writer的区别演示

#### 2. 图解out流和writer流的两个缓冲区如何工作

### 7. JSP的常用标签

#### 1. 静态包含

#### 2. 动态包含

#### 3. 页面转发

#### 4. 静态包含和动态包含的区别

### 8. Listener监听器

#### 1. 什么是Listener监听器

#### 2. ServletContextListener监听器

---

## 第 7 章: EL表达式与JSTL标签库

---

## 第 8 章: Cookie与Session

### 1. Cookie

#### 1. 什么是Cookie?

#### 2. 如何创建Cookie?

#### 3. 服务器3如何获取Cookie?

#### 4. Cookie值得修改

#### 5. 浏览器查看Cookie

#### 6. Cookie生命控制

#### 7. Cookie有效路径Paht的设置

#### 8. Cookie联系-免输入用户名登录

### 2. Session

#### 1. 什么是Session会话

#### 2. 如何创建Session和获取(id号是否为新)

#### 3. Session域数据的存取

#### 4. Session生命周期控制

#### 5. 浏览器和Session之间关联的技术内幕

----

## 第 9 章: Filter过滤器

### 1. Filter什么是过滤器

### 2. Filter的初体验

### 3. Filter的生命周期

### 4. FilterConfig类

### 5. FilterChain过滤器链

### 6. Filter的拦截路径

---

## 第 10 章: JSON与Ajax请求

### 1. JSON

#### 1.1 什么是JSON

#### 1.2 JSON在JavaScript中的使用

#### 1.3 Json在Java中的使用

### 2. AJAX

#### 2.1 什么是AJAX请求

#### 2.2 原生AJAX请求

#### 2.3 JQuery中的AJAX请求

---

## 第 11 章: i18h国际化

### 1. 什么是i18n国际化

### 2. 国际化相关要素介绍

### 3. 国际化资源properties测试

### 4. 通过请求头国际化页面

### 5. 通过显示的选择语言类型进行国际化

### 6. JSTL标签库实现国际化

----

## 书城案例

### 1 阶段:

### 2 阶段:

### 3 阶段:

### 4 阶段:

### 5 阶段:

### 6 阶段:

### 7 阶段:

### 8 阶段:

### 9 阶段:

### 10 阶段: