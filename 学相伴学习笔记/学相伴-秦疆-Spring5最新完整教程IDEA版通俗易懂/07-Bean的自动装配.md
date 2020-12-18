# Bean的自动装配

自动装配是使用spring满足bean依赖的一种方法

spring会在应用上下文中为某个bean寻找其依赖的bean。

Spring中bean有三种装配机制，分别是：

- 在xml中显式配置；
- 在java中显式配置；
- 隐式的bean发现机制和自动装配。

这里我们主要讲第三种：自动化的装配bean。

Spring的自动装配需要从两个角度来实现，或者说是两个操作：

1. **组件扫描(component scanning)**：spring会自动发现应用上下文中所创建的bean； 
2. **自动装配(autowiring)**：spring自动满足bean之间的依赖，也就是我们说的IoC/DI； 

组件扫描和自动装配组合发挥巨大威力，使的显示的配置降低到最少。

**推荐不使用自动装配xml配置 , 而使用注解**



## 1. 测试环境搭建

1. 新建一个项目

2. 新建两个实体类，Cat Dog 都有一个叫的方法

   ```java
   public class Cat { 
       public void shout() { 
           System.out.println("miao~"); 
       } 
   }
   ```

   ```java
   public class Dog { 
       public void shout() { 
           System.out.println("wang~"); 
       } 
   }
   ```

3. 新建一个用户类 User

   ```java
   public class User { 
       private Cat cat;
       private Dog dog;
       private String str;
   }
   ```

4. 编写Spring配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?> 
   <beans xmlns="http://www.springframework.org/schema/beans" 
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
          xsi:schemaLocation="http://www.springframework.org/schema/beans 
                              http://www.springframework.org/schema/beans/spring-beans.xsd"> 
       <bean id="dog" class="com.kuang.pojo.Dog"/>
       <bean id="cat" class="com.kuang.pojo.Cat"/>
       <bean id="user" class="com.kuang.pojo.User">
           <property name="cat" ref="cat"/>
           <property name="dog" ref="dog"/> 
           <property name="str" value="qinjiang"/>
       </bean> 
   </beans>
   ```

5. 测试

   ```java
   public class MyTest {
       @Test 
       public void testMethodAutowire() { 
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           User user = (User) context.getBean("user"); 
           user.getCat().shout(); 
           user.getDog().shout(); 
       } 
   }
   ```

   结果正常输出，环境OK



## 2. byName

**autowire byName (按名称自动装配)**

由于在手动配置xml过程中，常常发生字母缺漏和大小写等错误，而无法对其进行检查，使得开发效率降低。

采用自动装配将避免这些错误，并且使配置简单化。

测试：

1. 修改bean配置，增加一个属性 autowire="byName"

   ```xml
   <bean id="user" class="com.kuang.pojo.User" autowire="byName"> 
       <property name="str" value="qinjiang"/>
   </bean>
   ```

2. 再次测试，结果依旧成功输出！

3. 我们将 cat 的bean id修改为 catXXX

4. 再次测试， 执行时报空指针java.lang.NullPointerException。因为按byName规则找不对应set方法，真正的setCat就没执行，对象就没有初始化，所以调用时就会报空指针错误。

**小结:**

当一个bean节点带有 autowire byName的属性时。

1. 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。 
2. 去spring容器中寻找是否有此字符串名称id的对象。
3. 如果有，就取出注入；如果没有，就报空指针异常。





## 3. byType

**autowire byType (按类型自动装配)**

使用autowire byType首先需要保证：同一类型的对象，在spring容器中唯一。如果不唯一，会报不唯一的异常。

```java
NoUniqueBeanDefinitionException
```

测试：

1. 将user的bean配置修改一下 ： `autowire="byType"`

2. 测试，正常输出

3. 在注册一个cat 的bean对象！

   ```xml
   <bean id="dog" class="com.kuang.pojo.Dog"/> 
   <bean id="cat" class="com.kuang.pojo.Cat"/>
   <bean id="cat2" class="com.kuang.pojo.Cat"/> 
   <bean id="user" class="com.kuang.pojo.User" autowire="byType"> 
       <property name="str" value="qinjiang"/>
   </bean>
   ```

4.  测试，报错：NoUniqueBeanDefifinitionException

5. 删掉cat2，将cat的bean名称改掉！测试！因为是按类型装配，所以并不会报异常，也不影响最后的结果。甚至将id属性去掉，也不影响结果。

**这就是按照类型自动装配！**



## 4. 使用注解

jdk1.5开始支持注解，spring2.5开始全面支持注解。

准备工作： 利用注解的方式注入属性。

1. 在spring配置文件中引入context文件头

   ```xml
   xmlns:context="http://www.springframework.org/schema/context" 
   http://www.springframework.org/schema/context 
   http://www.springframework.org/schema/context/spring-context.xsd
   ```

2. 开启属性注解支持！

   ```xml
   <context:annotation-config/>
   ```

   



### 4.1 @Autowired

@Autowired是按类型自动转配的，不支持id匹配。

需要导入 spring-aop的包！

测试：

1.  将User类中的set方法去掉，使用@Autowired注解

   ```java
   public class User { 
       @Autowired 
       private Cat cat;
       @Autowired 
       private Dog dog;
       private String str;
       public Cat getCat() { 
           return cat; 
       }
       public Dog getDog() {
           return dog; 
       }
       public String getStr() {
           return str;
       } 
   }
   ```

2. 此时配置文件内容

   ```xml
   <context:annotation-config/> 
   <bean id="dog" class="com.kuang.pojo.Dog"/>
   <bean id="cat" class="com.kuang.pojo.Cat"/>
   <bean id="user" class="com.kuang.pojo.User"/>
   ```

   测试，成功输出结果！

【小狂神科普时间】

@Autowired(required=false) 说明： false，对象可以为null；true，对象必须存对象，不能为null。 

```java
//如果允许对象为null，设置required = false,默认为true 
@Autowired(required = false) 
private Cat cat;
```



### 4.2 @Qualifier

@Autowired是根据类型自动装配的，加上@Qualififier则可以根据byName的方式自动装配

@Qualififier不能单独使用。

测试实验步骤：

1. 配置文件修改内容，保证类型存在对象。且名字不为类的默认名字！

   ```xml
   <bean id="dog1" class="com.kuang.pojo.Dog"/>
   <bean id="dog2" class="com.kuang.pojo.Dog"/>
   <bean id="cat1" class="com.kuang.pojo.Cat"/>
   <bean id="cat2" class="com.kuang.pojo.Cat"/>
   ```

2. 没有加Qualififier测试，直接报错

3. 在属性上添加Qualififier注解

   ```java
   @Autowired 
   @Qualifier(value = "cat2") 
   private Cat cat; 
   @Autowired 
   @Qualifier(value = "dog2") 
   private Dog dog;
   ```

    测试，成功输出！

### 4.3 @Resource

@Resource如有指定的name属性，先按该属性进行byName方式查找装配；

其次再进行默认的byName方式进行装配；

如果以上都不成功，则按byType的方式自动装配。

都不成功，则报异常。

1. 实体类：

   ```java
   public class User { 
       //如果允许对象为null，设置required = false,默认为true 
       @Resource(name = "cat2")
       private Cat cat; 
       @Resource 
       private Dog dog;
       private String str;
   }
   ```

2. beans.xml

   ```xml
   <bean id="dog" class="com.kuang.pojo.Dog"/>
   <bean id="cat1" class="com.kuang.pojo.Cat"/> 
   <bean id="cat2" class="com.kuang.pojo.Cat"/> 
   <bean id="user" class="com.kuang.pojo.User"/>
   ```

   测试：结果OK

3. 配置文件2：beans.xml ， 删掉cat2

   ```xml
   <bean id="dog" class="com.kuang.pojo.Dog"/>
   <bean id="cat1" class="com.kuang.pojo.Cat"/>
   ```

4. 实体类上只保留注解

   ```java
   @Resource 
   private Cat cat;
   @Resource 
   private Dog dog;
   ```

   结果：OK

   结论：先进行byName查找，失败；再进行byType查找，成功。



## 5. 小结

**@Autowired与@Resource异同：**

1. @Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。
2. @Autowired默认按类型装配（属于spring规范），默认情况下必须要求依赖对象必须存在，如果
要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我
们想使用名称装配可以结合@Qualifier注解进行使用
3. @Resource（属于J2EE复返），默认按照名称进行装配，名称可以通过name属性进行指定。如果
没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在
setter方法上默认取属性名进行装配。 当找不到与名称匹配的bean时才按照类型进行装配。但是
需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。

**它们的作用相同都是用注解方式注入对象，但执行顺序不同。@Autowired先byType，@Resource先 byName。**





