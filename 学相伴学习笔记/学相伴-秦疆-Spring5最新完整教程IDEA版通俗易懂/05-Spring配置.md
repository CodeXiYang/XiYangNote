# Spring配置

## 1. 别名

*alias 设置别名 , 为bean设置别名 , 可以设置多个别名*

```xml
<!--设置别名：在获取Bean的时候可以使用别名获取--> 
<alias name="userT" alias="userNew"/>
```



## 2. Bean配置

```xml
<!--bean就是java对象,由Spring创建和管理--> 
<!--id 是bean的标识符,要唯一,
如果没有配置id,name就是默认标识符 
如果配置id,又配置了name,那么name是别名 
name可以设置多个别名,可以用逗号,分号,空格隔开 
如果不配置id和name,可以根据applicationContext.getBean(.class)获取对象; 
class是bean的全限定名=包名+类名 --> 
<bean id="hello" name="hello2 h2,h3;h4" class="com.kuang.pojo.Hello"> 
    <property name="name" value="Spring"/> 
</bean>
```



## 3. import

*团队的合作通过import来实现 .*

```xml
<import resource="{path}/beans.xml"/>
```

