# HelloSpring

## 1. 导入jar包

*注 : spring 需要导入commons-logging进行日志记录 . 我们利用maven , 他会自动下载对应的依赖项 .*

```xml
<dependency> 
    <groupId>org.springframework</groupId> 
    <artifactId>spring-webmvc</artifactId> 
    <version>5.1.10.RELEASE</version> 
</dependency>
```



## 2. 编写代码

1.  编写一个Hello实体类

   ```java
   public class Hello { 
       private String name; 
       public String getName() { 
           return name; 
       }
       public void setName(String name) { 
           this.name = name; 
       }
       public void show(){ 
           System.out.println("Hello,"+ name );
       }
   }
   ```

2. 编写我们的spring文件 , 这里我们命名为beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?> 
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
          xsi:schemaLocation="http://www.springframework.org/schema/beans 
                              http://www.springframework.org/schema/beans/spring-beans.xsd"> 
       <!--bean就是java对象 , 由Spring创建和管理--> 
       <bean id="hello" class="com.kuang.pojo.Hello"> 
           <property name="name" value="Spring"/> 
       </bean> 
   </beans>
   ```

3. 我们可以去进行测试了 

   ```java
   @Test public void test(){ 
       //解析beans.xml文件 , 生成管理相应的Bean对象 
       ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml"); 
       //getBean : 参数即为spring配置文件中bean的id . 
       Hello hello = (Hello) context.getBean("hello"); 
       hello.show(); 
   }
   ```

   

## 3. 思考

1. Hello 对象是谁创建的 ? 【 hello 对象是由Spring创建的 】

2. Hello 对象的属性是怎么设置的 ? 【hello 对象的属性是由Spring容器设置的 】

   这个过程就叫控制反转 :

   - **控制** : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的
   - **反转** : 程序本身不创建对象 , 而变成被动的接收对象 .

   

依赖注入 : 就是利用set方法来进行注入的.

IOC是一种编程思想，由主动的编程变成被动的接收

可以通过`newClassPathXmlApplicationContext`去浏览一下底层源码 





## 4. 修改案例一

*我们在案例一中， 新增一个Spring配置文件beans.xml*

```xml
<?xml version="1.0" encoding="UTF-8"?> 
    <beans xmlns="http://www.springframework.org/schema/beans" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
        xsi:schemaLocation="http://www.springframework.org/schema/beans 
            http://www.springframework.org/schema/beans/spring-beans.xsd"> 
<bean id="MysqlImpl" class="com.kuang.dao.impl.UserDaoMySqlImpl"/> 
    <bean id="OracleImpl" class="com.kuang.dao.impl.UserDaoOracleImpl"/> 
        <bean id="ServiceImpl" class="com.kuang.service.impl.UserServiceImpl"> 
            <!--注意: 这里的name并不是属性 , 而是set方法后面的那部分 , 首字母小写--> 
            <!--引用另外一个bean , 不是用value 而是用 ref--> 
            <property name="userDao" ref="OracleImpl"/> 
        </bean> 
</beans>
```

测试！

```java
@Test public void test2(){ 
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml"); 
    UserServiceImpl serviceImpl = (UserServiceImpl) context.getBean("ServiceImpl"); 
    serviceImpl.getUser(); 
}
```

*OK , 到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改, 所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 !*







