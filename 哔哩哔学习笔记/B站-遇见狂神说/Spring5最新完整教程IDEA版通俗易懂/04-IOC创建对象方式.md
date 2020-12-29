# IOC创建对象方式



## 1. 通过无参构造方法来创建

1. User.java

   ```java
   public class User { 
       private String name; public User() { 
           System.out.println("user无参构造方法"); 
       }
       public void setName(String name) { 
           this.name = name; }public void show(){ 
           System.out.println("name="+ name ); 
       } 
   }
   ```

2. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?> 
   <beans xmlns="http://www.springframework.org/schema/beans" 
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
          xsi:schemaLocation="http://www.springframework.org/schema/beans 
                              http://www.springframework.org/schema/beans/spring-beans.xsd"> 
       <bean id="user" class="com.kuang.pojo.User"> 
           <property name="name" value="kuangshen"/> 
       </bean> 
   </beans>
   ```

3. 测试类

   ```java
   @Test 
   public void test(){ 
       ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml"); 
       //在执行getBean的时候, user已经创建好了 , 通过无参构造 
       User user = (User) context.getBean("user"); 
       //调用对象的方法 . 
       user.show(); 
   }
   ```

   **结果可以发现，在调用show方法之前，User对象已经通过无参构造初始化了！**



## 2. 通过有参构造方法来创建

1. UserT . java

   ```java
   public class UserT { 
       private String name; 
       public UserT(String name) { 
           this.name = name;
       }
       public void setName(String name) { 
           this.name = name; 
       }
       public void show(){ 
           System.out.println("name="+ name ); 
       } 
   }
   ```

2. beans.xml 有三种方式编写

   ```xml
   <!-- 第一种根据index参数下标设置 --> 
   <bean id="userT" class="com.kuang.pojo.UserT"> 
       <!-- index指构造方法 , 下标从0开始 --> 
       <constructor-arg index="0" value="kuangshen2"/> 
   </bean>
   ```

   ```xml
   <!-- 第二种根据参数名字设置 --> 
   <bean id="userT" class="com.kuang.pojo.UserT"> 
       <!-- name指参数名 --> 
       <constructor-arg name="name" value="kuangshen2"/> 
   </bean>
   ```

   ```xml
   <!-- 第三种根据参数类型设置 --> 
   <bean id="userT" class="com.kuang.pojo.UserT"> 
       <constructor-arg type="java.lang.String" value="kuangshen2"/> 
   </bean>
   ```

3. 测试

   ```java
   @Test 
   public void testT(){ 
       ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml"); 
       UserT user = (UserT) context.getBean("userT"); 
       user.show(); 
   }
   ```

**结论**：在配置文件加载的时候。其中管理的对象都已经初始化了！