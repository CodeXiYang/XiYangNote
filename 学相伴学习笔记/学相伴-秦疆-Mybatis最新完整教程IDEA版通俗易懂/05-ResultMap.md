# ResultMap

*ResultMap用于解决属性名和字段名不一致的问题*



## 1. 查询为null的问题

1.  查看之前的数据库的字段名

2.  Java中的实体类设计

   ```java
   public class User { 
       private int id; //id 
       private String name; //姓名 
       private String password; //密码和数据库不一样！ 
       //构造 
       //set/get 
       //toString() 
   }
   ```

3. 接口

   ```java
   //根据id查询用户 
   User selectUserById(int id);
   ```

4. mapper映射文件

   ```xml
   <select id="selectUserById" resultType="user"> 
       select * from user where id = #{id} 
   </select>
   ```

5. 测试

   ```java
   @Test public void testSelectUserById() { 
       SqlSession session = MybatisUtils.getSession(); 
       //获取SqlSession连接 
       UserMapper mapper = session.getMapper(UserMapper.class); 
       User user = mapper.selectUserById(1); 
       System.out.println(user); 
       session.close(); 
   }
   ```

**结果**: 

- `User{id=1, name='狂神', password='null'}`
- 查询出来发现 password 为空 . 说明出现了问题！

**分析：**

- `select * from user where id = #{id}` 可以看做`select id,name,pwd from user where id = #{id}`
- mybatis会根据这些查询的列名(会将列名转化为小写,数据库不区分大小写) , 去对应的实体类中查找相应列名的set方法设值 , 由于找不到setPwd() , 所以password返回null ; 【自动映射】



## 2. 解决方案

方案一：为列名指定别名 , 别名和java实体类的属性名一致 

```xml
<select id="selectUserById" resultType="User"> 
    select id , name , pwd as password from user where id = #{id} 
</select>
```



方案二：使用结果集映射->ResultMap 【推荐】

```xml
<resultMap id="UserMap" type="User"> 
    <!-- id为主键 --> 
    <id column="id" property="id"/> 
    <!-- column是数据库表的列名 , property是对应实体类的属性名 --> 
    <result column="name" property="name"/> 
    <result column="pwd" property="password"/> 
</resultMap> 
<select id="selectUserById" resultMap="UserMap"> 
    select id , name , pwd from user where id = #{id} 
</select>
```



## 3. ResultMap

### 3.1 自动映射

- resultMap 元素是 MyBatis 中最重要最强大的元素。它可以让你从 90% 的 JDBCResultSets 数据提取代码中解放出来。

- 实际上，在为一些比如连接的复杂语句编写映射代码的时候，一份 resultMap 能够代替实现同等功能的长达数千行的代码。

- ResultMap 的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只需要描述它们的关系就行了。

你已经见过简单映射语句的示例了，但并没有显式指定 resultMap 。比如：

```xml
<select id="selectUserById" resultType="map"> 
    select id , name , pwd from user where id = #{id} 
</select>
```

上述语句只是简单地将所有的列映射到 HashMap 的键上，这由 resultType 属性指定。虽然在大部分情况下都够用，但是 HashMap 不是一个很好的模型。你的程序更可能会使用 JavaBean 或POJO（Plain Old Java Objects，普通老式 Java 对象）作为模型。

ResultMap 最优秀的地方在于，虽然你已经对它相当了解了，但是根本就不需要显式地用到他们。

### 3.2 手动映射

1. 返回值类型为resultMap

   ```xml
   <select id="selectUserById" resultMap="UserMap"> 
       select id , name , pwd from user where id = #{id} 
   </select>
   ```

2. 编写resultMap，实现手动映射！

   ```xml
   <resultMap id="UserMap" type="User"> 
       <!-- id为主键 --> 
       <id column="id" property="id"/> 
       <!-- column是数据库表的列名 , property是对应实体类的属性名 --> 
       <result column="name" property="name"/> 
       <result column="pwd" property="password"/> 
   </resultMap>
   ```

如果世界总是这么简单就好了。但是肯定不是的，数据库中，存在一对多，多对一的情况，我们之后会使用到一些高级的结果集映射，association，collection这些，我们将在之后讲解，今天你们需要把这些知识都消化掉才是最重要的！理解结果集映射的这个概念！









