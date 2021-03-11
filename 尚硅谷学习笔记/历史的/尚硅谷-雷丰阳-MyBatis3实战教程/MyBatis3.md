# MyBatis3

> 课程名称: [尚硅谷MyBatis实战教程全套完整版](https://www.bilibili.com/video/BV1mW411M737)
>
> 课程概述:
>
> - MyBatis配置文件编写，MyBatis动态SQL，MyBatis缓存机制，MyBatis-Spring整合，MyBatis逆向工程，
> - MyBatis高级内容（MyBatis源码解析，MyBatis单/多插件运行机制，MyBatis四大对象工作原理，自定义TypeHandler、MyBatis存储过程&游标处理等）。
> - 视频中会在重要的地方对比MyBatis操作MySQL以及Oracle之间的差异性。
>
> 讲述人: 尚硅谷-雷丰阳

## 第 1 章: MyBatis简介

> MyBatis就是iBatis

## 第 2 章: MyBatis-HelloWorld

## 第 3 章: MyBatis-全局配置文件

## 第 4 章:MyBatis-映射文件

## 第 5 章: MyBatis-动态SQL

## 第 6 章: MyBatis-缓存机制

## 第 7 章: MyBatis-Spring整合

## 第 8 章: MyBatis-逆向工程

> MyBatis逆向工程指的是通过数据表自动渲染出entity,service,dao,mapperxml等文件
>
> mybaits中通过`Generator`类来实现的

### 8.1 MBG简介

MBG是MyBatis
Generator的缩写，是一个专门为MyBatis框架使用者定制的代码生成器，可以快速的根据表生成对应的映射文件，接口，以及bean类。支持基本的增删改查，以及QBC风格的条件查询。但是表连接、存储过程等这些复杂sql的定义需要我们手工编写

MBG官方文档地址: http://www.mybatis.org/generator/

MBG官方工程地址: https://github.com/mybatis/generator/releases

### 8.2 MBG使用

**MBG依赖**

```xml
<dependencies>
    <!--mysql-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
    <!--mybaits 核心框架-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>
    <!--mybatis generator 生成器-->
    <dependency>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-core</artifactId>
        <version>1.3.2</version>
    </dependency>
</dependencies>
```

**使用步骤:**

1. 编写MBG的配置文件(重要几处配置 )
    - `jdbcConnection`配置数据库连接信息
    - `javaModelGenerator`配置javaBean的生成策略
    - `sqlMapGenerator` 配置sql映射文件生成策略
    - `javaClientGenerator`配置Mapper接口的生成策略
    - `table` 配置要逆向解析的数据表
        - `tableName`：表名
        - `domainObjectName`：对应的javaBean名
2. 运行代码生成器生成代码

**注意:**

Context标签

- **targetRuntime=“MyBatis3“**可以生成带条件的增删改查
- **targetRuntime=“MyBatis3Simple“**可以生成基本的增删改查
- 如果再次生成，建议将之前生成的数据删除，避免xml向后追加内容出现的问题。

### 8.3 MBG配置文件

```xml
<generatorConfiguration> <context id="DB2Tables" targetRuntime="MyBatis3">
    //数据库连接信息配置
    <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                    connectionURL="jdbc:mysql://localhost:3306/bookstore0629"
                    userId="root" password="123456">
    </jdbcConnection>
    //javaBean的生成策略
    <javaModelGenerator targetPackage="com.atguigu.bean" targetProject=".\src"> <property name="enableSubPackages" value="true" />
        <property name="trimStrings" value="true" />
    </javaModelGenerator>
    //映射文件的生成策略
    <sqlMapGenerator targetPackage="mybatis.mapper" targetProject=".\conf"> <property name="enableSubPackages" value="true" />
    </sqlMapGenerator>
    //dao接口java文件的生成策略
    <javaClientGenerator type="XMLMAPPER" targetPackage="com.atguigu.dao"
                         targetProject=".\src"> <property name="enableSubPackages" value="true" />
    </javaClientGenerator>
    //数据表与javaBean的映射
    <table tableName="books" domainObjectName="Book"></table>
    </context>
</generatorConfiguration>
```

### 8.4 生成器代码

```java
public static void main(String[] args) throws Exception {
    List<String> warnings = new ArrayList<String>();
    boolean overwrite = true;
    File configFile = new File("mbg.xml");
    ConfigurationParser cp = new ConfigurationParser(warnings);
    Configuration config = cp.parseConfiguration(configFile);
    DefaultShellCallback callback = new DefaultShellCallback(overwrite);
    MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
                                                             callback, warnings);
    myBatisGenerator.generate(null);
}
```

### 8.5 测试查询

测试查询：QBC风格的带条件查询

```java
@Test
public void test01(){
    SqlSession openSession = build.openSession();
    DeptMapper mapper = openSession.getMapper(DeptMapper.class);
    DeptExample example = new DeptExample();
    //所有的条件都在example中封装
    Criteria criteria = example.createCriteria();
    //select id, deptName, locAdd from tbl_dept WHERE 
    //( deptName like ? and id > ? ) 
    criteria.andDeptnameLike("%部%");
    criteria.andIdGreaterThan(2);
    List<Dept> list = mapper.selectByExample(example);
    for (Dept dept : list) {
        System.out.println(dept);
    } 
}
```

## 第 9 章: MyBaits-工作原理

## 第 10 章: MyBatis-插件开发