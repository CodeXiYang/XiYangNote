# MyBatis第一个程序

*思路流程：搭建环境-->导入Mybatis--->编写代码--->测试*



## 1. 搭建实验数据库

```sql
CREATE DATABASE `mybatis`; 
USE `mybatis`; 
DROP TABLE IF EXISTS `user`; 
CREATE TABLE `user` ( 
    `id` int(20) NOT NULL,
    `name` varchar(30) DEFAULT NULL, 
    `pwd` varchar(30) DEFAULT NULL, 
PRIMARY KEY (`id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
insert into `user`(`id`,`name`,`pwd`) values (1,'狂神','123456'),(2,'张 三','abcdef'),(3,'李四','987654');
```



## 2. 导入mybatis相关jar包

```xml
<dependency> 
    <groupId>org.mybatis</groupId> 
    <artifactId>mybatis</artifactId> 
    <version>3.5.2</version> 
</dependency> <dependency> 
    <groupId>mysql</groupId> 
    <artifactId>mysql-connector-java</artifactId> 
    <version>5.1.47</version> 
</dependency>
```



## 3. 编写mybatis核心配置文件

*官方文档*

```xml
<?xml version="1.0" encoding="UTF-8" ?> 
<!DOCTYPE configuration 
		PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
		"http://mybatis.org/dtd/mybatis-3-config.dtd"> 
<configuration> 
    <environments default="development"> 
        <environment id="development"> 
            <transactionManager type="JDBC"/> 
            <dataSource type="POOLED"> 
                <property name="driver" value="com.mysql.jdbc.Driver"/> 
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis? useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/> 
                <property name="username" value="root"/> 
                <property name="password" value="123456"/> 
            </dataSource> 
        </environment> 
    </environments> 
    <mappers> 
        <mapper resource="com/kuang/dao/userMapper.xml"/> 
    </mappers> 
</configuration>
```



## 4. 编写mybatis工具类

```java
public class MybatisUtils { 
    private static SqlSessionFactory sqlSessionFactory; 
    static { 
        try {
            String resource = "mybatis-config.xml"; 
            InputStream inputStream = Resources.getResourceAsStream(resource); 
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream); 
        } catch (IOException e) {
            e.printStackTrace(); 
        } 
    }
    //获取SqlSession连接 
    public static SqlSession getSession(){ 
        return sqlSessionFactory.openSession(); 
    } 
}
```



## 5. 创建实体类

```java
public class User { 
    private int id; //id 
    private String name; //姓名 
    private String pwd; //密码 
    //构造,有参,无参 
    //set/get 
    //toString() 
}
```



## 6. 编写mapper接口

```java
public interface UserMapper { 
    List<User> selectUser(); 
}
```



## 7. 编写mapper.xml文件

namespace 十分重要，不能写错！

```xml
<?xml version="1.0" encoding="UTF-8" ?> 
<!DOCTYPE mapper 
		PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
		"http://mybatis.org/dtd/mybatis-3-mapper.dtd"> 
<mapper namespace="com.kuang.dao.UserMapper"> 
    <select id="selectUser" resultType="com.kuang.pojo.User"> select * from user </select> 
</mapper>
```



## 8. 编写测试类并运行测试

Junit 包测试

```java
public class MyTest { 
    @Test public void selectUser() { 
        SqlSession session = MybatisUtils.getSession(); 
        //方法一: 
        //List<User> users = session.selectList("com.kuang.mapper.UserMapper.selectUser"); 
        //方法二: 
        UserMapper mapper = session.getMapper(UserMapper.class); 
        List<User> users = mapper.selectUser(); 
        for (User user: users){
            System.out.println(user); 
        }
        session.close(); 
    } 
}
```

运行测试

```java

```



补充说明: *Maven静态资源过滤问题*

```xml
<resources> 
    <resource> 
    <directory>src/main/java</directory> 
        <includes> 
            <include>**/*.properties</include> 
            <include>**/*.xml</include> 
        </includes> 
        <filtering>false</filtering> 
    </resource> 
    <resource> 
        <directory>src/main/resources</directory> 
        <includes> 
            <include>**/*.properties</include> 
            <include>**/*.xml</include> 
        </includes> 
        <filtering>false</filtering> 
    </resource> 
</resources>
```







